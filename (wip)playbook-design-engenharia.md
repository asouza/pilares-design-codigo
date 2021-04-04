## Este playbook é para você que considera que facilidade de manutenção do código importa

Existe muito debate sobre a relação entre deadlines, quantidade de funcionalidades em intervalo de tempo, pressão e qualidade de código. Cada empresa, equipe ou pessoa obviamente é livre para priorizar o que entender que é melhor. 

Este playbook em específico é para quem acredita ou quer fazer a aposta  que qualidade não é negociável. Para estabelecer uma régua neste documento, vamos considerar a seguinte combinação: Prazo, entrega e qualidade. Como eu sugiro que você lide com a trinca:

1. Qualidade não é negociável;
2. Prazo é negociável;
3. Entrega é negociável;

Já voltamos a falar sobre isso. 

## Primeiramente vamos criar uma régua sobre qualidade neste documento

A ISO/IEC 25010:2011 define um conjunto de características que podem ser observadas em relação a qualidade interna do software. É algo bem completo e que abrange
diversas áreas que podem afetar a chamada qualidade. Enquanto a ISO olha para diversas características, este documento está focado em duas partes: **Adequação funcional e Manuntenibilidade**. 

**Quando falamos de Adequação funcional**, temos algumas subcaracterísticas mas, na minha visão, o resumo da ópera é: O software faz o que foi especificado previamente, maximizando a geração de valor através de automação para quem vai utilizar. 

**Quando falamos de de Manutenibilidade**, podemos começar com um resumo trazido pela própria ISO: "_Maintainability can be interpreted as either an inherent capability of the product or system to facilitate maintenance activities, or the quality in use experienced by the maintainers for the goal of maintaining the product or system_". Em tradução feita pelo Google translate temos o seguinte: "_A capacidade de manutenção pode ser interpretada como uma capacidade inerente do produto ou sistema para facilitar as atividades de manutenção ou a qualidade em uso experimentada pelos mantenedores com o objetivo de manter o produto ou sistema_"

Para que isso seja possível, essa característica é subdividida em cinco outras:

1. Modularity(grau em que um sistema ou programa de computador é composto de componentes de tal modo que uma mudança em um componente tenha impacto mínimo em outros componentes);
2. Reusability(grau em que uma parte pode ser usada em mais de um sistema, ou na construção de outras partes)
3. Analysability(grau de eficácia e eficiência com o qual é possível avaliar o impacto em um produto ou sistema de uma alteração pretendida em uma ou mais de suas partes, ou diagnosticar um produto quanto a deficiências ou causas de falhas, ou identificar partes a serem modificadas)
4. Modifiability(grau em que um produto ou sistema pode ser modificado de forma eficaz e eficiente sem introduzir defeitos ou degradar a qualidade do produto existente)
5. Testability(grau de eficácia e eficiência com o qual os critérios de teste podem ser estabelecidos para um sistema, produto ou componente e os testes podem ser realizados para determinar se esses critérios foram atendidos). 

Não parece haver muita discussão sobre adequação funcional, **o que vai ser construído precisa atender o desejo de alguém**. Então este documento não investe esforço em reforçar a importância disso. 

Vamos colocar nossa energia sobre a característica da Manutenibilidade e para isso vamos escolher duas subcaracterísticas.

* Modifiability
* Testability

Modularização(Modularity) é importante mas, neste momento, entendo que está mais conectadas com necessidades de negócio do que com o esforço de manter o software em si. Algumas vezes a especificação já direciona para a necessidade de flexibilidade, de vez em quando não. Para ser sincero, entendo que flexibilidade é algo raro dentro dos sistemas produzidos na indústria. O mesmo vale para a necessidade de reuso(Reusability) de um recurso do sistema. Este playbook entende que tais subcaracterísticas são bem mais necessárias em projetos no estilo de frameworks.  

A aposta deste playbook é que o que mais muda num projeto de indústria é cada uma das pessoas que fazem parte das equipes em determinado intervalo de tempo, assim como o escopo. E, considerando este contexto, o direcionamento aqui é facilitar modificação e capacidade de testar se o software faz o que se espera dele. 

No futuro Analysability deve fazer parte deste playbook. Alberto acredita que documentação é chave para esta subcaractarística e ele ainda precisa ser uma versão melhor nesta parte. 


