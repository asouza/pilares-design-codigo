## Quando uma lógica merece uma abstração só para ela

### Contexto

Divisão de responsabilidades(separation of concerns) é um princípio fundamental na engenharia de software. Em geral isso é feito porque a pessoa que está programando acredita que dividindo aquele trecho de código vai facilitar o entendimento de outra. A pergunta que fica é: qual regra pode ser usada para fazer a divisão?

A expressão "dividir responsabilidade" acabou meio que se misturando com algo mais próximo de um feeling. Talvez você já tenha se perguntado algo como: será que esse método é dessa classe ou daquela? Quando essa tática é assumida, a implementação do código acaba cheia de vieses, já que é muuuuuito complicado você conseguir estabelecer uma régua de linha de pensamento geral para uma equipe de qualquer tamanho. 

Neste texto vamos vamos discutir uma maneira lógica de identificar a necessidade de dividir responsabilidade. Uma forma que pode ser adotada pela equipe inteira e que pode gerar uma divisão de responsabilidade que seja manutenível por qualquer pessoa.

### Problema

```
	public Transacao paga(TentativaPagamento tentativaPagamento) {

		TreeSet<Pagador> pagadores = this.processadores.stream()
				.map(processador -> processador.aceita(tentativaPagamento))
				.filter(Optional :: isPresent)
				.map(Optional :: get)
				.collect(Collectors.toCollection(() -> new TreeSet<>()));

		Assert.isTrue(!pagadores.isEmpty(),
				"Precisa existir pelo menos um objeto do tipo Pagador disponível para realizar o pagamento. #bug");

		for (Pagador pagador : pagadores) {
			try {
				Transacao novaTransacao = pagador.paga();
				log.info("Pagamento realizado com sucesso para {} na tentativa {}",pagador,tentativaPagamento);
				return novaTransacao;
			} catch (FeignException e) {
				log.error("Problema de rede enquanto tentava pagar com {} a tentiva {} => {}",pagador,tentativaPagamento,e);
			} catch(Exception e) {
				log.error("Azedou o mingau enquanto tentava pagar {} => {}",tentativaPagamento,e);
			}
		}
		
		log.error("Não foi possível realizar o pagamento para {}",tentativaPagamento);
		return new Transacao();
	}

```

Olhando só o método acima, desconsiderando outras dependências que a classe poderia ter, nós temos 11 pontos de carga intrínseca. 

