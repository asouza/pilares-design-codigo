* contexto importa para a análise da carga cognitiva
* descreve os componentes mais comuns numa aplicação web, que geralmente não tem operação de uso intenso de memória. 
* supõe o problema... tem muita literatura que te faz olhar para camadas e escreve textos sobre quando criar coisas(controllers, service, vo, model, repository), mas eu sinto que tudo parece mais uma história de drama do que computação. Um código, por mais que represente uma necessidade de negócio, é um troço lógico, então show me the logic. Minha mente tem problemas em entender quando duas pessoas debatem o papel de um domain service. Ou elas não leram o capítulo do livro ou o capítulo realmente não é tão claro. Ex:

When a significant process or transformation in the domain is not a natural responsibility of an ENTITY or VALUE OBJECT, add an operation to the model as standalone interface declared as a SERVICE. Define the interface in terms of the language of the model and make sure the operation name is part of the UBIQUITOUS LANGUAGE. Make the SERVICE stateless.

* que um código não é 100% coerente, você não precisa ser 100% coeso o tempo todo
* Essa aqui, para mim, é a arquitetura do mundo real. Da plebe, do proletariado, de quem faz aplicação web que recebe um request, trabalha sobre ele e grava ou recupera dados do banco. 

DDD do proletariado

## O DDD está presente no nosso dia a dia

Podemos dizer que o livro mais influente entre as práticas de software dos dias atuais é o DDD. Você tendo lidou ou não :). As sugestões do livro são amplamente disseminadas e é fácil perceber no código de muita equipe mundo a fora tais ideias as ideias sendo utilizadas. Inclusive existem frameworks que mencionam o livro regularmente como inspiração para algumas de suas evoluções, como o Spring. 

Olhando para as sugestões de abstrações presentes nos livros, temos algumas bem comuns. Vou começar pela família dos services;

* Application Service; em geral são representados pelos controllers nas aplicações web;
* Domain Service; em geral são classes dos sistemas que tem o sufixo Service,UseCase etc.
* Infrastructure Service; em geral são representadas por aquelas classes que encapsulam comunicações com servidores de email, chamadas http, mensagens para filas/tópicos. 

Agora vem a sequência de abstrações que são utilizadas pelos tipos de services listados.

* Entity; aquelas classes que em geral mantêm o estado do sistema e as operações sobre este estado. 
* Value Object; Aqui temos alguns sabores. 
   * Classes que nascem da combinação eventual de estados do sistema para serem persistidas também ou apenas para fazer parte de um fluxo em memória;
   * Classes criadas para adicionar semântica à alguma representação que o tipo primitivo da linguagem não suporta, alguns casos classicos: SenhaCriptografada, CPF, Email etc.
* Aggregate root; Esse nome gera bastante confusão. Podemos dizer que um Aggregate root é uma entidade composta por outras entidades ou objetos de valor e que controla o acesso a estas composições. 
* Repository; abstrações que isolam o registro, atualização, deleção e recuperação dos objetos em algum tipo de storage, normalmente um banco de dados. Geralmente são implementadas através de alguma tecnologia de acesso a dados específica. 

## A distorção

O grande desafio enquanto lemos o livro é entender e ser capaz de aplicar os conceitos apresentados de forma a realmente verificar ganhos no design da aplicação. O que queremos é ter uma aplicação que faça o necessário com um design suficiente e flexível. 

Infelizmente o que encontramos fortemente nos sistemas escritos são interpretações distorcidas das ideias apresentadas por Eric Evans no decorrer do livro. Abaixo deixo algumas. 

### Primeira distorção: controllers só acessam services de domínio

Você já deve ter ouvido alguém falar ou até reclamar com você que um Controller nunca pode acessar um repositório diretamente. 

Em primeiro lugar, os controllers que implementamos são chamados de Application Services pelo livro. Você encontra na página 102, no capítulo onde ele descreve os tipos de Service. Para completar, ele cita que Application Services podem acessar diretamente serviços de infraestrutura. **Repare, não sou eu que estou dizendo, está escrito.** 

### Segunda distorção: services de domínio concentram toda inteligência

