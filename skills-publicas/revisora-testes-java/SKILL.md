---
name: revisora-testes-java
description: Aplica as regras da revisora-testes a testes Java escritos com JUnit 5, Mockito e Spring. Use ao revisar arquivos *Test.java ou *IntegrationTest.java, ou diante de mock() ou @Mock sobre entidade, service interno mockado, any() e outros matchers do Mockito, falta de teste de fronteira, condicionais compostas sem cobertura MC/DC ou @DisplayName que não descreve o comportamento.
---

# Revisora — Testes (Java)

Aplicação da skill `revisora-testes` ao stack Java com JUnit 5, Mockito e Spring. As seis regras e a numeração são as mesmas; aqui ficam os sinais na API dessas ferramentas e os exemplos em Java. O racional independente de ferramenta (o que é fronteira, o que é MC/DC) está na `revisora-testes`.

Os exemplos usam um domínio fictício de pedidos (`Pedido`, `Cliente`, `Produto`, `Cupom`). Eles só ilustram as regras; aplique-as ao domínio do projeto revisado.

## Escopo

- Arquivos sob `src/test/` (geralmente `src/test/java/.../*Test.java` ou `*IntegrationTest.java`).
- Arquivos passados pelo usuário ou alterados na branch atual com sufixo de teste.
- Os achados são sempre sobre o arquivo de teste. Para avaliar as regras 4 e 5, leia também a classe testada: é nela que estão os limites e as condicionais compostas.

## Checagens

### Regra 1 — Versões reais sempre que possível

- [ ] Objetos de domínio (entidades, value objects) são **construídos de verdade**, não substituídos por dublês.
- Sinal: `mock(Cliente.class)`, `mock(Pedido.class)` ou `@Mock` sobre uma entidade → violação. Crie a instância via construtor.

```java
// Viola: entidade de domínio mockada
Cliente cliente = mock(Cliente.class);
when(cliente.getEmail()).thenReturn("ana@exemplo.com");
Pedido pedido = new Pedido(cliente);

// Corrige: instância real, criada pelo construtor
Cliente cliente = new Cliente("Ana", "ana@exemplo.com");
Pedido pedido = new Pedido(cliente);
```

### Regra 2 — Mocks e outros dublês restritos a banco e APIs HTTP externas

- [ ] Dublês (mock, stub, fake) são usados apenas para **acesso a banco de dados** e **APIs HTTP externas**; services internos do próprio módulo, components de domínio e helpers **não são substituídos**.
- Tipos com mock permitido:
  - **Repositories** (Spring Data) — acesso a banco.
  - **FeignClients** / `RestClient` / `WebClient` — APIs HTTP externas.
- Sinal: `mock(...)`, `@Mock`, `@MockBean` ou `@MockitoBean` sobre um `@Service` ou `@Component` do próprio módulo → violação. Use a instância real.

```java
PedidoRepository pedidoRepository = mock(PedidoRepository.class); // acesso a banco: mock permitido
Pedido pedido = new Pedido(new Cliente("Ana", "ana@exemplo.com"));

// Viola: service interno mockado; o cálculo do frete nunca roda de verdade
CalculadoraDeFrete calculadoraDeFrete = mock(CalculadoraDeFrete.class);
when(calculadoraDeFrete.calcula(pedido)).thenReturn(new BigDecimal("15.00"));
FechaPedido fechaPedido = new FechaPedido(pedidoRepository, calculadoraDeFrete);

// Corrige: service interno real, criado pelo construtor
FechaPedido fechaPedido = new FechaPedido(pedidoRepository, new CalculadoraDeFrete());
```

### Regra 3 — Proibido matcher coringa (`any()` e família)

- [ ] **Nenhum matcher coringa** (os que aceitam "qualquer valor" ou "qualquer valor de um tipo"); setup e verificação dos dublês usam **as referências reais** criadas no próprio teste.
- Matchers proibidos: `any()`, `any(SomeClass.class)`, `anyString()`, `anyInt()`, `anyLong()`, `anyMap()`, `anyList()`, etc.
- Sinal: `when(pedidoRepository.save(any(Pedido.class))).thenReturn(...)` → violação. Use `when(pedidoRepository.save(pedido)).thenReturn(...)` com o `pedido` real construído no teste.

```java
PedidoRepository pedidoRepository = mock(PedidoRepository.class); // repository: mock permitido (regra 2)
CancelaPedido cancelaPedido = new CancelaPedido(pedidoRepository);
Pedido pedido = new Pedido(new Cliente("Ana", "ana@exemplo.com"));

// Viola: o teste passa com qualquer id e com qualquer pedido salvo
when(pedidoRepository.findById(anyLong())).thenReturn(Optional.of(pedido));
cancelaPedido.executa(42L);
verify(pedidoRepository).save(any(Pedido.class));

// Corrige: o id e o pedido são os que o próprio teste criou
when(pedidoRepository.findById(42L)).thenReturn(Optional.of(pedido));
cancelaPedido.executa(42L);
verify(pedidoRepository).save(pedido);
```