## Agora vamos voltar a trinca qualidade, prazo e entrega

### Qualidade não é negociável

Existem diversas publicações indicando que o tempo e custo de manutenção(evolutiva, corretiva, adaptativa) aumentam demais durante o ciclo de vida do software, criando um gargalo na capacidade produtiva da equipe como um todo e afetando a capacidade da maximizar sua geração de valor no mercado. 

Por conta disso, no olhar deste playbook, qualidade é algo que deve ser entregue de maneira padrão. A ideia é que fazer algo seguindo a régua de qualidade aqui do playbook não seja algo a mais, seja o comum. 

### Sobre prazos(deadlines)

Deadlines podem ser estabelecidos de algumas formas. Alguns exemplos são: Através de uma negociação entre partes, prazo legal(estabelecido por lei), algum comprometimento público feito e que por algum motivo não pode ser modificado etc. 

Idealmente todo prazo deveria ser estabelecido, ou pelo menos aceito, pela equipe que vai realizar o trabalho de fato. Tal equipe deveria ter a principal voz sobre o tempo necessário para a realização de algo, dado que isso fala diretamente com a percepção e capacidade real de cada indivíduo ali dentro. Claro que as lideranças, clientes etc podem opinar mas, decidir pela equipe, pode ser considerado uma intransigência. 

Caso exista um prazo que não pode ser alterado de forma alguma, como aconteceu com o lançamento do PIX no Brasil, a equipe deveria ser consultada e retornar com uma proposta para buscar o prazo. E a empresa deveria ser capaz de satisfazer as necessidades da equipe. 

Em caso de possibilidade de alteração do deadline, sugiro a renegociação e nova tentativa. 

Deadlines deveriam ser usados para dar ritmo para o time e não para colocar pressão desnecessária.

### Sobre o que vai ser entregue

Aqui, mais uma vez, a equipe exerce o papel central. Só às próprias pessoas podem dizer o que são capaz de entregar num determinado intervalo de tempo. Não é porque existe um prazo não negociável que as pessoas automagicamente se tornam capazes.

Só que aqui na entrega existem alguns outros pontos que podem ser trabalhados para realmente sairmos com o mínimo de coisas que vão gerar o maior impacto possível. 

* Fazer análise de causa raiz de cada funcionalidade necessária (até para entender o nível de entendimento para cada uma delas);
* Ter uma priorização clara sobre o que precisa ser feito (vai ser importante para entender o que pode ser deixado para trás);

**Análise de causa raiz** é importante porque gera um entendimento mais profundo sobre cada necessidade. Inclusive pode ter algo sendo feito sem nenhum entendimento por parte da pessoa que pediu sobre a necessidade. Feeling também pode ganhar jogo :). Não é porque não passou pelo filtro dos N porquês que não precisa ser feito. A falta de entendimento é um sinal sobre a falta de maturidade da necessidade, mas é apenas uma dimensão. 

**A priorização** é importante para facilitar a decisão do que pode ser deixado para trás num momento de decisão. 

No final a sugestão aqui é parecida com o prazo. Se nada da entrega pode cair, é necessário verificar com a equipe o que é precisa ser feito para manter tudo que tinha sido combinado. 

Agora se existe flexibilidade na entrega, é necessário olhar para a priorização e entender o ponto de corte. 

## Resumo da visão do playbook sobre a trinca formada por qualidade, prazo e entrega. 

Negocie prazos e entregas, nunca qualidade pelo viés da manutenibilidade e adequação funcional.  

## E como buscamos a melhora na manutenibilidade ?

Bom, aqui temos algumas subcaracterísticas que precisamos buscar. 

* Modifiability
* Testability


## Sobre Modifiability

Relembrando a definição formal pela ISO referenciada: "grau em que um produto ou sistema pode ser modificado de forma eficaz e eficiente sem introduzir defeitos ou degradar a qualidade do produto existente". 

### Defina um limite de complexidade para as unidades de código

Toda equipe deve definir uma forma clara de controlar a complexidade de código. Isso quer dizer que qualquer pessoa da equipe, pouco importa o nível de 
expertise, precisa ser capaz de olhar para uma unidade de código a ser definida pela equipe(arquivo,método,classe) e falar se aquilo está dentro, acima ou abaixo do limite definido pela equipe. Para que isso fique claro vamos observar o código abaixo:

```java
public class Solution {

	public static String processMessages(List<String> messages) {
		Map<String, Logica> logicas = Map.of("proposal.created",
				new CriaProposta(), "proponent.added", new AdicionaProponente(),
				"warranty.added", new AdicionaGarantia());
		Propostas propostas = new Propostas();
		List<Validacao> validacoes = List.of(new ValorDoEmprestimo(),
				new TempoMaximoPagamento(), new MinimoDoisProponentes(),
				new MinimoUmProponentePrincipal(),
				new TodosProponentesDevemSerMaioresDeDezoito(),
				new RendaProponentePrincipal(), new MinimoDeUmaGarantia(),
				new GarantiaDeDeterminadosEstadosNaoSaoAceitas(),
				new SomaDasGarantiasMaiorQueDobroEmprestimo());
		Set<String> idPropostasValidas = new LinkedHashSet<>();

		for (String message : messages) {
			String[] partesDaMensagem = message.split(",");
			String tipoLogica = partesDaMensagem[1] + "." + partesDaMensagem[2];

			Logica logicaASerExecutada = logicas.get(tipoLogica);

			Objects.requireNonNull(logicaASerExecutada,
					"Não foi possível encontrar a lógica para o tipo "
							+ tipoLogica);
			logicaASerExecutada.executa(message, propostas);
		}

   	        return propostas.stream()
		 .filter(proposta -> {
			return validacoes.stream().allMatch(validacao -> validacao.taValida(proposta))
		 })
	         .map(proposta -> proposta.id)
	         .collect(Collectors.joining(",");
    }
}
```
Todo mundo na sua equipe deveria analisar a complexidade deste código do mesmo jeito. Ou seja, todo mundo deveria dar a mesma a resposta sobre a necessidade de refatoração deste código. Além disso, se existe um limite claro, existe a garantia que a refatoração vai resultar num código mais bem avaliado do que o atual(sim, isso nem sempre é verdade). 

### E como eu poderia definir um limite?

Sugiro usar a linha de design de código chamada **Cognitive Driven Development** para derivar métricas de entendimento e limites em cima das métricas. Essa linha de design acredita que se um código que pode ser entendido, pode ser mantido. E isso fala diretamente com a subcaracterística **Modifiability**. 

Em primeiro lugar a equipe precisa definir o que é uma unidade de código. 
 * É um arquivo?
 * É um método/função?
 * É uma classe (caso esteja sendo usada uma linguagem que segue o paradigma Orientada a Objetos) ?

A sugestão aqui é considerar um arquivo como unidade a ser analisada a dificuldade de entendimento. 

Depois disso, toda equipe, independente do tipo de linguagem, stack etc escolhida, precisa definir um conjunto de itens que dificultam entendimento. Atualmente a sugestão é sempre escolher no mínimo três e no máximo cinco. Estas quantiadades já foram escolhidas por outras equipes. Cada item escolhido conta um ponto de entendimento naquela unidade de código. Para o conjunto de itens, vamos usar a expressão **métrica derivada do CDD**.

Agora é necessário estabelecer um limite de pontos de dificuldade de entendimento por unidade de código. A sugestão atual é seguir a seguinte conta simples: **limite** >= número de itens * 2. Alguns exemplos:
 * Três itens na métrica? Limite tem que ser 6 ou mais. 
 * Quatro itens na métrica? Limite tem que ser 8 ou mais.
 * Cinco itens na métrica? Limite tem que ser 10 ou mais. 

Com a unidade, métrica derivada do CDD e limite definidos, sua equipe tem um direcionamento claro de controle de complexidade do código. 

### Regra clara para criação de novas unidades de código

Nenhuma nova unidade de código pode passar do limite estabelecido pela equipe. Isso garante que pouco importa a expertise da pessoa, não vão existir unidades com complexidade maiores que a acordada. Isso traz a flexibilidade necessária para termos soluções interessantes, porém estabelece o limite visando maximizar a chance daquela solução ser entendida pela próxima pessoa. 

### Regra clara para controle de complexidade em alterações

Caso o código já exista, o controle de complexidade se mantém. Quando uma unidade for sofrer alteração, pouco importa o motivo, a pessoa vai conseguir estabelecer os pontos de complexidade dela e decidir se a alteração vai ser toda acomodada ali dentro ou se vai precisar de novas unidades de código. 