Isso inclusive já foi tema de post de Martin Fowler, os tais dos modelos anêmicos. O assunto é velho, mas o problema persiste. Ficamos com controllers de uma linha nas aplicações, entidades e objetos de valor só com atributos e toda operação sobre esses estados é feita dentro de Domain Services.

É escrito claramente no capítulo sobre camadas, na última seção, que é na CAMADA de DOMÍNIO que reside o modelo. Esse modelo é expresso por Entities, Value Objects, Domain Services, Aggregate Roots e Repositories. Inclusive o livro cita que a única camada que precisa existir é a de domínio(model). 

### Terceira distorção: camada superior só acessa a camada inferior subsequente

Ainda no capítulo sobre camadas, na seção chamada "Relacionado as camadas" ele escreve a seguinte frase:

"As camadas superiores podem utilizar ou manipular elementos das camadas inferiroes de forma bastante simples, chamando suas interfaces públicas, fazendo referências a eles(pelo menos temporariamente) e geralmente usando meios convencionais de interação". 

Não está escrito em nenhum santo lugar que Controller não chama Repository. Ele não escreveria isso, porque não faz sentido o mesmo programa que pode ser escrito com um arquivo e uma linha seja escrito com dois arquivos e duas linhas. Voltarei a essa parte de novo no artigo. 

## Por que distorcermos?

Aqui tem a hipótese mais óbvia, a maioria das pessoas nas equipes que tentam seguir práticas sugeridas pelo Domain Driven Design não leram o livro. Elas entendem as sugestões do livro pelos olhos de outras pessoas, que podem ter lido ou não também. É quase um efeito cascata de interpretação. 

Uma outra hipótese também bem aceitável é que por mais que o livro tente dar exemplos e explicar muito bem cada uma das sugestões, é complicado pegar aquele conceito e transferir para o seu dia a dia. Qualquer transferência de aprendizagem exige real experimentação, com foco no aprendizado e olhar crítico, o que claramente não é o cenário encontrado pela maioria das pessoas nos seus trabalhos.

A terceira hipótese é que os livros famosos de arquitetura de software, DDD incluso, são muito genéricos. E a maioria dos sistemas que trabalhamos não são genéricos, são específicos. O que precisamos é: **como aplicamos as práticas sugeridas pelo DDD nas aplicações web desenvolvidas hoje em dia?** 

## O cenário de hoje em dia

As práticas de design sugeridas pelo DDD são agnósticas em relação a frameworks, linguagem e quaisquer outras tecnologias envolvidas. Por outro lado a maioria das aplicações são desenvolvidas em cima justamente de uma linguagem e com um conjunto de tecnologias estabelecidos que formam a fundação de qualquer sistema a ser desenvolvido naquele lugar. 

Não podemos e seria até ingênuo ignorar os frameworks que facilitam nosso trabalho. Pelo menos para a maioria esmagadora das aplicações que são desenvolvidas. Precisamos é tirar proveito de tais tecnologias em cima de alguma sugestão de padrão, neste caso estamos falando do DDD. 

## DDD do proletariado

O DDD do proletariado é a minha sugestão de design código baseada na teoria da carga cognitiva aplicada sobre o Domain Driven Design especificamente para aplicações web desenvolvidas no mundo atual. 

O que eu quero com isso? Possibilitar um padrão de desenvolvimento de aplicações web que façam o necessário, tenham design suficientemente flexível para suas necessidades e onde o esforço de entendimento da maior parte do projeto seja baixo. Basicamente é minha tentativa de resolver o mesmo problema que todo padrão tenta, complexidade do código :). 

Sinto que existem algumas diferenças sobre o que eu vou tentar mostrar versus o que já foi proposto pelos outros livro. 

* Estou focado em aplicações web;
* Penso majoritariamente em aplicações que fazem uso de linguagens orientadas a objetos;
* Levo em consideração fortemente os frameworks que facilitam nossa vida;
* Proponho a divisão de responsabilidade dentro do código pelo olhar da carga cognitiva, ou seja, vou deixar com você um jeito lógico de fazer isso;

### Estou focado em aplicações web

O quero dizer é que vamos levar em consideração o fluxo natural da maioria das requisições que chegam nas aplicações web. Lembrando que seu microservice é uma aplicação web. 

