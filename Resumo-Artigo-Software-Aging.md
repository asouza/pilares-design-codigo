## Informações técnicas do artigo:

* Fonte - http://www.inf.ed.ac.uk/teaching/courses/seoc/2004_2005/resources/bullet11.pdf
* Título - Software Aging
* Ano: 1994
* Autor: David Parnas

## Resumo

É explicado que o envelhecimento do software é um fato, assim como na vida. E também, assim como na vida, deveríamos podemos tomar atitudes para que esse envelhecimento seja menos danoso, na parte da saúde,para a gente. Ele traz uma frase que eu considero muito boa:

"A sign that the Software Engineering profession has matured will be that we lose our preoccupation with the first release and focus on the long term health of our products"

Para fechar ele fala que praticantes e pesquisadores devem mudar sua forma de pensar e, só aí, engenharia de software deve ser chamada de engenharia. 

## What nonsense!

Aqui ele reforça o que fala no resumo e apresenta de maneira mais geral o propósito do artigo. Começa com uma brincadeira sobre o lance que software é uma coisa matemática e que se determinada formula funciona agora, ela vai continuar funcionando daqui 200 anos. Logo abaixo ele fala que por mais que isso possa ser verdade, não se aplica a software. 

De novo ele traz o paralelo com o envelhecimento humano, diz isso que isso vai acontecer e que devemos estar preparados para o fato. Traz o ponto que software está cada vez mais no coração de muitos negócios e que precisamos ficar ainda mais preparados para este envelhecimento. 

## 2. The causes of software aging

Abre rapidamente que tem duas principais causas para um software envelhecer. A primeira é que o software não conseguiu ser modificado para atender as necessidades do negócio e aí, naturalmente, ele vai ficando mais velho. A segunda é justamente pelas mudanças que foram feitas em cima daquele software com o passar do temp. 

### 2.1 Lack of movement

A primeira causa aqui é literalmente a incapacidade de atualizar software da maneira que é esperada pelo mercado. Ele cita que antigamente era normal você esperar horas e até dias para que seu programa pudesse ser compilado e depois executado. Isso hoje não é mais aceitável. 

### 2.2 Ignorant surgery

Aqui é explicado que as mudanças feitas no decorrer da vida de um software vão sendo feitas por pessoas que sabem cada vez menos sobre as decisões de design iniciais e também sobre o negócio em si que está tentando ser resolvido. 

O empilhamento dessas alterações vai levando o software para um estado onde as mudanças vão demorando cada vez mais além de se serem cada vez mais propensas a gerar novos bugs no projeto. 

No fim desta seção ele traz a primeira crítica a falta de cuidado na atualização da documentação. 

## 3 Kidney failure

Aqui é uma seção rápida onde ele fala de problemas de alocação de memória, disco etc que podem ter sido causados pelas alterações indevidas no software. É algo que parece mais tratado nos dias de hoje. 

## 4 The costs of software aging

Aqui ele abre com uma lista de custos que aparecem por conta do envelhecimento do software. 

1. Os donos do produto percebem que está muito complicado manter o ritmo de atualização quando comparado a novos produtos e aí perdem clientes para estes novos produtos. 
2. Volta a falar que o software que está envelhecendo também começa a mostrar a ter uma performance pior por conta da sua estrutura cada vez pior. 
3. E volta a falar dos bugs, dizendo que o software envelhecendo começa a ficar meio buggy com o tempo. 

### 4.1 Inability to keep up

Aqui é trazido o "peso" que o software vai ganhando no sentido da dificuldade de manutenção. Com o tempo as mudanças empilhadas e a falta de documentação adequada faz com que todas as alterações fiquem mais demoradas. Além disso é dito que alterações que eram feitas em poucos lugares, agora refletem em múltiplos lugares diferentes. 

Para fechar ele traz o fato que a falta de alterações impacta diretamente nos rendimentos da empresa assim como o fato de tentar manter o ritmo de atualizações também acaba gerando um custo crescente.

### 4.2 Reduced performance 