### Regra clara para revisão de código

Faz o que precisa ser feito e passou do limite de complexidade? Não passa na revisão. Faz o que precisa ser feito e está dentro do limite de complexidade? Passa na revisão. 

A parte legal é que isso não impede o(a) revisor(a) de opinar. A pessoa que está revisando pode ter uma ideia de solução que utilize menos recurso computacional e fique dentro da complexidade, por exemplo. Ou na revisão pode ser percebido que menos unidades eram necessárias para fazer a mesma coisa. 

### Voltando para a análise do código anterior

Dado que temos a seguinte definição em cima do CDD:

* **Unidade de código?** Arquivo
* **Métrica derivada do CDD?**
  * Branch de código -> 1 ponto
  * Acoplamento com classe específica do projeto -> 1 ponto
  * Passagem de função como argumento -> 1 ponto
* **Limite inicial?** 7 pontos. 

Vamos analisar o arquivo abaixo. 

```java
public class Solution {

	public static String processMessages(List<String> messages) {
	        //1 -> Classe Logica
		Map<String, Logica> logicas = Map.of(
				//1 -> classe CriaProposta 
				"proposal.created",new CriaProposta(), 
				//1 -> classe AdicionaProponente
				"proponent.added", new AdicionaProponente(),
				//1 -> classe AdicionaGarantia 
				"warranty.added", new AdicionaGarantia());
		//1 -> classe Propostas
		Propostas propostas = new Propostas();
		//1 -> classe Validacao
		List<Validacao> validacoes = List.of(
				//1 -> classe ValorDoEmprestimo 
				new ValorDoEmprestimo(),
				//1 -> classe TempoMaximoPagamento 
				new TempoMaximoPagamento(), 
				//1 -> classe MinimoDoisProponentes 
				new MinimoDoisProponentes(),
				//1 -> classe MinimoUmProponentePrincipal 
				new MinimoUmProponentePrincipal(),
				//1 -> classe TodosProponentesDevemSerMaioresDeDezoito 
				new TodosProponentesDevemSerMaioresDeDezoito(),
				//1 -> classe RendaProponentePrincipal 
				new RendaProponentePrincipal(), 
				//1 -> classe MinimoDeUmaGarantia 
				new MinimoDeUmaGarantia(),
				//1 -> classe GarantiaDeDeterminadosEstadosNaoSaoAceitas 
				new GarantiaDeDeterminadosEstadosNaoSaoAceitas(),
				//1 -> classe SomaDasGarantiasMaiorQueDobroEmprestimo 
				new SomaDasGarantiasMaiorQueDobroEmprestimo());
		Set<String> idPropostasValidas = new LinkedHashSet<>();

		//1 -> for
		for (String message : messages) {
			String[] partesDaMensagem = message.split(",");
			String tipoLogica = partesDaMensagem[1] + "." + partesDaMensagem[2];

			Logica logicaASerExecutada = logicas.get(tipoLogica);

			Objects.requireNonNull(logicaASerExecutada,
					"Não foi possível encontrar a lógica para o tipo "
							+ tipoLogica);
			logicaASerExecutada.executa(message, propostas);
		}

   	        return propostas.stream()
		//1 -> funcao passada para o filter
		 .filter(proposta -> {
		 	//1 -> funcao passada para o allMatch
			return validacoes.stream().allMatch(validacao -> validacao.taValida(proposta))
		 })
		 //1 -> funcao passada para o map
	         .map(proposta -> proposta.id)
	         .collect(Collectors.joining(",");
    }
}
```

Caso a conta manual não tenha falhado, temos uma unidade de código com 19 pontos. Dado que o limite é 7, ela não pode ficar assim. A parte legal é que dá até para estimar quantas unidades vão ser necessárias para comportar todo esse código. Outro ponto interessante é que você consegue, visualmente, identificar qual trecho machuca mais a complexidade. Nesta unidade, o principal problema é a construção da lista com as implementações de validação. 

## Sobre Testability 

Lembrando que pela definição formal, nós temos: "grau de eficácia e eficiência com o qual os critérios de teste podem ser estabelecidos para um sistema, produto ou componente e os testes podem ser realizados para determinar se esses critérios foram atendidos". 