1. Dados da requisição entram por um método do controller;
2. Tais dados são convertidos para os tipos específicos;
3. Se a conversão deu certo, eles são validados seguindo as regras daquele endoint;
4. Em função de tais dados, algum objeto do model é criado/carregado;
5. Alguma lógica pode ser executada em cima do model;
6. Caso a lógica altere o estado do model, esse estado é refletido no seu banco de dados;
7. Pode ser que essa alteração de estado dispare novos fluxos no sistema;
8. Um objeto de resposta é gerado;
9. Tal resposta é serialiazada num formato específico e enviada para a aplicação cliente;

Esse fluxo todo acontece dentro de uma requisição e, quando levamos em consideração apenas UMA requisição, não podemos dizer que tal fluxo faz uso intensivo de memória. Por outro lado é bem diferente de aplicações que precisam fazer realmente processamentos em memória a partir de qualquer fonte de dados. Processar dados textuais, imagens, buscas inteligentes etc.

É muito importante que você perceba a diferença desse tipo de software para outros, inclusive que você utiliza como tecnologia base. É muito diferente implementar um Tomcat versus uma aplicação web tradicional. O mesmo vale para seu framework, biblioteca etc. 

### Penso majoritariamente em aplicações que fazem uso de linguagens orientadas a objetos

Desde cedo na minha carreira eu abracei as linguagens orientadas a objetos. Posso dizer que me considero um especialista em tal paradigma e sinto que tenho boas coisas para compartilhar e propor. Considero que as linguagens que suportam O.O, escolha a que quiser, fornecem os meios necessários para construirmos aplicações web suficientes para as necessidades do mercado.

Por outro lado não me aprofundei em outros paradigmas, um exemplo clássico é o funcional. Acho que faço bom uso de funções como ciadadãs de primeiro nível em códigos com Javascript, Kotlin, Scala(quase) etc. Também tiro proveito dessas construções em linguagens como Java e C#. Sou também fã de imutabilidade e realmente acho que ela facilita o controle do estado da aplicação e o debug em cima de algum fluxo. 

Daí a dizer que posso sugerir algo para quem usa clojure ou qualquer outra linguagem que faça uso mais forte do paradigma funcional seria muita ousadia :).

### Levo em consideração fortemente os frameworks que facilitam nossa vida

Olhando para a sequência de passos, podemos notar que os frameworks atuam em algumas frentes facilitando nosso vida. 

1. Encapsulam toda a lógica de receber a request, converter e validar boa parte dos dados;
2. Fornecem mencanismos para você criar validações customizadas que adicionem proteção as bordas;
3. Fornecem os meios para refletir o estado do model no seu banco de dados;
4. Podem fornecer(geralmente fornecem) os meios para disparar os eventos em função da alteração de estado;
5. Serializam a resposta no formato pedido pelo cliente;

Eu não vejo motivos para criarmos aplicações que sejam desacopladas dos frameworks que formam a fundação de um código. Alguns exemplos práticos de combinação entre linguagem e framework.

* Java + Spring;
* Ruby + Rails;
* Python + Django/Flesk;
* Php + Laravel;
* Javasript + NestJs;
* Elixir + Phoenix;

Uma tentativa de construir código que te deixe fortemente desacoplado dos frameworks citados vai elevar o número de linhas de código do seu projeto, trazer abstrações mais complicadas e tudo isso para que? Para alimentar a ilusão que uma troca de framework vai ser suave? 

Mesmo que uma eventual troca seja realmente mais suave, vale o esforço de passar meses/anos em cima de um código que parece um circo multi colorido só para não olhar para um import do Spring dentro da sua camada de Model? Na minha opinião não vale.

A minha sugestão vai abraçar o framework escolhido e deixar claro que está tudo bem seu código ficar acoplado a ele. O que você precisa decidir é sobre com quem você quer ficar amarrado. Uma vez que decidimos, vamos tirar o máximo de proveito. 

### Proponho a divisão de responsabilidade dentro do código pelo olhar da carga cognitiva

Você já deve ter parado e pensado: de quem é a responsabilidade desse código? Em qual classe eu deixo isso aqui? Crio uma nova? 

