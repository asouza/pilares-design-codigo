---
name: revisora-cdd
description: Use quando for preciso revisar a complexidade cognitiva de código, em qualquer linguagem, sob a ótica de Cognitive-Driven Development (CDD) — pedidos para contar ICP, checar o limite cognitivo de classes, módulos ou arquivos, apontar unidades grandes ou difíceis de entender que pedem extração, ou revisar código de um projeto que tem um `cdd.json`.
---

# Revisora — Cognitive-Driven Development (CDD)

CDD limita os **ICP (Intrinsic Complexity Points)** de cada unidade de implementação, com base na Teoria da Carga Cognitiva: a memória de trabalho de quem lê o código é limitada, e uma unidade que passa do limite pede extração. O time combina o que conta como ICP e qual é o limite (Souza & Pinto, "Toward a Definition of Cognitive-Driven Development", ICSME 2020).

Esta skill conta o ICP de cada unidade, em qualquer linguagem de programação, e sinaliza as que passam do limite. Ela **não inventa** o que conta como ponto: lê as regras do `cdd.json` do projeto.

## Pré-requisito: arquivo `cdd.json`

**Qual arquivo vale.** Para cada arquivo revisado vale o `cdd.json` mais próximo, subindo a árvore de diretórios a partir dele até a raiz do repositório (ou do diretório de trabalho, se não houver repositório). Um repositório com vários subprojetos pode ter um `cdd.json` em cada um. Se o usuário indicar um caminho, vale esse para todos os arquivos.

**Se não existir.** Arquivo sem `cdd.json` aplicável não é revisado: não use defaults silenciosamente, porque o cálculo precisa ser auditável. Se nenhum arquivo do escopo tem `cdd.json`, interrompa a execução e peça o arquivo. Se só alguns não têm, revise os demais e liste esses em "Não revisados". Nos dois casos você pode oferecer, depois do relatório, um rascunho: o ponto de partida padrão é `exemplos/cdd.json`, pronto para copiar para a raiz do projeto; como alternativa, há um template por linguagem em `exemplos/` (lista abaixo). Os arquivos que dependem do rascunho só são contados depois que o `cdd.json` existir no projeto e o usuário aprovar o conteúdo.

### Campos

O schema completo está em `cdd.schema.json` (JSON Schema draft 2020-12), ao lado deste arquivo. O projeto pode apontar para ele com `"$schema"` no próprio `cdd.json` para ter validação no editor.

| Campo | Obrigatório | Significado |
|---|---|---|
| `unidade` | sim | `"arquivo"` ou `"tipo"`. Define o que é uma unidade (ver abaixo). |
| `limiteIcpPorUnidade` | sim | Inteiro ≥ 1. Teto de ICP por unidade. |
| `limiteIcpPorUnidadeDeTeste` | não | Inteiro ≥ 1. Teto para unidades de teste. Se ausente, vale `limiteIcpPorUnidade`. |
| `testes` | condicional | Globs que identificam arquivos de teste. Sem o campo, todo arquivo é de produção. |
| `ignorar` | não | Globs de arquivos que não entram na contagem. |
| `itens` | sim | Lista do que conta. Cada item tem `id` (único, `^[a-z][a-z0-9-]*$`), `descricao` (texto livre lido pelo revisor), `pontos` (inteiro ≥ 0, por ocorrência) e, opcionalmente, `aplicaSomenteEm`: `"producao"`, `"teste"` ou `"ambos"` (padrão). |

Regras que tornam o arquivo **inválido**:

- Campo desconhecido ou campo obrigatório ausente.
- `limiteIcpPorUnidadeDeTeste` presente, ou algum item com `aplicaSomenteEm` diferente de `"ambos"`, sem o campo `testes`. Não há padrão embutido para reconhecer teste.
- Dois itens com o mesmo `id` (o schema não consegue expressar esta regra; confira à parte).

Globs de `testes` e `ignorar` são relativos ao diretório do `cdd.json`, com `/` como separador: `*` casa qualquer trecho dentro de um segmento, `?` casa um caractere e `**` casa zero ou mais segmentos. Assim `**/*_test.go` casa em qualquer profundidade, inclusive na raiz, e `**/vendor/**` casa tudo o que está abaixo de qualquer diretório `vendor`. Se um arquivo casa com `ignorar` e com `testes`, ele é ignorado.

### Exemplo de `cdd.json`

Independente de linguagem e alinhado com a Tabela I do artigo do ICSME 2020. É o mesmo conteúdo de `exemplos/cdd.json`:

```json
{
  "unidade": "tipo",
  "limiteIcpPorUnidade": 7,
  "itens": [
    { "id": "ramo-condicional",       "descricao": "Cada ramo de um desvio condicional: o `if` conta 1, cada `else if` conta 1 e o `else` final conta 1 (if-else soma 2).", "pontos": 1 },
    { "id": "case",                   "descricao": "Cada ramo (`case`) de uma seleção múltipla, incluindo o ramo padrão (`default`).", "pontos": 1 },
    { "id": "tratamento-de-excecao",  "descricao": "Cada bloco de um tratamento de exceção: o `try` conta 1, cada `catch` conta 1 e o `finally` conta 1 (try-catch-finally soma 3).", "pontos": 1 },
    { "id": "acoplamento-contextual", "descricao": "Cada tipo distinto criado no próprio projeto que a unidade referencia pelo nome, fora ela mesma. Conta 1 por tipo, não por uso. Tipos da linguagem, da biblioteca padrão, de frameworks e de bibliotecas não contam. Funções e constantes do projeto não contam.", "pontos": 1 },
    { "id": "funcao-como-argumento",  "descricao": "Cada função passada como argumento em uma chamada (lambda, closure, bloco, função anônima ou referência a função).", "pontos": 1 }
  ]
}
```

A linha "código de infraestrutura, framework e biblioteca = 0" da tabela não precisa de item: o que não está em `itens` não pontua. O limite 7 é a sugestão do artigo para aplicações web com times de experiência mista; para frameworks e bibliotecas ele sugere entre 10 e 12. Este exemplo deixa loops, operador ternário, operadores lógicos e herança sem item, então a revisão os lista em "Construções não cobertas" até o time decidir.

Além desse, `exemplos/` traz templates com `testes`, `ignorar` e itens escritos para cada ecossistema: `cdd.java.json`, `cdd.python.json`, `cdd.typescript.json` e `cdd.go.json`. Copie o escolhido para o projeto com o nome `cdd.json`.

## O que é uma unidade

- **`"arquivo"`**: cada arquivo-fonte é uma unidade. O nome da unidade é o caminho do arquivo.
- **`"tipo"`**: cada tipo de nível superior (classe, struct, interface, trait, protocolo, enum, record, object, tipo nomeado) com todos os seus membros é uma unidade. O nome da unidade é o nome do tipo, qualificado pelo pacote ou módulo se houver homônimos.
  - **Membros em outros arquivos** pertencem à mesma unidade: partial classes de C#, métodos com receiver em Go, blocos `impl` em Rust, `extension` em Swift. Procure-os antes de contar (mesmo pacote em Go, mesmo crate em Rust).
  - **Tipos aninhados** (classes internas, anônimas, locais a um método) não são unidades: o código deles conta na unidade do tipo de nível superior que os contém.
  - **Código fora de qualquer tipo** (funções livres, inclusive as construtoras; instruções de topo; constantes e variáveis de módulo, tipadas ou não) forma uma unidade por arquivo, chamada `<arquivo> (fora de tipos)`. Tipos declarados dentro de uma função livre contam nela. Arquivo sem código fora de tipos não gera essa unidade.

**Produção ou teste.** A unidade é de teste quando o arquivo dela (em `"tipo"`, o arquivo onde o tipo é declarado) casa com algum glob de `testes`; senão é de produção. Uma unidade nunca mistura os dois lados nem configurações: membros de um tipo declarados em arquivo do outro lado contam na unidade `(fora de tipos)` daquele arquivo (onde o tipo deles é um tipo referenciado como outro qualquer), e membros em arquivo ignorado ou regido por outro `cdd.json` ficam de fora.

## Como contar