Aqui é trazido a observação que com o tempo o programa consome mais memória e também vai performando cada vez pior. Tudo parece verdade nos dias de hoje, mas toda essa parte de cloud facilitou muito a vida para mitigar esse problema. 

Aqui ele chega a citar que o cliente tem que ficar fazendo upgrade da máquina :). Como disse, é meio que verdade ainda... Só mudou quem fica fazendo upgrade da máquina.

### 4.3 Decreasing reliability

Aqui é citado que com o tempo seu software vai ficando cada vez menos confiável. É trazida uma observação, sem referenciar a fonte, que na indústria para cada erro corrigido, na média, mais de um erro novo é introduzido. 

Ele cita que comentaram com ele uma vez sobre um sistema que tinha mais de 2000 bugs conhecidos e que geravam erros, mas que ficavam lá de boa. 

## 5. Reducing the costs of software aging

Agora ele entra nas formas que acredita serem as melhores para promover um envelhecimento de software mais tranquilo. 

Basicamente ele fala que não podemos olhar apenas para o primeiro release. Normalmente o esforço para colocar o primeiro release no ar é muito menor do que aquele necessário para lançar novas versões do projeto. 

É dito que para tratar sobre o envelhecimento de software é necessário muito mais do que um bom management das coisas. É preciso uma engenharia sólida. 

## 6. Preventive medicine

Para tentar reduzir os custos de envelhecimento, ele entra com o tópico que chama de medicina preventiva. O que faz bastante sentido. 

### 6.1 Design for success

Aqui ele traz o tópico que chama de Design for Change. A ideia é que o design do seu acomode consiga acomodar novas mudanças no seu software. Como você não sabe direito o que vai mudar, ele cita que é necessário uma investigação mais séria para tentar entender quais partes tem mais chance. 

Também traz que na opinião dele isso já tem solução faz tempo, começou sendo chamado de information hiding, depois virou data hiding e ultimamente tem sido chamado de orientação a objetos. 

Cita que as pessoas até conhecem as técnicas, mas falham recorrentemente em aplicá-las de maneira que facilite a mudança do software(não acho que as técnicas atuais resolvam, mas concordo que as pessoas conhecem. Aqui também tem a oportunidade de falar que conhecer é diferente de conseguir aplicar). 

E aí ele parte para uma lista de coisas que considera que acontecem para que a indústria não aplique tal princípio. 

1. Fala que os livros que tratam do assunto são muito superficiais. Dizem que devemos sim esconder as partes que mudam, mas não entram em detalhes de como fazer isso. Diz que o princípio é simples, mas que você precisa colocar muito esforço para identificar os pontos que realmente precisam preparados para mudança.(acho que o ddd entra neste detalhe que ele cita)
2.Cita que as pessoas que programam estão tão impacientes para o primeiro release(diria próximo) e para cumprir o deadline que não se importam com os próximos. A mesma coisa vale para a gerência. 
3. Cita que os programadores(as) pensam no design como uma sequência de passos dentro do código e não necessariamente código que esconde algo que pode mudar no futuro. (que fique claro, aqui ele não está falando apenas de tecnologias, por mais que ele cite entrada de dados anteriormente)
4. Diz que designers tendem a imitar o que já viram e as pessoas, em geral, não veem bons designs. 
5. Programadores(as) tendem a confunir princípios de design com a linguagem de programação. Por exemplo, acham que só podem aplicar as ideias da orientação a objetos numa linguagem orientada a objetos. 
6. As pessoas que estão desenvolvendo não são educadas para tal profissão. Aqui ele fala de uma época onde o engenheiro químico, quando precisava de um software, ele mesmo escrevia.Tal pessoa nao tinha se preparado para fazer algo manutenível. Dado que precisava ser, é claro. 
7. Pesquisadores tendem a escrever papers só para outros pesquisadores e ignoram a indústria(acho que faz sentido, mas hoje você pega muito paper que analisa e testa coisas na indústria). 

### 6.2 Keeping records - documentation