Tal pensamento é interessante e ajuda na construção do sistema, mas não é suficiente. Quando pensamos em dividir responsabilidade, estamos tentando, na maioria das vezes, dar um jeito de fazer um código que seja mais fácil de entender e, consequetemente, evoluir. E aí é que entra ter o conhecimento básico sobre alguma frente de estudo sobre os mecanismos que nos levam a entender algo.

A teoria da carga cognitiva diz que temos um espaço super limitado de memória enquanto estamos tentando entender algo novo. Já falei sobre isso no texto anterior onde relaciono design de código e esta teoria(link). Mas o básico é: **se você for estiver voando, na plenitudade da sua forma, sua memória de trabalho aceita entre 5 e 9 coisas diferentes ao mesmo tempo na sua cabeça.**

E você, mesmo de forma inconsciente, já sabe um pouco disso. Você olha para um arquivo de código com muitas linhas e já pensa: deus, como que eu vou entender isso tudo? Sua reação é tentar ir refatorando para chegar em algo que seja minimamente aceitável. E o que é aceitável? Como saber? 

Minha sugestão é que nenhum arquivo tenha mais de 9 pontos de carga cognitiva. E aí temos um jeito lógico de ir quebrando nossa aplicação em arquivos e de vez em quando sistemas menores, não é mesmo microservices? Imagine que você pode colocar um limite de pontos de carga cognitiva por microservice :). Quem sabe esse é um jeito de você descobrir seus bounded contexts? 

**É necessário ter um motivo lógico para eu aumentar o número de arquivos do meu sistema**. Criamos arquivos com classes para distribuir a carga cognitiva do sistema e não simplesmente porque achamos mais elegante/bonito etc. 

## Agora show me the code!

É claro que vou show you the code. Específicamente neste post eu vou trabalhar a distribuição da carga cognitiva pelo olhar de uma aplicação web escrita com usando Spring, mas as práticas podem ser seguidas para qualquer combinação de linguagem O.O e um framework que realmente te abstraia o trabalho duro. 

### Controllers 100% coesos

