# Configuração de Quality Gate sugerida

* Coverage	is less than 40.0% (estou a espera de evidências que uma régua de testes automatizados seja útil. Vou adorar mudar de ideia)
* Duplicated Lines	is greater than	3% (fui com o default do sonar)
* Maintainability Rating is worse than	A (acho que devemos concentrar esforço aqui, acho que precisa ser A)
* Reliability Rating is worse than A (também acho esse acompanhamento importante. O quão confiável é nosso código)


# Como assim 40% de cobertura de testes?

Por mais que as pessoas propaguem a importância de testes automatizados dos mais variados sabores, ninguém conseguiu estabelecer uma relação direta entre a criação deles e qualidade de código. Existem alguns argumentos que são usadas regularmente nas conversas:

* É uma documentação viva do software. A pessoa chega e já vai nos testes. 
* Você se sente mais confortável na hora de realizar uma alteração ou refatoração no código
* Você aumenta as chances de entregar a funcionalidade pedida realmente funcionando de maneira correta
* Ajuda a realizar os testes de regressão

Todos estes e outros talvez sejam válidos e eu também sou adepto de escrever testes. O problema que eu enxergo no mercado é o quanto. Um incentivo potencialmente ruim pode ser criado quando você estabelece uma régua super alta para um código. A pessoa tende a buscar aquela régua, muitas vezes gamificando o objetivo mesmo. É justamente para isso que serve um objetivo, direcionar. Se você direcionou para cobertura alta, você vai ganhar cobertura alta. 

A questão toda é que as empresas relacionam cobertura com qualidade de código, e aí vira algo similar a horóscopo. Como a maioria das pessoas de computação não acreditam em horóscopo, elas também não deveriam acreditar que testes melhoram a qualidade. Quando falo qualidade, eu me refiro a um código que busque ser fácil de entender seguindo a Teoria da Carga Cognitiva. Aqui você consegue ter uma noção dela => https://www.sonarsource.com/resources/white-papers/cognitive-complexity.html . Caso queira ir além, pode procurar pelo livro Software Design, cognitive aspects.  

Por enquanto, nenhuma pesquisa conseguiu estabelecer a relação direta entre escrever testes automatizados(com ou sem TDD) e qualidade de código. Então eu, Alberto, não acho que você e sua empresa deveriam fazer disso um mantra. 

Então, por conta de tudo isso, eu sugiro começar com uma busca de cobertura baixa e acompanhar 
o código produzido. 

# E onde o Security Rating do quality gate?

Pois é, eu tirei. Eu analisei as regras do sonar que são aplicadas para criar a visão de segurança e eu considero que a maioria delas vive fora código de negócio da aplicação em si. Então não vejo porque ficar avaliando isso. É óbvio que você pode habilitar isso no seu projeto, mas aí você vai precisar aumentar o quality profile que foi definido por mim. Ele é fortemente baseado em manutenção. 