1. **Cada item é contado de forma independente.** Uma construção que casa com dois itens soma nos dois, a não ser que a descrição de um deles a exclua. Excluir uma construção não exclui o que está dentro dela: se o `if` de filtro de uma comprehension não conta, o `and` da condição dele ainda conta no item de operador lógico.
2. **A descrição do item manda.** Ela diz o que casa e o que é uma ocorrência. Valem as construções que ela nomeia e as equivalentes diretas delas na linguagem do arquivo, pela linha da família na tabela abaixo (`if` ↔ `unless`, `guard`; `else if` ↔ `elif`, `elsif`; `catch` ↔ `except`, `rescue`). Uma construção do núcleo sem equivalente nomeado não conta: se a descrição fala só de `if` e `else if`, o `else` fica de fora. Quando a descrição nomeia apenas a família ("cada loop"), vale o núcleo inteiro. Listas na descrição são fechadas, a não ser que venham marcadas como exemplo ("ex.:", "por exemplo", "como").
3. **Conte o que está escrito, não a intenção.** `if err != nil` é um `if`: casa com um item de desvio condicional e só casa com um item de tratamento de erro se a descrição disser.
4. **Todo o código da unidade conta**, inclusive funções aninhadas, lambdas e closures. Comentários, literais de texto e linhas de import não contam.
5. **Itens de 0 ponto** são contados e aparecem na quebra como os outros.
6. **Na dúvida, não conte.** Se a descrição não resolve se um trecho casa, deixe-o fora do ICP e registre-o em "Construções não cobertas" com a pergunta para o time. Nunca estique uma descrição nem invente pontuação.

Famílias de construções equivalentes entre linguagens:

| Família | Núcleo | Só conta se a descrição do item mencionar |
|---|---|---|
| Desvio condicional | `if`, `else if` / `elif` / `elsif`, `else`, `unless`, `guard` (inclusive `if` usado como expressão) | guarda de `case`, `if` de filtro em comprehension, diretivas de template (ex.: `v-if`, `{#if}`) |
| Seleção múltipla | ramos (`case`, `default`) de `switch`, `match`, `when`, `cond`, `select` | cláusulas de função com pattern matching |
| Expressão condicional | ternário `c ? a : b`, `a if c else b` | — |
| Loop | `for`, `foreach` / `for-in` / `for-of` / `range`, `while`, `do-while`, `until`, `loop`, `repeat` | iteração de ordem superior (ex.: `map`, `filter`, `find`, `forEach`, `each`), comprehensions, diretivas de template (ex.: `v-for`, `{#each}`) |
| Tratamento de erro | `try`, `catch` / `except` / `rescue`, `finally` / `ensure` | `if err != nil`, operador `?` de Rust, `Result` / `Either`, `defer` / `recover`, `.catch()` de promise |
| Operador lógico | `&&`, `\|\|`, `and`, `or` | `??`, `?.`, operador Elvis `a ?: b` |
| Função como argumento | lambda, closure, bloco, função anônima ou referência a função passada em uma chamada | decorators, função passada como prop ou atributo |
| Acoplamento contextual | tipos declarados no próprio projeto que a unidade referencia pelo nome | funções, constantes e módulos do projeto |
| Herança | `extends` ou lista de bases com classe abstrata ou concreta | implementar interface, trait ou protocolo; embedding de struct; mixins |

**Acoplamento contextual**, salvo se a descrição do item disser outra coisa: conta a referência ao tipo pelo nome, em qualquer posição do código da unidade, inclusive no acesso a um membro estático ou de enum (ex.: `Plano.Ouro`). Um identificador homônimo (campo, variável ou método com o mesmo nome do tipo) não é referência, e um tipo usado sem ser nomeado (inferido) não conta. A unidade não conta a si mesma nem aos seus tipos aninhados. "Próprio projeto" é o código escrito pelo time no repositório; tipos da linguagem, da biblioteca padrão, de frameworks e de dependências, mesmo copiadas para dentro do repositório, não entram.

Uma construção está **coberta** quando a descrição de algum item a menciona, em qualquer papel, seja para contar, seja para excluir — mesmo que o item não se aplique àquele lado (produção ou teste). Uma chamada de ordem superior cuja função já contou em um item de função como argumento também está coberta. Vai para "Construções não cobertas" toda construção do núcleo ou da terceira coluna que aparece no código e que nenhum item cobre, além das dúvidas da regra 6. É a lista do que o time ainda não decidiu; ela não altera o ICP.

## Escopo

- Arquivos de código-fonte passados pelo usuário (um diretório vale por todos os arquivos de código-fonte abaixo dele) ou, se nada for passado, alterados na branch atual. Arquivos de dados, configuração, marcação pura, manifestos de dependência e artefatos de build ficam fora, sem menção no relatório.
- Respeite `ignorar`: arquivos que casam com algum glob não são contados.
- Em `"tipo"`, entra no escopo toda unidade com algum trecho nos arquivos do escopo, e ela é contada inteira.

## Como executar

### Passo 1 — Carregar a configuração