A primeira coisa é entender o significado da palavra coesão. Existem algumas métricas disponíveis para avaliar coesão, a que eu utilizo como base aqui é uma chamada Tight and Loose Class Cohesion(https://www.aivosto.com/project/help/pm-oo-cohesion.html#TCC_LCC). Não para aplicá-la exatamente igual o sugerido, já que um controller não é bem uma classe no sentido puro da palavra.

Um controller é apenas um recipiente de rotas que representam os endpoints expostos nas aplicações. Ele não mantém estado da apliacação e seus atributos deveriam ser, na verdade, variáveis locais de seus métodos. Uma rota é uma função, que recebe uma entrada e gera uma saída. Pela definição do artigo Tipos Absratos de Dados(link) eles possuem as chamadas funções abstratas. O problema é que linguagens como Java e C# nos obrigam a escrever classes para que seja possível declarar funções(métodos estáticos). E os frameworks se apoiam nisso. Em vez de declararmos variáveis locais, declaramos atributos e recebemos seus valores injetados pelo framework.

Dado esse cenário, a minha sugestão é que todo método de um controller use todos os atributos declarados. Além disso a carga cognitiva dele não pode passar de 7 pontos(acesse aqui para entender o que aumenta a carga cognitiva). O resultado dessa combinação é que você vai ter controllers enxutos e que não ultrapassam o limite da memória de trabalho. 

O tradeoff é que você vai ter mais classes que representam controllers do que você tem atualmente. Na minha opinião isso não é um problema. Inclusive controllers inflados foi justamente um problema encontrado por Aniche(link) nuam pesquisa que ele fez em cima de várias aplicações web encontradas no github. 

Por fim, um controller escrito usando um framework interessante, já abstrai toda infra http para você. E agora você tem um cenário onde ele é enxuto e super específico. A soma disso é que na minha opinião você pode fundir em vários momentos a ideia de Application Service com Domain Service e chegar em um código parecido com esse:

```
@RestController
public class RetornoPagamentoPagSeguroController {

    @Autowired
    private CompraRepository compraRepository;

    @Autowired
    private PagamentoRepository pagamentoRepository;

    @Autowired
    private ApplicationEventPublisher applicationEventPublisher;

    @InitBinder
    public void initBinder(WebDataBinder dataBinder) {
        dataBinder.addValidators(new VerificaSeCompraJaEstaConcluidaValidator(compraRepository));
    }

    @PostMapping(value = {"/api/retorno-pagamento/{compraId}/pagseguro"})
    @Transactional
    public void processaRetorno(@PathVariable("compraId") Long compraId, @Valid RetornoPagamentoPagSeguroRequest retornoPagamentoPagSeguroRequest, UriComponentsBuilder uriComponentsBuilder) {

        Compra compra = FindById.executa(compraId, compraRepository);

        Pagamento pagamento = retornoPagamentoPagSeguroRequest.criaPagamento(compra);
        pagamentoRepository.save(pagamento);

        compra.registra(pagamento);
        compraRepository.save(compra);

        applicationEventPublisher.publishEvent(new NovoPagamentoEvent(this, pagamento, uriComponentsBuilder));
    }
}

```

Dado a minha sugestão do que o único acoplamento que devemos levar em consideração é o feito com classes criadas no próprio sistema, temos a seguinte conta cognitiva aqui:

* +1 CompraRepository;
* +1 PagamentoRepository;
* +1 VerificaSeCompraJaEstaConcluidaValidator;
+ +1 RetornoPagamentoPagSeguroRequest;
* +1 Compra;
* +1 FindById;
* +1 Pagamento;
* +1 NovoPagamentoEvent;

Temos 8 pontos de carga cognitiva. Ultrapassa em 1 ponto a minha sugerida pelo controller, mas continua dentro do limite aceitável. 

Eu sugiro a carga do controller ficar em 7 justamente porque um controller está na borda mais externa da aplicação e deve ser mais fácil de entender. Claro que você pode ser mais restritivo e baixar essa pontuação, se achar interessante. Um detalhe legal é que só passamos de 7 pontos porque o código decidiu usar uma abstração chamada ```FindBy``` para isolar o tratamento do retorno Opcional da busca pelo id da ```Compra```.

Você ganhou controllers coesos, com carga cognitiva baixa e que tem uma régua clara para review de código. Inclusive que pode ser automatizada. Se a carga cognitiva passar de 7, você tenta distribuir :). Vou dar um exemplo para esse de cima:

```
@RestController
public class RetornoPagamentoPagSeguroController {

    @PersistenceContext
    private EntityManager manager;

    @Autowired
    private ApplicationEventPublisher applicationEventPublisher;

    @InitBinder
    public void initBinder(WebDataBinder dataBinder) {
        dataBinder.addValidators(new VerificaSeCompraJaEstaConcluidaValidator(compraRepository));
    }

    @PostMapping(value = {"/api/retorno-pagamento/{compraId}/pagseguro"})
    @Transactional
    public void processaRetorno(@PathVariable("compraId") Long compraId, @Valid RetornoPagamentoPagSeguroRequest retornoPagamentoPagSeguroRequest, UriComponentsBuilder uriComponentsBuilder) {

        Compra compra = manager.find(compraId,Compra.class);

        Pagamento pagamento = retornoPagamentoPagSeguroRequest.criaPagamento(compra);
        manager.persist(pagamento);

        compra.registra(pagamento);

        applicationEventPublisher.publishEvent(new NovoPagamentoEvent(this, pagamento, uriComponentsBuilder));
    }
}

```

Usei o ```EntityManager``` da JPA direto e ainda tirei uma linha de ```save``` que não precisava. Agora, se fizermos a mesma conta:

* +1 VerificaSeCompraJaEstaConcluidaValidator;
+ +1 RetornoPagamentoPagSeguroRequest;
* +1 Compra;
* +1 Pagamento;
* +1 NovoPagamentoEvent;

Saímos de 8 para 5 sem criar novos arquivos, códigos etc. Usei o ```EntityManager``` direto no nosso mais novo ControllerService, usei, e daí?

E se eu puder processar compras através de outras entradas do sistema? Adapte o código :). Você não precisa ter medo de mudança, basta que ela seja mais fácil de ser realizada. 







