## O que é esse documento ?

Neste documento está descrito o sistema que guia a escrita de testes automatizados para a quarta edição da Jornada Dev Eficiente.

## Por que precisamos de um sistema de testes?

Eu entendo que determinado pedaço de código deveria ser testado contra os mesmos valores por todo mundo de uma equipe. Por exemplo:

```java
if(valor <= 2000){
  //algum codigo
}
```

Dado que valor é um inteiro, o teste automatizado para este código escrito pelas pessoas de uma equipe deveria ter pelo menos uma intersecção de valores. Caso
a equipe utilize boundary testing como uma das técnicas de teste, seria obrigatório ter testes com **2000** e **2001**. Se não tem intersecção de valores 
nos casos de teste para um mesmo código, então provavelmente não existe uma ou mais técnicas guiando aquele time. 

A ausência de um sistema claro que guia a escrita de testes, na minha opinião, diminui a chance de você criar uma bateria de testes que realmente seja reveladora
de bugs. Ou seja, dado que um dos principais interesses por trás de escrever testes automatizados seria o aumento de confiabilidade do sistema, você estaria indo
contra o próprio motivo da existência deste tanto de código a mais. 

## Como funciona o meu sistema?

Atualmente o sistema que sigo é pensado para API's. Olhando pelo viés da pirâmide de testes, eu sugiro dois tipos de testes:

* Testes de unidade mais integrado possível respeitando o requisito de execução rápida (inclusive pode-se derivar um requisito de performance aqui)
* Testes de API

Além disso, as técnicas que eu utilizo neste momento são:

* Boundary testing;
* Structural testing MC/DC;
* Property Based Testing;