Aqui ele cita o completo descaso com todo tipo de documentação que poderia existir dentro de um software. Muitas vezes nada é feito e, quando é feito não é considerada cidadã de primeiro nível. Nunca é atualizada e no final acaba sendo deixada de lado... (aqui é verdade total. Nenhum tipo de texto escrito pelos times de desenvolvimento da indústria é feito com carinho... Backlog de atividades, documentação de alto nível do software etc)

### 6.3 Second opinions - reviews

Aqui ele cita que toda indústria que se preze tem um processo de review muito duro no processo de construção de seus produtos e que o mesmo deveria acontecer na indústria de software. Cita alguns pontos que podem levar a essa falta de review e vou deixar aqui dois que me chamaram atenção:

1. Muitas vezes o software é implementado sob pressão o que leva a pessoa a achar que o trabalho dela não precisa de review.
2. Muitos(as) programadores(as) acham que seu trabalho é uma arte e ficam ressentidos(as)  de ter sua entrega criticada. 

Fecha dizendo que o processo de review é essencial para a construção de um design que acomode mudanças no futuro. 

### 6.4 Why software aging is inevitable

Aqui ele fecha dizendo que o envelhecimento é inevitável, mesmo que você siga tudo que ele falou. No final a ideia é retardar a velhice do software e também facilitar para que ela aconteça de maneira menos corrosiva. 

## 7. Software geriatrics

Agora ele entra com o tratamento para o envelhecimento do software. Assim como nós, pode ser interessante que o software seja acompanhado por um geriatra. 

### 7.1 Stopping the deterioration

Aqui ele diz que se você ainda não aplica o que foi citado anteriormente, chegou a hora. Ele cita que você reimplementar módulos que estão causando problemas e tudo mais(numa arquitetura distribuída, isso, supostamente, deveria ser mais fácil. Já que o acoplamento entre o mundo externo e seu módulo está bem definido através de um contrato ali...). No final ele comenta que cortar o mal pela raiz é sempre mais interessante, ficar contendo é mais doloroso. 

### 7.2 Retroactive documentation

Aqui ele cita que um trabalho de atualizar ou criar uma documentação muito boa em cima de um software já com muitos efeitos de velhice pode ser bem interessante. Inclusive cita o fato que para uma documentação desse nível ser produzida, você provavelmente vai olhar para o software de maneira sistemática e que pode achar até bugs no meio do caminho. (eu gosto bastante disso aqui... Em vez de começar consertando a estrada, você começa facilitando como que alguém navega ali. Sem contar que você consegue gerar valor bem mais rápido e fazer entregas mais constantes.)

### 7.3 Retroactive incremental modularisation

Aqui é algo parecido com o 7.1... A ideia é identificar o conjunto de coisas que mais podem mudar, escondê-las e ficar mais preparado para o futuro. Cita que isso é muito mais fácil de falar do que de fazer e que talvez até um consultor possa ajudar. 

## 8 Planning ahead

Aqui ele está partindo para o fechamento do artigo e quer voltar no assunto que quanto mais cedo você se prevenir, melhor. 

### 8.1 A new “Life Style”

É falado que é chegada a hora de parar com o lance que o importante é rodar. Precisamos reconhecer que o software vai continuar lá. Cita que isso pode ser imposto colocando padrões fortes de desenvolvimento. 

### 8.2 Planning for change

Faça um planejamento sério sobre as possibilidades de mudança no software. Pense na documentação, código e tudo mais que está conectado com as futuras mudanças. Inclusive cita que deveria ter gente só olhando para isso. (de novo acho que o ddd fala muito sobre isso)

### 8.3 If it’s not documented, it’s not done

Aqui ele deixa claro que documentação deveria ser tratada como cidadã de primeiro nível. No artigo é descrito que nenhum produto que se preze é entregue sem documentação. (é verdade)

### 8.4 Retirement savings plans

Aqui é bem interessante porque ele fala que as empresas deveriam ter um plano de aposentadoria para o software que está sendo escrito agora(acho que faz muito sentido). 

## 10. Conclusions for our profession

