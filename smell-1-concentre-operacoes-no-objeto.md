## Operações sobre dados cujo tipo é padrão da linguagem devem ficar dentro das classes dos objetos.

### Problema

```
        final Bolao bolao = bolaoRepository.findById(this.idBolao).get();

        if (bolao.getDataExpiracao().isBefore(Instant.now())) {
            return ResultadoConfirmacao.conviteExpirado();
        }

```

Aqui temos uma lógica de verificação de data sobre um estado do objeto do tipo Bolao.

### Solução

```
        final Bolao bolao = bolaoRepository.findById(this.idBolao).get();

        if (bolao.aceitaConvite()) {
            return ResultadoConfirmacao.conviteExpirado();
        }

```

Pode ser que seja necessário deixar o instante flexível. Para isso usamos os parâmetros. 

```
        final Bolao bolao = bolaoRepository.findById(this.idBolao).get();

        if (bolao.aceitaConvite(Instant.now())) {
            return ResultadoConfirmacao.conviteExpirado();
        }

```

### Implementação

```
	public boolean aceita(Instant data) {
		return this.dataExpiracao.before(data);
}
```

### Por que eu devo fazer isso?

Porque quando você concentra toda lógica sobre um tipo padrão da linguagem que também representa o estado de um objeto dentro da classe daquele objeto você aumenta as chances de distribuir a complexidade do código respeitando os limites do DDD da massa. 

### Por que você está falando sobre o tipo padrão da linguagem?

Porque essa é uma forma nítida de você ter um norte. No exemplo acima, ```DateTime``` é um tipo padrão do Java, então você concentra a lógica sobre ela dentro da sua classe. Só que como ```DateTime``` também é tipo complexo, aplicamos a mesma regra de concentrar a lógica sobre tipos mais básicos dentro da classe que é dona dos tipos mais básicos.

Você não acessou a informação que guarda o instante no tipo ```DateTime``` para analisar se era anterior ao instante atual. Delegamos a chamada para um método pronto na própria classe ```DateTime```. Ou seja, aplicou a mesma regra. 

Um outro exemplo onde podemos podemos fazer a mesma análise.

```
      public Set<PaymentMethod> filterDesiredPaymentMethods(User user) {
return this.paymentMethods
                    .stream()
.filter(paymentMethod ->   user.getDesiredPaymentMethods().contains(paymentMethod)) 
.collect(Collectors.toSet());

```

Perceba que na linha ```user.getDesiredPaymentMethods().contains(paymentMethod)) ``` é acessado diretamente o tipo que referencia uma coleção e aí em seguida acessamos um método pronto. O problema é que esse tipo vive no ```User``` :).  Aqui basta que seja aplicada a mesma regra **operações sobre dados cujo tipo é padrão da linguagem devem ficar dentro das classes dos objetos**.


```
      public Set<PaymentMethod> filterDesiredPaymentMethods(User user) {
return this.paymentMethods
                    .stream()
.filter(user :: acceptsPayment) 
.collect(Collectors.toSet());

```

Agora você tem um método no ```User``` que opera sobre o estado dele. Lá dentro, para operar sobre estado da coleção, é invocado o método ```contains```. E dessa forma você vai distribuindo a complexidade do sistema e deixando a inteligência da aplicação dividida de uma forma que respeite nosso limite natural de entendimento. 

## Conclusão

Lógica vivendo fora da classe que tem a declaração do estado do objeto é algo regular nas aplicações. Agora você tem um jeito lógico de dividir isso. O ponto de atenção recorrente é a verificação da carga intrínseca da classe :). Lembre que classes que mantém estado inteligente do sistema devem respeitar o limite de 9 pontos máximos de carga intrínseca. Essa já é um pontuação super alta quando olhamos para a limitação sugerida da nossa memória de trabalho, então seja bondoso(a) e não demande mais do que a cabeça do(a) colega pode suportar. 