## Relembrando o real motivo para você criar N arquivos em vez de UM para resolver um problema

## O DDD do proletariado

aqui eu falo do fluxo padrao de uma requisicao e falo que agora de juntar os padrões estabelecidos por um livro super famoso com algo mega prático e que pode ser usado já por você. 



## As sugestões arquiteturais de hoje são sujeitas a interpretação

Antes de começar eu vou deixar aqui uma lista de possíveis influências que você usa ou que talvez já tenha ouvido falar:

* Domain Driven Design
* Onion Architecture
* Hexagonal Architecture
* The Clean Architecture

Todas elas são interessantes, afinal de contas, pelo menos na minha opinião, todo conhecimento pode expandir seu horizonte de mundo. 

E será que falta alguma coisa nelas? Para mim sim. Vou listar alguns itens que eu gostaria de ver:

* Um jeito lógico de definir que tipo de código vai em cada camada. Como eu sei que meu controller está perto de algo ok? Como eu sei que meu service está realmente interessante?
* Especificidade. Contexto importa e eu não quero algo genérico, preciso de algo específico. 

Por exemplo, olha essa frase extraída do DDD:

> When a significant process or transformation in the domain is not a natural responsibility of an > ENTITY or VALUE OBJECT, add an operation to the model as standalone interface declared as a > SERVICE. Define the interface in terms of the language of the model and make sure the operation > name is part of the UBIQUITOUS LANGUAGE. Make the SERVICE stateless.

Em algum momento eu posso passar um formulário de investigação para pegar respostas dos(as) devs sobre esse trecho. Enquanto não faço isso, minha observação diz que eu teria muitas respostas diferentes, e essa é uma situação que eu não posso aceitar. Eu não aguento quando duas pessoas estão discutindo sobre o que é um Domain Service, dado que o suposto conceito está definido em um livro. Ou elas não leram o livro, ou o trecho não foi tão bem escrito.

Existe uma terceira opção para a visão diferente sobre o mesmo conceito, a tal da interpretação. 

## A interpretação arquitetural é danosa

Quanto maior a equipe, mais complicado é você manter uma linha coerente de raciocínio dentro de um mesmo sistema. São muitas cabeças diferentes, com interpretações diferentes sobre o mesmo conceito, escrevendo códigos que refletem a visão delas sobre como uma aplicação web deveria ser programada.  

Tem um cenário talvez ainda pior. Duas pessoas interpretaram o mesmo conceito de forma igual, mas na hora de implementar fazem diferentes. 

## Os componentes e fluxos básicos da maioria das aplicações web

A grande maioria das aplicações web baseadas em linguagens orientadas a objetos, sejam elas pequenas ou grandes, tem alguns componentes muito claros, são eles:

* Controllers que recebem as requisições com seus parâmetros e cabeçalhos;
* Classes que representam os dados enviados na request. De vez em quando são chamados de YYYDTO, YYYViewModel, YYYForms, YYYRequests ou outra nomenclatura que você queira colocar;
* Classes que representam as respostas dos controllers. De vez em quando são chamados de YYYDTO, YYYResponse, YYYOutput ou outra nomenclatura que você queira colocar;
* Classes que representam os tais dos fluxos de negócios. Aqui entram os Domain Services do DDD;
* Classes que representam o tal do model do sistema e que supostamente deveriam guardar e operar sobre o estado da aplicação em si;
* Outras classes que executam funções ortogonais a todo o sistema, como enviar email, mandar mensagem para filas, tópicos, fazer requisições http etc. 

### Fluxo padrão uma vez que uma requisição chegou numa aplicação web

Vou deixar em lista de novo, porque deve ser mais fácil de visualizar. Dado que eu não sei desenhar. 

* Dados da requisição entram por um método do controller;
* Os dados são transformados para algo do seu model;
* Pode ser que algum estado seja alterado no seu model em memória;
* Pode ser que a alteração de estado seja refletida no seu banco de dados;
* Pode ser que essa alteração de estado dispare novos fluxos no sistema;
* Uma resposta é gerada e enviada para quem fez a requisição;