1. We cannot assume that the old stuff is known and didn’t work. If it didn’t work, we have to find out why. Often it is because it wasn’t tried.
2. We cannot assume that the old stuff will work. Sometimes widely held beliefs are wrong.
3. We cannot ignore the splinter software engineering groups. Together they outnumber the people who will read our papers or come to our
conferences.
4. Even if the principle is right, without real models, the technology
won’t transfer. Practitioners imitate what they see in other products.
5. We need to make the phrase “software engineer” mean something. Until we have professional standards, reasonably standardised educational
requirements, and a professional identity, we have no right to use the phrase, “Software Engineering”.


## Opinião de Alberto

* Tirando algumas referências a poder computacional, compilação etc, parece que o artigo foi escrito hoje de manhã. 
* Na verdade eu acho que está ficando cada vez pior. Ainda hoje a maior preocupação, pela minha observação, está nas estratégias de release. 
* Falando de uma época diferente, o artigo questiona que nossa profissão não pode ser chamada ainda de engenharia e também questiona o preparo das pessoas. Eu concordo com as duas coisas, mas não vejo como uma coisa terrível, já explico. 
* Não dá para negar que a barreira de entrada no mundo de desenvolvimento ficou bem facilitada. Gosto de lembrar que você consegue rodar um programa de computador no seu navegador... Consigo fazer minha mãe, que não saca de desenvolvimento neste momento do vídeo, fazer um alert em 1 minuto. Isso é demais. Por que alguém vai reclamar que a barreira de entrada é baixa?
* Por outro lado, a consequência da facilidade de entrada é a possível falta de preparo para exercer a profissão. Infelizmente, a história mostra repetidamente que não é porque algo é fácil de começar a fazer que é fácil para você ficar realmente bom/boa. Não é porque eu pego duas/três bolinhas de malabares e consigo brincar, que eu vou para o cirque du soleil. 
* A excelência vem da oportunidade de colocar muita energia em estudo e treino. Pelo menos, até hoje, não entendo que isso mudou. 
* Então no fim você a dicotomia... A área tem muito espaço para trabalho? Tem. A área é composta por profissionais que muitas vezes ainda não tiveram a oportunidade necessária para ficarem melhores preparados? Também é verdade. Isso é ruim? Depende do ponto de vista. Para a gente não é, é ótimo. Trabalho é bom. Para o médio e longo prazo do produto sendo desenvolvido é bom? Infelizmente não. 
* Sobre o desenvolvimento em si. Na opinião do artigo o conceito de information hiding, que já existe há tempos, seria suficiente. Eu não enxergo assim e já falo sobre isso. 
* Concordo demais que o total descaso com a documentação dificulta demais. É para isso que serve orelha de livro, abstracts de artigos e qualquer outro tipo de documentação. Você quer ter uma visão geral do está rolando. 
* E daí que tem que ficar atualizando o tempo inteiro? Você não fica testando o tempo inteiro? E daí que pode ficar desatualizada? Precisa existir uma estratégia de atualização da documentação. A documentação deveria ficar versionada lindamente, com suas releases e tudo mais. 
* Sobre falar que o conceito de information hiding é suficiente, eu não vejo assim. Quando se fala de código, eu concordo com a linha de pesquisa que entende que a maneira que desenvolvemos software não funciona para criar algo que realmente facilite um envelhecimento saudável. Grandes livros como Design Patterns, Domain Driven Design, Clean Code, Extreme Programming, Refactoring e por aí vai trazem práticas muito boas, mas nenhuma sistematização. E, para mim, se não tem sistematização não vai ser sustentável. 
* Bom, no fim eu poderia usar parte desse artigo para descrever o propósito da Jornada Dev Eficiente. Eu realmente acredito que vale bastante a pena colocar esforço em facilitar a escrita de código que atenda a velocidade que é necessária dentro do mercado de hoje em dia, mas que faça com a qualidade necessária para que ele seja sustentável por muitos releases. Atualmente eu faço isso através de muita pesquisa aplicada na área de qualidade dentro da engenharia de software e também tentando facilitar o preparo da galera que se conecta com o valor da Jornada. 
