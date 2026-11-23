---
name: revisora-coesao
description: Revisa coesão e encapsulamento em código de produção de qualquer linguagem ou paradigma (Tell Don't Ask, Lei de Demeter, feature envy/inveja de funcionalidade). Use ao revisar código que orquestra regras sobre dados de domínio, ou quando houver cadeias de acesso (train wreck), regra de negócio decidida fora do dono do dado ou cálculo feito sobre dados de outra unidade.
---

# Revisora — Coesão & Encapsulamento (Tell, Don't Ask / Demeter)

Skill focada em **decisões de design** que poderiam aumentar a coesão movendo comportamento
para o **dono do dado**. O sintoma recorrente é uma unidade de código que **lê** o estado de um
colaborador, **decide/calcula por fora** e, com isso, **vaza a estrutura interna** desse
colaborador — em vez de simplesmente **mandar uma mensagem** e deixar o dono do dado responder.
Vale para qualquer linguagem e paradigma: coesão não depende de a linguagem ter classes.

Princípio-guia (Lei de Demeter): a partir de um colaborador, navegue **no máximo um nível**. Você
pode falar com um colaborador direto e mandar **uma** mensagem para ele. Não saia "pulando" de
estrutura em estrutura puxando dados para tomar a decisão lá fora.

## Vocabulário

- **Dono do dado**: quem define o dado e deveria concentrar as regras sobre ele. Em OO é a
  classe; em Go, Rust ou C é a struct/tipo com seus métodos ou funções associadas; em linguagens
  funcionais (Elixir, Clojure, Haskell, F#) é o módulo que define o tipo. Quando a entidade é
  representada por um mapa/dicionário, o dono é o módulo que a constrói e concentra suas funções.
- **Leitura de estado alheio**: qualquer acesso, feito de fora do dono, à estrutura do dado.
  Getter é só um caso; também contam propriedade, campo público, acesso por chave em
  mapa/dicionário que representa uma entidade e desestruturação/pattern matching sobre a
  estrutura de um tipo definido em outro módulo.
- **Mensagem**: pedir o resultado ao dono — chamada de método do tipo dono
  (`produto.aceitaQuantidade(q)`) ou de função do módulo dono
  (`Produto.aceita_quantidade?(produto, q)`).
- **Unidade**: onde o código revisado mora — classe, módulo, arquivo de funções.
- **Colaborador direto**: campo/atributo da própria unidade, parâmetro da função/método ou valor
  criado ali dentro.

## Exemplo canônico

Os exemplos desta skill usam um domínio de pedidos: um `Pedido` tem `Item`s, cada `Item` aponta
para um `Produto`, e o `Produto` define quantas unidades dele cabem num mesmo pedido. O exemplo
abaixo está em Java apenas como ilustração; o raciocínio é o mesmo em qualquer linguagem.

Trecho que **viola** (a regra de "quantas unidades o produto aceita por pedido" mora fora do produto):

```java
if (quantidade > item.getProduto().getQuantidadeMaximaPorPedido()) {
    throw new QuantidadeInvalidaException(
            "A quantidade excede o maximo permitido para o produto");
}
```

Trecho que **respeita** (o produto decide; o chamador navega um nível e manda uma mensagem):

```java
if (!item.getProduto().aceitaQuantidade(quantidade)) {
    throw new QuantidadeInvalidaException(
            "A quantidade excede o maximo permitido para o produto");
}
```

A correção move a comparação para dentro de `Produto`:

```java
public boolean aceitaQuantidade(int quantidade) {
    return quantidade >= 1 && quantidade <= this.quantidadeMaximaPorPedido;
}
```

Ganhos: o invariante "1..máximo" passa a ter **um único dono**; a leitura externa de
`quantidadeMaximaPorPedido` deixa de ser necessária para essa decisão; qualquer mudança na
regra acontece num só lugar.

### Como o mesmo sintoma aparece em outros estilos

| Estilo | Sintoma | Correção |
|---|---|---|
| Getters encadeados (Java, C#, Kotlin) | `if (quantidade > item.getProduto().getQuantidadeMaximaPorPedido())` | Método no dono: `item.getProduto().aceitaQuantidade(quantidade)` |
| Propriedades/campos (Python, TypeScript, Go, Rust) | `if quantidade > item.produto.quantidade_maxima_por_pedido` | Método no tipo dono: `item.produto.aceita_quantidade(quantidade)` |
| Optional chaining (TypeScript, C#, Kotlin, Swift) | `item?.produto?.categoria?.nome` — o `?.` não muda a contagem de níveis | Acessor delegado (`item.nomeDaCategoria()`) se for só leitura; comportamento no dono se alimentar decisão |
| Funcional (Elixir, Clojure, Haskell, F#) | Função do módulo `Carrinho` decide com campo de `Produto`, struct de outro módulo: `if quantidade > item.produto.quantidade_maxima_por_pedido` (ou o mesmo campo extraído por pattern matching) | Função no módulo dono: `Produto.aceita_quantidade?(item.produto, quantidade)` |

## Escopo

- Código de **produção**, em qualquer linguagem: unidades que definem dados de domínio (entidades,
  value objects, structs, records) e unidades que orquestram regras sobre eles (services, casos de
  uso, handlers/controllers, módulos de funções).
- Arquivos passados pelo usuário ou alterados na branch/commit atual.
- **Fora de escopo**: arquivos de teste (pela convenção da linguagem/projeto), configuração e
  infraestrutura, interfaces de acesso a dados e clientes HTTP.

## Princípio de bloqueio

Cada achado sai com uma severidade: **VIOLAÇÃO** (bloqueante — a mudança não deveria ser aceita
sem a correção) ou **OBSERVAÇÃO** (sugestão, não bloqueia). As checagens marcadas como DURA geram
VIOLAÇÃO.

- **É bloqueante** quando uma regra/decisão de negócio sobre um dado alheio mora fora do dono
  desse dado (regra 1), quando há navegação além de um nível dentro de lógica de decisão/cálculo
  (regra 2 em contexto de lógica) ou quando há inveja de funcionalidade clara (regra 3).
- **Não é bloqueante** (observação) a navegação além de um nível usada apenas para **leitura**
  (montar DTO/payload de saída, log, mensagem) — estruturas de saída podem extrair dados de
  domínio. Aqui só sugerimos um acessor delegado.
- **Na dúvida entre "vaza estado e decide fora" e "apenas lê para reexpor", trate como observação**
  e não bloqueie. O objetivo desta revisão é pegar o caso nítido do exemplo canônico, não policiar
  toda leitura de dado.

## Checagens

### 1. Tell, Don't Ask — decisão sobre estado alheio (DURA)

- [ ] Uma decisão de negócio (condicional, comparação relacional, validação, escolha de fluxo,
  guarda de função) é tomada a partir de **valor obtido por leitura de estado alheio**, quando
  essa decisão poderia ser um **comportamento no dono do dado**.
- Sinal: `if (x > obj.getLimite())`, `if obj.status == ATIVO and obj.saldo > 0`,
  `if (item.getProduto().getQuantidadeMaximaPorPedido() < n)`,
  `if n > item.produto.quantidade_maxima_por_pedido`.
- Correção: criar o comportamento no dono — método do tipo (`obj.suportaX(...)`,
  `obj.estaAtivoComSaldo()`, `produto.aceitaQuantidade(n)`) ou função do módulo dono
  (`Produto.aceita_quantidade?(produto, n)`) — e reduzir o uso externo a **uma mensagem**.
- **Não é violação**: decisão sobre estado **da própria unidade** (`this`/`self`, ou dado de um
  tipo definido no próprio módulo), sobre parâmetro primitivo, ou sobre valor criado na própria
  função/método.

### 2. Lei de Demeter — navegação além de um nível (DURA em lógica; OBSERVAÇÃO em leitura pura)

- [ ] Cadeia que navega **dois ou mais níveis** em estado alheio. **Nível** é cada leitura de
  estado na cadeia: a segunda leitura já é feita sobre o valor **devolvido** pela primeira. A
  contagem não depende da sintaxe:
  - chamada de método: `a.getB().getC()`
  - propriedade/campo: `a.b.c`
  - acesso por chave: `a["b"]["c"]`, `get_in(a, [:b, :c])`
  - optional chaining: `a?.b?.c`
  - desestruturação/pattern matching aninhado: `const { b: { c } } = a`, `%{b: %{c: c}} = a`
- Guardar o valor intermediário numa variável local não zera a contagem: `b = a.b` seguido de
  `b.c` continua sendo dois níveis.
- Permitido (um nível): uma mensagem ou uma leitura sobre um colaborador direto —
  `colaborador.fazAlgo(...)`, `colaborador.getX()`, `colaborador.x`. Também é permitido ler um
  nível e mandar uma **mensagem** ao valor obtido (`item.getProduto().aceitaQuantidade(q)`,
  `Produto.aceita_quantidade?(item.produto, q)`): mensagem ao dono não conta como leitura de estado.
- Sinal de violação: `item.getProduto().getCategoria().getNome()`, `item.produto.categoria.nome`
  (produto → categoria → nome).
- Classifique:
  - **DURA** quando a cadeia alimenta **decisão ou cálculo** (control flow, comparação, regra).
  - **OBSERVAÇÃO** quando a cadeia só **lê para reexpor** (montagem de DTO/payload de saída, log,
    corpo de e-mail/mensagem). Sugira um acessor delegado no intermediário
    (ex.: `item.nomeDaCategoria()` delegando para produto/categoria), mas **não trate como
    bloqueante**.

### 3. Inveja de funcionalidade — lógica ou derivação sobre dados de outro dono (DURA)

- [ ] Uma função/método monta uma sequência de passos/cálculo usando **predominantemente leituras
  de estado de outro dono** (vários `outro.getA()`, `outro.b`, `outro[:c]` combinados numa
  conta/regra), em vez de pedir o resultado ao dono.
- Sinal (cálculo): total/desconto/elegibilidade feito fora do dono lendo 2+ dados dele.
- [ ] **Projeção/derivação de coleção alheia**: iterar uma coleção **exposta por um colaborador**
  (campo/atributo da própria unidade ou parâmetro) e, no corpo, projetar leituras dos elementos para
  montar outra coleção/agregado. Vale para qualquer forma de iteração ou pipeline: laço `for`,
  stream, LINQ, comprehension, `map`/`filter`/`reduce`, operador pipe. A coleção é estado interno do
  colaborador; a regra "como derivar X a partir dos meus itens" deveria morar nele.
- Sinal (projeção):
  `request.getItens().stream().map(ItemPedidoRequest::getCodigoProduto).collect(...)`,
  `sum(i.quantidade for i in pedido.itens)`,
  `pedido.itens |> Enum.map(& &1.valor) |> Enum.sum()`,
  `for _, i := range pedido.Itens { total += i.Valor }`.
- Correção: mover a derivação para o dono e expor o **resultado pronto** (método do tipo ou função
  do módulo dono). No primeiro sinal, em que `request` é um `NovoPedidoRequest`, criar
  `NovoPedidoRequest.codigosDosProdutos()` (que internamente percorre os seus `itens`) e trocar o
  trecho externo por `List<String> codigosDosProdutos = request.codigosDosProdutos();`. Nos demais:
  `pedido.quantidade_total()`, `Pedido.total(pedido)`, `pedido.Total()`. Mesma família de um
  `request.toModel()`: o próprio request entrega o resultado pronto a partir do seu estado.
- **Não é violação**: a fonte da iteração é uma **variável local** (que não seja só um apelido para
  a coleção do colaborador), estado da própria unidade (`this`/`self`) ou um **parâmetro que já é a
  própria coleção** (ex.: uma lista de códigos recebida por argumento) — aí não há coleção alheia
  sendo vazada.

### 4. Falsos positivos a evitar (NÃO são violação)

- **APIs fluentes / builders / pipelines** em que cada passo devolve o próprio valor ou um wrapper:
  method chaining (`logger.atInfo().addKeyValue("pedidoId", id).log("Pedido criado")`,
  `StringBuilder`), query builders e query DSLs, encadeamento de Optional/Result/Maybe
  (`Optional.map().orElse(...)`, `resultado.map(...).unwrap_or(...)`), operador pipe entre funções
  (`pedido |> Pedido.aplica_cupom(cupom) |> Pedido.fecha()`). Encadeamento aqui não é navegação de
  Demeter **por si só**.
  - **Exceção (vira regra 3, é violação)**: quando a **fonte** da iteração/pipeline é uma coleção
    exposta por um colaborador (`colaborador.getColecao()`, `colaborador.colecao`) e o corpo projeta
    leituras dos elementos para montar outra coleção/agregado. Isso é inveja de funcionalidade, não
    estilo. Pipelines cuja fonte é variável local, estado da própria unidade ou um parâmetro que já
    é a própria coleção continuam livres.
- **Navegação de um único nível** para mandar uma mensagem a um colaborador direto
  (`item.getProduto().aceitaQuantidade(...)`, `Produto.aceita_quantidade?(item.produto, ...)`) —
  esse é justamente o alvo correto.
- **Identificador / chave para persistência ou referência** (`repo.findById(x.getId())`,
  `Repo.get(Produto, item.produto_id)`).
- **Estruturas de saída lendo** dados de domínio para montar a resposta (vira observação na
  regra 2, não DURA).
- **Acesso a estado da própria unidade**, inclusive por acessor interno.

## Tipos que são dados puros por design

DTOs, records/data classes sem comportamento, structs de transporte e payloads de
request/response seguem os mesmos critérios, sem regra própria:

- **Estrutura de saída lendo domínio** (DTO de resposta, payload, view): leitura para reexpor.
  Um nível é livre; cadeia de dois ou mais níveis é OBSERVAÇÃO na regra 2, nunca DURA.
- **Derivação sobre os dados do próprio tipo**: mora nele, mesmo que ele seja "só dados". A lista
  de códigos de produto de um `NovoPedidoRequest` é entregue pelo request
  (`request.codigosDosProdutos()` ou a função equivalente no módulo que define o request), não
  montada por quem o recebe (regra 3).
- Fora esses dois casos, não há isenção por o tipo ser "só dados": as regras 1 a 3 se aplicam como
  a qualquer outro dono.

## Formato do relatório

Para cada achado:

```
<arquivo>:<linha>
Categoria: Coesão
Regra: <1 | 2 | 3>
Severidade: <VIOLAÇÃO | OBSERVAÇÃO>
Trecho:
    <2-5 linhas>
Diagnóstico: <que estado alheio é lido / quantos níveis navega / qual regra está fora do dono>
Correção sugerida: <comportamento a criar no dono do dado (método do tipo ou função do módulo
    dono) e como o chamador fica — ex.: "criar Produto.aceitaQuantidade(int) e trocar a
    comparação por item.getProduto().aceitaQuantidade(quantidade)" ou "criar
    Produto.aceita_quantidade?/2 e chamar Produto.aceita_quantidade?(item.produto, quantidade)">
```

Se nenhuma violação: **"Coesão: nenhuma decisão sobre estado alheio encontrada."**

## Princípios

- A regra 1 é o coração da skill: **regra de negócio sobre o dado X mora no dono de X**. O exemplo
  da quantidade máxima por pedido é o caso de referência.
- Demeter (regra 2) tolera **um** nível de navegação. Dois ou mais níveis em lógica de decisão são
  bloqueantes; em leitura pura para saída/log viram observação (estruturas de saída podem extrair
  dados de domínio).
- Prefira sempre a correção que **aumenta coesão** (comportamento no dono) à que só "esconde" a
  cadeia atrás de mais um acessor delegado — embora o acessor delegado já seja melhor que o train
  wreck.
- **Não bloqueie por estilo**: APIs fluentes, pipelines e builders não são violação de Demeter —
  **exceto** o caso da regra 3 em que a iteração/pipeline percorre uma coleção exposta por um
  colaborador para derivar outra coleção (aí a inveja de funcionalidade é bloqueante, seja qual for
  a construção usada).
- CDD (complexidade por unidade) é da `revisora-cdd`; qualidade de teste é da `revisora-testes`.
  Esta skill só olha **coesão/encapsulamento**.