1. Localize o `cdd.json` aplicável a cada arquivo do escopo.
2. Valide cada `cdd.json` contra `cdd.schema.json` e contra as regras de invalidez acima. Se inválido, interrompa e mostre o erro.
3. Memorize: `unidade`, `limiteIcpPorUnidade`, `limiteIcpPorUnidadeDeTeste` (se houver), `testes`, `ignorar` e a lista `itens` indexada por `id`.

### Passo 2 — Delimitar as unidades

Para cada arquivo no escopo:

1. Se o caminho casa com algum padrão de `ignorar`, pule.
2. Determine se o arquivo é de **produção** ou de **teste** pelos globs de `testes`.
3. Separe o código em unidades conforme o campo `unidade`. Em `"tipo"`, busque os membros de cada tipo em outros arquivos.

### Passo 3 — Para cada unidade

1. Percorra o código contando as ocorrências de cada item de `itens`, respeitando `aplicaSomenteEm` e as regras de "Como contar". Anote arquivo e linha de cada ocorrência; para acoplamento, a linha da primeira referência a cada tipo.
2. Some `pontos × ocorrências` para obter o **ICP da unidade**.
3. Compare contra o limite aplicável:
   - Teste → `limiteIcpPorUnidadeDeTeste` se definido, senão `limiteIcpPorUnidade`.
   - Produção → `limiteIcpPorUnidade`.

### Passo 4 — Marcar violações

- Unidade com ICP **acima** do limite → **violação**.
- Unidade com ICP **igual** ao limite → **observação** (no fio).
- Unidade abaixo → silêncio.

## Formato do relatório

Para cada unidade revisada que mereça menção (o bloco é o mesmo para violação e no fio):

```
Unidade: <nome da unidade>
Arquivo(s): <caminho>[, <caminho>...]
Categoria: CDD
ICP: <total> / <limite> (<producao|teste>)
Status: <VIOLAÇÃO | NO FIO>
Quebra:
  - <id-item-1>: <ocorrências> × <pontos> = <subtotal>
  - <id-item-2>: <ocorrências> × <pontos> = <subtotal>
  ...
Hotspots:
  - <arquivo>:<linha> — <id-item>[, <id-item>...] (trecho curto)
  - ...
Sugestão de extração: <sugestão concreta — ex: "extrair a validação de X para um tipo ValidaX", "mover o cálculo Y, com seus loops aninhados, para um módulo dedicado">
```

Caminhos são relativos à raiz do repositório (ou do diretório de trabalho). Em `"arquivo"` o nome da unidade já é o caminho; omita a linha `Arquivo(s)`. Na quebra entram os itens com pelo menos uma ocorrência. Se a revisão usou mais de um `cdd.json`, acrescente a cada bloco, antes de `Categoria`, a linha `Config: <caminho do cdd.json>`.

Depois das unidades, quando houver o que listar:

```
Construções não cobertas (<caminho do cdd.json>):
  - <construção> — <n> ocorrência(s), ex.: <arquivo>:<linha> — <por que nenhum item cobre, ou a dúvida para o time>
Não revisados:
  - <arquivo> — <sem cdd.json aplicável | ignorado pelo cdd.json>
```

"Construções não cobertas" tem um bloco por `cdd.json` e uma linha por construção (cada palavra-chave ou operador na sua), somando todas as unidades revisadas, inclusive as que ficaram abaixo do limite. Em "Não revisados", arquivos ignorados só aparecem se o usuário os passou explicitamente.

Se nenhuma unidade ultrapassar o limite, feche o relatório com: **"CDD: nenhuma unidade acima do limite cognitivo."** Os blocos de unidades no fio continuam aparecendo.

## Princípios

- **Sem defaults**: sem `cdd.json` válido não há contagem. A pontuação precisa ser auditável.
- **Não conte regras que não estão em `itens`**: se algo parece complexo mas não tem item correspondente, registre em "Construções não cobertas" — não invente pontuação.
- **Hotspots ajudam a agir**: a sugestão de extração não vem do nada — aponte os 2-3 lugares com mais peso na unidade. A extração sugerida tem de tirar pontos da unidade, isto é, levar o trecho para outra unidade; mover para outra função da mesma unidade não muda o ICP.
- **ICP é intra-unidade**: a contagem olha só para o código da própria unidade e nunca soma o ICP das unidades que ela usa. O que pode entrar, se o time listar em `itens`, é o acoplamento contextual: a menção a outro tipo do projeto, visível dentro da unidade. Métricas de acoplamento entre unidades (quem depende de quem, e quanto) não entram aqui.
- **Declarações não contam por si só**: funções, métodos, construtores, acessores, campos e imports só pontuam se algum item os descrever.
