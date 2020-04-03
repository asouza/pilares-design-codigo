# Meus pilares para a criação de um código

* A prioridade máxima é funcionar de acordo com o caso de uso. Beleza e formosura não dão pão nem fartura.

* Execute o seu código o mais rápido possível. Estou falando de execução real, não de testes automatizados.

* Protegemos as bordas do sistema como se não houvesse amanhã. 

* Quanto mais externa a borda, mais proteção temos. 

* Não retornamos nulo dentro das regras da aplicação. Pense que seu computador vai explodir.

* Separamos as bordas externas do sistema do seu núcleo. Não ligamos parâmetros de requisição externa com objetos de domínio diretamente, assim como não serializamos objetos de domínio para respostas de API.

* Avaliamos de forma lógica o nível de complicação de cada trecho de código. Aqui não tem espaço para feeling. Usamos a teoria da carga cognitiva e medimos o nível de pontos de entendimentos necessário por trecho. Uma leitura simples e que introduz bem o assunto https://www.sonarsource.com/resources/white-papers/cognitive-complexity.html

* A complicação do nosso é código é proporcional a complicação da nossa feature. Quanto mais simples, melhor.

* Usamos tudo que conhecemos que está pronto. Só fazemos código do zero se for estritamente necessário. 

* Idealmente, todo código escrito deveria ser chamado por alguém. Se não tem ninguém chamando, ele não deveria existir.

* Só alteramos estado de referências que criamos. Não mexemos nos objetos alheios. A não ser que esse objeto seja criado para isso, como é o caso de argumentos de métodos de borda mais externa. Estes são, geralmente, associados a frameworks.

* A versão mais eficiente de uma pessoa programando é aquela que questiona e implementa estritamente o que foi descrito. Não inventamos coisas que não foram pedidas, não fazemos suposição de funcionalidade e nem caímos na armadilha de achar que entendemos mais do que a pessoa que solicitou a funcionalidade.

* Você precisa entender o que está usando e olhar sempre o lado negativo de cada decisão. 

* Se você acha tudo bom, quer dizer que você não está entendendo nada.

* Deixamos pistas que facilitem o uso do código onde não conseguimos resolver com compilação. 

* A sua api deve deixar claro o caminho que deve ser seguido pelo ponto do código que decide usá-la. Não espere que ninguém lembre de invocar nada. Faça de tudo para gerar obrigações. Quanto mais específico é seu código, menos democrático ele é. 

* Não usamos exception para controle de fluxo.

* Regras de negócio devem ser declaradas de maneira explícita na nossa aplicação. 

* Métodos criados nas classes que possuem os atributos que representam o estado da aplicação devem retornar algo em função do seu estado interno ou alterar o seu próprio estado interno.

* Criamos testes automatizados com duas finalidades: ter um local que reflita a maioria das lógicas executadas pela aplicação e para nos ajudar a desenvolver uma lógica que não sabemos direito como usar. 


# Informações complementares

Aqui você também encontra uma sugestão de quality profile para utilização no Sonar junto a seu projeto Java. Assim como uma sugestão de quality gate. 
