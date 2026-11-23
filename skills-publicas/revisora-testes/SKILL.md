---
name: revisora-testes
description: Revisa testes automatizados em qualquer linguagem. Use ao revisar arquivos ou suítes de teste, ou quando houver suspeita de excesso de mocks, objetos de domínio substituídos por dublês, matchers coringa ("qualquer valor"), falta de teste de fronteira, condicionais compostas sem cobertura MC/DC ou descrições de teste que não dizem o comportamento esperado. Para Java com JUnit/Mockito, use a revisora-testes-java.
---

# Revisora — Testes

Skill focada exclusivamente em **arquivos de teste**, em qualquer linguagem e com qualquer ferramenta de teste. As seis regras têm a mesma numeração na `revisora-testes-java`, que as aplica a Java com JUnit/Mockito; se o código revisado for desse stack, use ela.

Os exemplos usam um domínio fictício de pedidos (`Pedido`, `Cliente`, frete, desconto). Eles só ilustram as regras; aplique-as ao domínio do projeto revisado.

## Escopo

- Arquivos de teste, reconhecidos pela convenção da linguagem ou do projeto (diretório próprio de testes, prefixo ou sufixo no nome do arquivo).
- Arquivos de teste passados pelo usuário ou alterados na branch atual.
- Os achados são sempre sobre o arquivo de teste. Para avaliar as regras 4 e 5, leia também a unidade testada (a função, classe ou módulo que o teste exercita): é nela que estão os limites e as condicionais compostas que os testes deveriam cobrir.

## Checagens

### Regra 1 — Versões reais sempre que possível

- [ ] Objetos de domínio (entidades, value objects) são **construídos de verdade**, não substituídos por dublês.
- Sinal: um `Cliente` ou `Pedido` fabricado pela ferramenta de dublês, ou trocado por um substituto escrito à mão, no lugar do objeto real → violação. Crie a instância real (construtor ou o meio equivalente na linguagem).

### Regra 2 — Mocks e outros dublês restritos a banco e APIs HTTP externas

- [ ] Dublês (mock, stub, fake) são usados apenas para **acesso a banco de dados** e **APIs HTTP externas**; serviços internos do próprio módulo, componentes de domínio e helpers **não são substituídos**.
- Olhe o que o dublê substitui, não como ele foi criado: repositório, DAO ou cliente do banco → permitido; cliente HTTP de um serviço externo → permitido; qualquer outro serviço, componente ou helper do próprio projeto → violação.
- Substituir um módulo ou uma função inteira pelo caminho (o *patch* de módulo de algumas ferramentas) conta como dublê do alvo substituído.

### Regra 3 — Proibido matcher coringa

- [ ] **Nenhum matcher coringa** (os que aceitam "qualquer valor" ou "qualquer valor de um tipo"); setup e verificação dos dublês usam **as referências reais** criadas no próprio teste.
- Sinal: o stub ou a verificação recebe um coringa no lugar do argumento → violação. Troque pelo próprio objeto ou valor criado no teste.
- Ficam fora desta regra: matchers que comparam de fato (igualdade, predicado, correspondência parcial) e coringas usados em asserções sobre o resultado, sem dublê envolvido.
- Coringa implícito: em algumas ferramentas o stub devolve o valor configurado para quaisquer argumentos, e há verificações que só conferem que a chamada aconteceu. Como não há matcher escrito, não é a violação dura; registre como observação quando nenhuma verificação do teste confere a chamada com os valores reais.

### Regra 4 — Boundary testing

- [ ] Quando há limites/fronteiras (tamanhos, datas, índices, contadores), os testes cobrem **valores na fronteira** (mínimo permitido, máximo permitido, off-by-one acima e abaixo).
- Não é violação dura por arquivo, mas marque como "boundary ausente" quando uma condição de limite óbvia está sem cobertura.

Fronteira é o valor em que o comportamento muda. Operador trocado (`>` no lugar de `>=`) e limite deslocado em uma unidade só aparecem nos valores colados a esse ponto; valores no meio de cada faixa passam do mesmo jeito com o código errado.

```python
# Ilustração em Python; o raciocínio é o mesmo em qualquer linguagem.
# Unidade testada: desconto de atacado a partir de 10 unidades
def tem_desconto_de_atacado(quantidade):
    return quantidade >= 10

# Fronteira sem cobertura: 2 e 50 continuam passando se alguém trocar `>=` por `>`
assert not tem_desconto_de_atacado(2)
assert tem_desconto_de_atacado(50)

# Fronteira coberta: o limite e o vizinho imediato
assert not tem_desconto_de_atacado(9)
assert tem_desconto_de_atacado(10)
```

### Regra 5 — Cobertura MC/DC em condicionais compostas

