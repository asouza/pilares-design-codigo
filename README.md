# Meus pilares para a criação de um código

* Qualidade não é negociável. O código deve ser feito com o design proporcional a complexidade considerando os conhecimentos que temos no momento na produção

* A prioridade máxima é funcionar de acordo com o caso de uso. 

* Execute o seu código o mais rápido possível. Priorize implementar de fora para dentro, dessa forma você visualiza o que realmente precisa e usar uma abordagem mais incrmental. O "fora" aqui pode ser o endpoit que vai receber uma chamada, pode ser seu teste automatizado...

* Protegemos as bordas do sistema como se não houvesse amanhã. 

* Quanto mais externa a borda, mais proteção temos. 

* Não retornamos nulo dentro das regras da aplicação. Pense que seu computador vai explodir.

* Separamos as bordas externas do sistema do seu núcleo. Não ligamos parâmetros de requisição externa com objetos de domínio diretamente, assim como não serializamos objetos de domínio para respostas de API.

* Avaliamos de forma lógica o nível de complicação de cada trecho de código. Aqui não tem espaço para feeling. Usamos a teoria da carga cognitiva e medimos o nível de pontos de entendimentos necessário por trecho. Uma leitura que explica melhor o assunto https://github.com/asouza/pilares-design-codigo/blob/master/ddd-da-massa.md
* Toda indireção aumenta a dificuldade de entendimento da aplicação como um todo, ela precisa merecer existir. Ou seja, precisa ajudar a distribuir a carga intrínseca pelo sistema. 

* Usamos o construtor para criar o objeto no estado válido.

* A complicação do nosso é código é proporcional a complicação da nossa feature. Quanto mais simples, melhor.

* Usamos tudo que conhecemos que está pronto. Só fazemos código do zero se for estritamente necessário. 

* Idealmente, todo código escrito deveria ser chamado por alguém. Se não tem ninguém chamando, ele não deveria existir.

* Só alteramos estado de referências que criamos. Não mexemos nos objetos alheios. A não ser que esse objeto seja criado para isso, como é o caso de argumentos de métodos de borda mais externa. Estes são, geralmente, associados a frameworks.

* A versão mais eficiente de uma pessoa programando é aquela que entende, questiona e implementa estritamente o que foi combinado. Não inventamos coisas que não foram pedidas, não fazemos suposição de funcionalidade e nem caímos na armadilha de achar que entendemos mais do que a pessoa que solicitou a funcionalidade.

* Você precisa entender o que está usando e olhar sempre o lado negativo de cada decisão. 

* Deixamos pistas que facilitem o uso do código onde não conseguimos resolver com compilação. 

* A sua api deve deixar claro o caminho que deve ser seguido pelo ponto do código que decide usá-la. Não espere que ninguém lembre de invocar nada. Faça de tudo para gerar obrigações. Quanto mais específico é seu código, menos democrático ele é. 

* Não usamos exception para controle de fluxo.

* Regras de negócio devem ser declaradas de maneira explícita na nossa aplicação. 

* Favorecemos a coesão através do encapsulamento.

* Criamos testes automatizados para que ele nos ajude a revelar e consertar bugs na aplicação. 


# Informações complementares

* Diminua a complexidade do código observando a carga cognitiva dele - https://github.com/asouza/pilares-design-codigo/blob/master/design-orientado-a-carga-cognitiva.md
* Um pouco da explicação sobre o meu quality profile do sonar - https://github.com/asouza/pilares-design-codigo/blob/master/quality-gate-deveficiente.md
* Quality gate para seu projeto analisado pelo sonar - https://github.com/asouza/pilares-design-codigo/blob/master/sonar-rules-deveficiente.xml
