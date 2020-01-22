# Meus pilares para a criação de um código

* A prioridade maxima é funcionar de acordo com o caso de uso. Beleza e formosura não dão pão nem fartura.

* Execute o seu código o mais rápido possível. Estou falando de execução real, não de testes automatizados.

* Protegemos as bordas do sistema como se não houvesse amanhã. 

* Quanto mais externa a borda, mais proteção temos. 

* Não retornamos nulo dentro das regras da aplicação. Pense que seu computador vai explodir.

* Separamos as bordas externas do sistema do seu núcleo. Não ligamos parâmetros de requisição externa com objetos de domínio diretamente, assim como não serializamos objetos de domínio para respostas de API.

* Avaliamos de forma lógica o nível de complicação de cada trecho de código. Aqui não tem espaço para feeling. Usamos a teoria da carga cognitiva e medimos o nível de pontos de entendimentos necessário por trecho.

* A complicação do nosso é código é proporcional a complicação da nossa feature. Quanto mais simples, melhor.

* Usamos tudo que conhecemos que está pronto. Só fazemos código do zero se for estritamente necessário. 

* Idealmente, todo código escrito deveria ser chamado por alguém. Se não tem ninguém chamando, ele não deveria existir.

* Só alteramos estado de referências que criamos. Não mexemos nos objetos alheios. A não ser que esse objeto seja criado para isso, como é o caso de argumentos de métodos de borda mais externa. Estes são, geralmente, associados a frameworks.

* Você precisa entender o que está usando e olhar sempre o lado negativo de cada decisão. 

* Se você acha tudo bom, quer dizer que você não está entendendo nada.

* Deixamos pistas que facilitem o uso do código onde não conseguimos resolver com compilação. 

* Preferimos testes de unidade a outros testes. Não morremos se eles só forem criados depois.

* Criamos testes de unidade com duas finalidades: ter um local que reflita a maioria das lógicas executadas pela aplicação e para nos ajudar a desenvolver uma lógica que não sabemos direito como usar. 
