# Meus pilares para a criação de um código

* Qualidade não é negociável. O código deve ser feito com o design proporcional a complexidade considerando os conhecimentos que temos no momento na produção

* A prioridade máxima é funcionar de acordo com o caso de uso. 

* Execute o seu código o mais rápido possível. Priorize implementar de fora para dentro, dessa forma você visualiza o que realmente precisa e usa uma abordagem mais incrmental. O "fora" aqui pode ser o endpoint que vai receber uma chamada, pode ser seu teste automatizado...

* Protegemos as bordas do sistema como se não houvesse amanhã. 

* Quanto mais externa a borda, mais proteção temos. 

* Não retornamos nulo dentro das regras da aplicação. Pense que seu computador vai explodir.

* Separamos as bordas externas do sistema do seu núcleo. Não ligamos parâmetros de requisição externa com objetos de domínio diretamente, assim como não serializamos objetos de domínio para respostas de API.

* Avaliamos de forma lógica o nível de complicação de cada trecho de código. Aqui não tem espaço para feeling. Usamos a teoria da carga cognitiva e medimos o nível de pontos de entendimentos necessário por trecho. Uma leitura que explica melhor o assunto https://github.com/asouza/pilares-design-codigo/blob/master/ddd-da-massa.md
  
* Toda indireção tende a aumentar a dificuldade de entendimento da aplicação como um todo, ela precisa merecer existir. Ou seja, precisa ajudar a distribuir a complexidade pelo sistema. 

* Usamos o construtor para criar o objeto no estado válido.

* Usamos tudo que conhecemos que está pronto. Só fazemos código do zero se for estritamente necessário. 

* Idealmente, todo código escrito deveria ser chamado por alguém. Se não tem ninguém chamando, ele não deveria existir.

* Só alteramos estado de referências que criamos. Não mexemos nos objetos alheios. A não ser que esse objeto seja criado para isso.

* A versão mais eficiente de uma pessoa programando é aquela que entende, questiona, refina e, uma vez combinado, implementa estritamente o que foi combinado. Não inventamos coisas que não foram pedidas e não fazemos suposição de funcionalidade.

* Você precisa entender o que está usando e olhar sempre o lado negativo de cada decisão. 

* Deixamos pistas que facilitem o uso do código onde não conseguimos resolver com compilação. 

* A sua api deve deixar claro o caminho que deve ser seguido pelo ponto do código que decide usá-la. Não espere que ninguém lembre de invocar nada. Faça de tudo para gerar obrigações. Pense em API's não democráticas. 

* Sinalizamos de maneria nítida quando um problema, de qualquer natureza, acontece na execução de um fluxo. Você pode sinalizar com exceptions, abstrações extras etc. 

* Regras de negócio devem ser declaradas de maneira explícita na nossa aplicação. 

* Favorecemos a coesão através do encapsulamento.

* Criamos testes automatizados para aumentar a confiabilidade da aplicação e, consequentemente, aumentar a confiança da equipe para realizar as alterações que são necessárias.

* Testes automatizados devem ser derivados de maneira pragmática através das técnicas já conhecidas. Só depois de derivar casos padrões, usamos nossa criatividade para buscar extrapolar. 


# Informações complementares

* Diminua a complexidade do código observando a carga cognitiva dele - https://github.com/asouza/pilares-design-codigo/blob/master/design-orientado-a-carga-cognitiva.md
* Um pouco da explicação sobre o meu quality profile do sonar - https://github.com/asouza/pilares-design-codigo/blob/master/quality-gate-deveficiente.md
* Quality gate para seu projeto analisado pelo sonar - https://github.com/asouza/pilares-design-codigo/blob/master/sonar-rules-deveficiente.xml