O desafio agora é implementar esse fluxo, em sistemas de variados tamanhos, mantendo a carga intrínseca de cada pedaço do sistema dentro do recomendado que é de no máximo 9 pontos. Sendo que 9 pontos já vai ser complicado.

* Como que eu faço um controller que presta?
* Como eu transformo esses dados para o meu model?
* Como eu reflito o estado do model no banco de dados?
* Como eu disparo esses novos fluxos no sistema?
* Como eu gero minha resposta?

### A resposta

Existem algumas premissas em cima da minha resposta:

* O sistema é construído em cima de uma linguagem orientada a objetos;
* Vou assumir que você utiliza algum framework que abstraia o trabalho duro de uma aplicação web. Spring MVC, Nest.js, Djanjo, Rails, Phoenix, .NET Core ou sei lá mais o que;
* Vou utilizar o design orientado a carga cognitiva para sugerir implementações;
* A aplicação web é um misto entre programação orientada a objetos e procedural. Está tudo bem;

Abaixo segue a lista de itens que considero que contam pontos de carga intrínseca de um código:

* Classes necessárias para habilitar a execução de um determinado ponto do nosso código. Aqui eu só levo em consideração classes que foram criadas pelo próprio sistema. Classes do runtime da linguagem, frameworks etc devem ser conhecidas a priori. Não sabe? Precisa estudar. 
* Número de branches no código. Ifs, loops, ifs e/ou loops aninhados. 
* Número de invocações de métodos em classes específicas do sistema para realizar determinado fluxo.

Cada item acima encontrado em uma classe aumenta um ponto na carga intrínseca. 




#### Como devem ser os controllers?

Os controllers representam o ponto de entrada dos dados. Ele deve ser um lugar onde qualquer possa bater o olho e entender o que está rolando para a execução de um determinado fluxo relativo a uma requisição.

A primeira coisa é que eles devem ter TCC=1(zero também seria válido. uma rota que só usa os parâmetros) seguindo a métrica Tight and Loose Class Cohesion(link), ou seja todos os métodos devem usar todos os atributos(caso existam). O que isso significa? Que numa liguagem orientada a objetos, cada uma das rotas definidas em um controller devem usar todos os atributos. O resultado disso é que você vai acabar com muitos controllers com um método só e está tudo bem. Uma rota é uma procedure, ou uma função abstrata seguindo a ideia do artigo Tipos Abstratos de Dados. Ela poderia muito bem ser escrita sem a necessidade de uma classe :). 

// exemplo de código aqui

A segunda coisa é que você deve trabalhar duro para manter a contagem de pontos da carga intrínseca em no máximo 7. Passou de 7, chegou a hora de dividir. Isso significa que provavelmente você deve criar uma nova classe para realiar aquele fluxo. 

//exemplo aqui

A terceira coisa e que talvez você fique mais assustado. Em um cenário de controllers com TCC=1, provavelmente você não precisa mais daquele service que você criaria. Um controller desse, dado o contexto de utilização de frameworks descrito acima, já tende a representar o caso de uso em si. 

//exemplo aqui 

#### E quando a carga intrínseca do controller estoura?

aqui vou falar da criação dessas outras classes que fazem fluxos... vão ser as ramificações do seu service. Elas também deve ter TCC=1 e não estourar os 7 pontos. 

#### E onde eu transformo os dados

aqui eu deixo a porta aberta para converers(adapters) ou form.toModel(dependencias)... Pouco importa pq você manter a carga cognitiva baixa. Com um converter você vai ter 2 pontos a mais e com um form.toModel 1 ponto a mais. Já que eu vou continuar recebendo o form na rota e vou ter a injeção do converter.

#### como eu reflito

fala rápido de repositórios e pronto. TCC=1 também

#### como eu altero o estado do model na memória

aqui aceita a carga intrínseca até 9 e também deixa claro que o lcc >= 0.8 (uam sugestão inicial)

#### geração de respostas 

nem sei vale entrar no mérito aqui.. seria sempre algo assim new Resposta(dominio) e a resposta navega no dominínio. Essa é uma classe que pode ser mais tosca pq ela vai ser super específica. 

ai eu vou ter outro texto falando de smells que considero que podem ser observados. 