Supondo que quanto mais eu treino meu código, mais preparado ele fica para funcionar bem em produção, a ideia é ter no mínimo 90% de cobertura bem treinado. O número
90% é inspirado [neste estudo](https://www.researchgate.net/profile/Akbar-Siami-Namin/publication/220854552_The_influence_of_size_and_coverage_on_test_suite_effectiveness/links/577164e508ae0b3a3b7d6e5d/The-influence-of-size-and-coverage-on-test-suite-effectiveness.pdf). 


O problema do 90% aqui é que ele pode servir como incentivo perverso, já que toda mensuração unidimensional tende a ser pobre. Então para um pedaço de código
ser considerado bem testado pelo sistema atual, temos a seguinte combinação de dimensões:

1. Caso o trecho de código tenha condições utilizando valores, como ```java x<100```, precisamos ver o uso dos valores sugeridos pelo Boundary Testing para a derivação dos testes.
2. Caso o trecho de código tenha branches e/ou condicionais precisamos que o número de testes derivados sejam guiados pelo MC/DC. 
3. Só usa Mock se tocar em sistema externo e essa integração fazer com que o teste execute de maneira lenta(lenta aqui ainda é abstrato, então é algo a ser melhorado). 
4. O teste pode usar Property Based Testing para treinar o código com valores que seriam complicados de imaginar. 
5. Cada teste precisa ter uma ou mais asserções de modo a minimizar a chance de termos falsos positivos. 
6. O código de produção deve ter cobertura maior ou igual a 90%. 

Dessa forma eu entendo que o sistema empurra a qualidade da bateria de testes para cima, aumentando a chance de revelar bugs. Esse sistema deve ser evoluído com
o tempo. Podemos falar de testes mutantes, fuzzing etc. 

## Sugestão da busca dos 90% ou mais combinando tipos e técnicas de testes

Para buscar o percentual de cobertura proposto respeitando as dimensões e executando o mais rápido possível, sugiro a seguinte combinação. 

1. Sempre que você encontrar um código que tenha uma branch/condicional escrita de maneira explícita, crie um teste de unidade o mais integrado possível.
2. Sempre que você encontrar um código que não tenha branch/condicional e que não foi tocado pelos testes de unidade mais integrados, crie um teste de API para o endpoint que vai forçar aquela execução. 
3. Para os testes criados no item 2, eu sugiro a utilização de property based testing para que valores sejam gerados de maneira automática e treinem seu código de maneira mais exigente do que você faria. 

## O que é uma branch/condicional escrita de maneira explícita?

Vamos ver um primeiro exemplo de código com branch/condicional escrita de maneiria explícita:

```java
		return this.valorEmprestimo.compareTo(minimo) > 0 
				&& this.valorEmprestimo.compareTo(maximo) < 0;
```
Acima o código tem a utilização de um operador **AND** sendo usado explicitamente. Para este caso é necessário que três testes sejam derivados, seguindo a proposta
do MC/DC. Além disso também é necessário que exista um teste onde o _valorEmprestimo_ seja igual ao _minimo_, assim como um teste onde o _valorEmprestimo_ seja igual
ao _máximo_. Este é um caso legal porque usamos Boundary Testing para derivar valores e MC/DC para derivar os testes em si. 

Agora vamos ver mais um código que executa uma branch/condicional de maneira explícita:

```java
		if (!adicionou) {
			throw new IllegalStateException(
					"Foi tentado adicionar um proponente com equals true com esse daqui " + novoProponente);
		}
```

Aqui temos uma branch cuja condição com apenas uma expressão dentro. Pelo MC/DC precisamos de dois testes aí. Um que entra no **if** e outro que não entra. 

Agora vamos ver um código que executa uma branch/condicional de maneira implícita:

```java
		return diaEsperadoRetorno
				.isBefore(LocalDate.now());
```
Se existe um método retornando booleano, é porque em algum momento do fluxo de execução dele, vai ter uma branch/condicional. Entretanto, olhando só para este 
código, não vemos nenhuma branch/condicional. E se não estamos enxergando, eu sugiro não criar teste de unidade. Esse pedaço de código pode ser exercitado
por algum outro teste de unidade integrado que toque nele ou por um teste de API. 

Se você quiser criar um teste de unidade para ele você pode, é claro, mas a sugestão é que você treine pedaço de código pelos outros caminhos citados. Se vai testar
esse através de teste de unidade, aí eu sugiro usar o property based testing para trabalhar com valores que você não tem tanto controle. 

## Combine de forma criativa

Perceba que você pode buscar todas as dimensões utilizando apenas testes de api ou apenas testes de unidade. O importante é respeitar as regras do jogo. Se você
construiu uma API que não tem uma branch/condicional, pode mandar bala só em teste de API com property based testing. Do mesmo jeito que poderia fazer um teste
de unidade o mais integrado possível com Property Based Testing. 

No geral eu vou de combinação porque entendo que ganho uma boa velocidade de execução mantendo uma boa chance de revelar erros o mais cedo possível.

## Sobre self testing (opcional)

Esta é uma técnica que eu considero que aumenta demais a confiabilidade do código. Cada método ter a chance de verificar suas pré e pós condições é algo que considero precioso. Chegou um parâmetro com valor errado num lugar que supostamente não poderia? É um bug! O fluxo precisa ser encerrado. Um exemplo de código aqui:

```java
	public Emprestimo criaEmprestimo(@NotNull @Valid Livro livro,
			@Positive int tempo) {
		Assert.state(livro.aceitaSerEmprestado(this),
				"Você está gerar um emprestimo de um livro que não aceita ser emprestado para o usuario "
						+ this.getId());
		Assert.state(livro.estaDisponivelParaEmprestimo(),
				"O livro precisa estar disponível para empréstimo para ser emprestado");
		Assert.state(this.aindaPodeSolicitarEmprestimo(),
				"Este usuário já está no limite de empréstimos");
				
		...
	}
```

O código acima é de um método que cria empréstimo para um usuário em função de um livro e um tempo. Neste ponto da execução é necessário que todas aquelas 
asserções sejam verdadeiras. Caso qualquer uma falhe temos um bug e precisamos de intervenção humana para fazer a correção. Perceba que o método não precisa 
saber o que aconteceu antes e nem o que vai acontecer depois. Ele só tem visão do que é necessário para aquele escopo. 

## Este é um sistema em evolução

Claro que eu posso mudar de ideia e mudar o sistema. Não é sobre ter o melhor sistema e sim sobre criar uma forma de padronizar a escrita de testes dentro de uma 
equipe de modo a criar uma suite de testes que maximize a revelação de bugs. 

## Referências

Neste momento, a fonte que mais me inspira e que eu considero que ainda estou no caminho de tirar proveito, é o livro aberto de Mauricio Aniche sobre testes.
Você pode acessá-lo [aqui](https://sttp.site/). 