### Regra 4 — Boundary testing

- [ ] Quando há limites/fronteiras (tamanhos, datas, índices, contadores), os testes cobrem **valores na fronteira** (mínimo permitido, máximo permitido, off-by-one acima e abaixo).
- Não é violação dura por arquivo, mas marque como "boundary ausente" quando uma condição de limite óbvia está sem cobertura.

```java
// Produção (Produto): aceita de 1 até o máximo de unidades por pedido
public boolean aceitaQuantidade(int quantidade) {
    return quantidade >= 1 && quantidade <= this.quantidadeMaximaPorPedido;
}

Produto teclado = new Produto("Teclado", 10); // máximo de 10 unidades por pedido

// Fronteira sem cobertura: 5 e 50 continuam passando se alguém trocar `<=` por `<`
assertTrue(teclado.aceitaQuantidade(5));
assertFalse(teclado.aceitaQuantidade(50));

// Fronteira coberta: mínimo, máximo e o vizinho imediato de cada um
assertFalse(teclado.aceitaQuantidade(0));
assertTrue(teclado.aceitaQuantidade(1));
assertTrue(teclado.aceitaQuantidade(10));
assertFalse(teclado.aceitaQuantidade(11));
```

### Regra 5 — Cobertura MC/DC em condicionais compostas

- [ ] Em métodos com condicionais compostas (`a && b`, `a || b`, ternários encadeados), os testes cobrem cada condição independentemente afetando o resultado.
- Marque como observação (não violação dura) se sentir que a cobertura das condicionais compostas não está demonstrada.

```java
// Produção (Cupom): duas condições, A && B
public boolean aplicavelA(BigDecimal totalDoPedido, LocalDate hoje) {
    return !hoje.isAfter(this.validade) && totalDoPedido.compareTo(this.valorMinimo) >= 0;
}

// Cupom com valor mínimo de R$ 100,00 e validade até 30/06/2025
Cupom cupom = new Cupom("PROMO10", new BigDecimal("100.00"), LocalDate.of(2025, 6, 30));

// Não demonstra: A e B mudam juntas; os dois testes passariam também com `||` no lugar de `&&`
assertTrue(cupom.aplicavelA(new BigDecimal("150.00"), LocalDate.of(2025, 6, 15)));
assertFalse(cupom.aplicavelA(new BigDecimal("50.00"), LocalDate.of(2025, 7, 15)));

// Demonstra: cada condição, sozinha, muda o resultado (com valores de fronteira, regra 4)
assertTrue(cupom.aplicavelA(new BigDecimal("100.00"), LocalDate.of(2025, 6, 30)));  // A e B verdadeiras
assertFalse(cupom.aplicavelA(new BigDecimal("100.00"), LocalDate.of(2025, 7, 1)));  // só A mudou
assertFalse(cupom.aplicavelA(new BigDecimal("99.99"), LocalDate.of(2025, 6, 30)));  // só B mudou
```

### Regra 6 — Descrição do teste em português (`@DisplayName`)

- [ ] A descrição legível de cada teste — em JUnit 5, toda anotação `@DisplayName` — está em português e descreve o **comportamento esperado** (não só o nome do método).
- Sinal: `@DisplayName("testCancelaPedido")` ou em inglês → violação.

```java
// Viola: repete o nome do método
@Test
@DisplayName("testCancelaPedido")
void testCancelaPedido() { /* ... */ }

// Viola: está em inglês
@Test
@DisplayName("should not apply an expired coupon")
void naoAplicaCupomVencido() { /* ... */ }

// Corrige: em português, descrevendo o comportamento esperado
@Test
@DisplayName("deve cancelar o pedido que ainda não foi enviado")
void cancelaPedidoNaoEnviado() { /* ... */ }
```

## Formato do relatório

```
<arquivo>:<linha>
Categoria: Testes
Regra: <1 ... 6>
Trecho:
    <2-5 linhas>
Correção sugerida: <substituir mock por real, trocar any() por referência X, etc>
```

Se nenhuma violação: **"Testes: nenhuma violação encontrada."**

## Princípios

- A regra 3 é **dura** — qualquer ocorrência de `any...()` é violação direta.
- As regras 4 e 5 são marcadas como **observação** quando ausentes, e como **violação** apenas quando há claramente cenários de limite ou ramos lógicos não exercitados.
- As regras 1 e 2 não valem para classes não-domínio (config, infra) — use bom senso para o que é "domínio".