- [ ] Em funções e métodos com condicionais compostas (`a && b`, `a || b`, `a and b`, ternários encadeados), os testes cobrem cada condição independentemente afetando o resultado.
- Marque como observação (não violação dura) se sentir que a cobertura das condicionais compostas não está demonstrada.

MC/DC (Modified Condition/Decision Coverage) pede, para cada condição da expressão, um par de testes em que **só ela** muda de valor e o resultado muda junto. Para `a && b` isso dá três casos (V,V · F,V · V,F); para `a || b`, outros três (F,F · V,F · F,V). Um teste "tudo verdadeiro" e outro "tudo falso" não mostram o efeito de nenhuma condição isolada.

```python
# Ilustração em Python; o raciocínio é o mesmo em qualquer linguagem.
# Unidade testada: frete grátis para assinante (A) ou pedido a partir de R$ 200 (B)
def frete_gratis(assinante, total):
    return assinante or total >= 200

# Não demonstra: A e B mudam juntas; os dois testes passariam também com `and` no lugar de `or`
assert frete_gratis(True, 250)
assert not frete_gratis(False, 50)

# Demonstra: cada condição, sozinha, muda o resultado (com valores de fronteira, regra 4)
assert not frete_gratis(False, 199)  # A e B falsas
assert frete_gratis(True, 199)       # só A mudou
assert frete_gratis(False, 200)      # só B mudou
```

### Regra 6 — Descrição do teste em português

- [ ] A descrição legível de cada teste está em português e descreve o **comportamento esperado** (não só o nome do método).
- Sinal: descrição que repete o nome da função de teste (`"testCancelaPedido"`) ou está em inglês → violação. Exemplo aceito: `"deve cancelar o pedido que ainda não foi enviado"`.
- A regra avalia o texto de descrição que a ferramenta oferece (título em string, nome de exibição, nome de subteste). Se o teste só tem o identificador da função, não há descrição a avaliar: a regra não exige criar uma nem se aplica ao identificador.

## Como os sinais aparecem em cada ferramenta

Amostra para reconhecer os sinais; não é lista completa. Em outra ferramenta, procure o equivalente.

| Ferramenta | Criação de dublê (regras 1 e 2) | Matcher coringa (regra 3) | Descrição do teste (regra 6) |
|---|---|---|---|
| Mockito + JUnit 5 (Java) | `mock(X.class)`, `@Mock`, `@MockBean`, `@MockitoBean` | `any()`, `any(X.class)`, `anyString()`, `anyLong()` | `@DisplayName("...")` |
| Jest / Vitest (JS/TS) | `jest.fn()`, `jest.mock(...)`, `vi.fn()`, `vi.mock(...)` | `expect.anything()`, `expect.any(Tipo)` | texto de `test("...")` / `it("...")` |
| unittest.mock / pytest (Python) | `Mock()`, `MagicMock()`, `patch(...)`, `mocker.patch(...)`, `monkeypatch.setattr(...)` | `mock.ANY` | docstring do teste no unittest; no pytest, só o nome da função |
| gomock / testify (Go) | `NewMockXxx(ctrl)` gerado pelo mockgen; struct com `mock.Mock` embutido | `gomock.Any()`, `mock.Anything`, `mock.AnythingOfType("...")` | nome passado a `t.Run("...", ...)` |
| Moq / NSubstitute (C#) | `new Mock<T>()`, `Mock.Of<T>()`, `Substitute.For<T>()` | `It.IsAny<T>()`, `Arg.Any<T>()` | xUnit: `[Fact(DisplayName = "...")]` |

- Dublê por caminho de módulo (regra 2): `patch("loja.frete.calcula")`, `jest.mock("./frete")`, `vi.mock("./frete")`.
- Coringa implícito (regra 3): stubs como `repositorio.busca.return_value = pedido` e `busca.mockReturnValue(pedido)`; verificações como `assert_called()` e `toHaveBeenCalled()`. As que conferem os valores reais são `assert_called_once_with(42)` e `toHaveBeenCalledWith(42)`.

## Formato do relatório

```
<arquivo>:<linha>
Categoria: Testes
Regra: <1 ... 6>
Trecho:
    <2-5 linhas>
Correção sugerida: <substituir dublê por objeto real, trocar coringa pela referência X, etc>
```

Se nenhuma violação: **"Testes: nenhuma violação encontrada."**

## Princípios

- A regra 3 é **dura** — qualquer matcher coringa no setup ou na verificação de um dublê é violação direta.
- As regras 4 e 5 são marcadas como **observação** quando ausentes, e como **violação** apenas quando há claramente cenários de limite ou ramos lógicos não exercitados.
- As regras 1 e 2 não valem para classes não-domínio (config, infra) — use bom senso para o que é "domínio".