* +1 Transacao
* +1 Pagador
* +1 TentativaPagamento
* +1 map(processador -> processador.aceita(tentativaPagamento))
* +2 filter(Optional :: isPresent) branch do filter + branch do if
* +1 map(Optional :: get)
* +1 collect(Collectors.toCollection(() -> new TreeSet<>()));
* +1 for (Pagador pagador : pagadores)
* +1 try {
* +1 catch (FeignException e) {
* +1 catch(Exception e) {

Aqui claramente estouramos o limite de carga intrínseca numa classe considerada um Domain Service da aplicação, que tem uma restrição mais forte de carga intrínseca(7 pontos), já que deveria lidar com fluxo de negócio e isso precisa ser super claro. 

Por outro lado, ela respeita lindamente a regra de manter as lógicas que operam sobre o estado dentro da própria classe. Operamos sobre o atributo ```processadores``` que é uma coleção de possíveis processadores de pagamento e temos o método ```aceita``` que é um método público da interface ```ProcessadorDePagamento``` que também opera sobre seu estado. Ou seja, estamos sendo bons súditos de Evans e deixando nossa inteligência distribuída no que ele chama de Domain Model.  Como fazer então?

Essa é a parte boa de termos um jeito lógico de distribuir a complexidade pelo nosso sistema. Não importa se tudo está supostamente ok, temos um Domain Service que passou de 7 pontos e a lei da distribuição da carga intrínseca  diz que isso não é permitido neste ponto do sistema. A parte do código que você vai querer distribuir é um detalhe importante, mas não mais importante do que o fato de distribuir em si. 

### Solução

```
	@Autowired //pedindo injeção automática pelo Spring
private PagadoresOrdenadosPeloMenorCusto pagadoresOrdenadosPeloMenorCusto;

	public Transacao paga(TentativaPagamento tentativaPagamento) {
		//cortamos 5 pontos de carga intrínseca aqui
		SortedSet<Pagador> pagadoresFiltrados = pagadoresOrdenadosPeloMenorCusto
				.filtra(tentativaPagamento);


		Assert.isTrue(!pagadores.isEmpty(),
				"Precisa existir pelo menos um objeto do tipo Pagador disponível para realizar o pagamento. #bug");

		for (Pagador pagador : pagadores) {
			try {
				Transacao novaTransacao = pagador.paga();
				log.info("Pagamento realizado com sucesso para {} na tentativa {}",pagador,tentativaPagamento);
				return novaTransacao;
			} catch (FeignException e) {
				log.error("Problema de rede enquanto tentava pagar com {} a tentiva {} => {}",pagador,tentativaPagamento,e);
			} catch(Exception e) {
				log.error("Azedou o mingau enquanto tentava pagar {} => {}",tentativaPagamento,e);
			}
		}
		
		log.error("Não foi possível realizar o pagamento para {}",tentativaPagamento);
		return new Transacao(,,,);
	}

```

Agora a classe ```PagadoresOrdenadosPeloMenorCusto``` tem o código que filtra os possíveis pagadores para uma determinada tentativa de pagamento. Poderia ter sido isolado todo código do ```for``` também. Essa classe que acabou de ser criada, inclusive, pode ter visibilidade de pacote(caso a linguagem utilizada suporte tal visibilidade). 

No fim realmente a carga intrínseca foi distribuída. As duas classes, na verdade, formam uma só dividida em duas para balancear a carga intrínseca do material. É importante rodarmos a mesma análise de carga intrínseca, para comparar. 

* +1 Transacao
* +1 Pagador
* +1 TentativaPagamento
* +1 for (Pagador pagador : pagadores)
* +1 try {
* +1 catch (FeignException e) {
* +1 catch(Exception e) {
* +1 PagadoresOrdenadosPeloMenorCusto(novo acoplamento)



O código agora caiu para 8 pontos de carga intrínseca. Ainda estamos acima um ponto, muito por conta do tratamento de exceptions. Seguindo a lei, ainda é necessário cortar um ponto aqui, a parte boa é que agora você tem um caminho para isso.

Devo agora criar uma abstração para executar o for também? Se você fizer isso vai cair num código mais anêmico, dado que essa classe vai ter carga intrínseca super baixa(3 pontos talvez). Dessa forma você estaria desconsiderando a capacidade de entendimento do(a) colega de trabalho.

### Primeiro ponto extra de atenção

Seu objetivo não é deixar cada arquivo com dois ou três pontos de carga intrínseca. Você acabaria com muito mais arquivos do que precisaria e o balanço provavelmente não seria positivo. Já que estaria distribuindo responsabilidade de forma exagerada e desconsiderando a capacidade de entendimento das pessoas que trabalham com você. 

Ficar entre 5 e 9 pontos tende a ser interessante, dada às pesquisas relativas a psicologia da cognição e que já mencionamos aqui. 

### Segundo ponto extra de atenção

Fique atento a códigos onde você tem uma sequência muita grande de transformação em cima de uma coleção. Naturalmente aquele código vai ter carga intrínseca mais alta e pode virar um candidato a ser extraído para outro ponto.

### Por que eu devo fazer isso?

Porque vai ter uma hora que a lógica que você está trabalhando é simplesmente um pouco mais complicada de implementar e isso pode acarretar no estouro da carga intrínseca permitida para aquela parte. Agora você tem uma mais uma forma clara de dividir responsabilidade pelo código. 

### Conclusão

Toda indireção(novo arquivo) precisa merecer ser criada. Dividir a carga intrínseca, também conhecida como divisão de responsabilidade do código é um ótimo motivo para criar essa indireção. 
