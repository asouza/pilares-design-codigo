## O cenário do DDD de hoje em dia

As práticas de design sugeridas pelo DDD são agnósticas em relação a frameworks, linguagem e quaisquer outras tecnologias envolvidas. Por outro lado a maioria das aplicações são desenvolvidas em cima justamente de uma linguagem e com um conjunto de tecnologias estabelecidos que formam a fundação de qualquer sistema a ser desenvolvido naquele lugar. 

Não podemos e seria até ingênuo ignorar os frameworks que facilitam nosso trabalho. Pelo menos para a maioria esmagadora das aplicações que são desenvolvidas. Precisamos é tirar proveito de tais tecnologias em cima de alguma sugestão de padrão, neste caso estamos falando do DDD. 

## DDD do proletariado

O DDD do proletariado é a minha sugestão de design código baseada na teoria da carga cognitiva aplicada sobre o Domain Driven Design especificamente para aplicações web desenvolvidas no mundo atual. 

O que eu quero com isso? Possibilitar um padrão de desenvolvimento de aplicações web que façam o necessário, tenham design suficientemente flexível para suas necessidades e onde o esforço de entendimento da maior parte do projeto seja baixo. Basicamente é minha tentativa de resolver o mesmo problema que todo padrão tenta, complexidade do código :). 

Sinto que existem algumas diferenças sobre o que eu vou tentar mostrar versus o que já foi proposto pelos outros livros. 

* Estou focado em aplicações web;
* Penso majoritariamente em aplicações que fazem uso de linguagens orientadas a objetos;
* Levo em consideração fortemente os frameworks que facilitam nossa vida;
* Proponho a divisão de responsabilidade dentro do código pelo olhar da carga cognitiva, ou seja, vou deixar com você um jeito lógico de fazer isso;

### Estou focado em aplicações web

O que quero dizer é que vamos levar em consideração o fluxo natural da maioria das requisições que chegam nas aplicações web. Lembrando que seu microservice é uma aplicação web. 

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

É muito importante que você perceba a diferença desse tipo de software para outros, inclusive em aqueles que você utiliza como tecnologia base. É muito diferente implementar um Tomcat versus uma aplicação web tradicional. O mesmo vale para seu framework, biblioteca etc. 

### Penso majoritariamente em aplicações que fazem uso de linguagens orientadas a objetos

Desde cedo na minha carreira eu abracei as linguagens orientadas a objetos. Posso dizer que me considero um especialista em tal paradigma e sinto que tenho boas coisas para compartilhar e propor. Considero que as linguagens que suportam O.O, escolha a que quiser, fornecem os meios necessários para construirmos aplicações web suficientes para as necessidades do mercado.

Por outro lado não me aprofundei em outros paradigmas, um exemplo clássico é o funcional. Acho que faço bom uso de funções como cidadãs de primeiro nível em códigos com Javascript, Kotlin, Scala(quase) etc. Também tiro proveito dessas construções em linguagens como Java e C#. Sou também fã de imutabilidade e realmente acho que ela facilita o controle do estado da aplicação e o debug em cima de algum fluxo. 

Daí a dizer que posso sugerir algo para quem usa clojure ou qualquer outra linguagem que faça uso mais forte do paradigma funcional seria muita ousadia :).

### Levo em consideração fortemente os frameworks que facilitam nossa vida

Olhando para a sequência de passos, podemos notar que os frameworks atuam em algumas frentes facilitando nosso vida. 

1. Encapsulam toda a lógica de receber a request, converter e validar boa parte dos dados;
2. Fornecem mecanismos para você criar validações customizadas que adicionem proteção as bordas;
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

Uma tentativa de construir código que te deixe fortemente desacoplado dos frameworks citados vai elevar o número de linhas de código do seu projeto, trazer abstrações mais complicadas e consequentemente aumentar a carga intrínseca do sistema como um todo, tudo isso para que? Para alimentar a ilusão que uma troca de framework vai ser suave? 

Mesmo que uma eventual troca seja realmente mais suave, vale o esforço de passar meses/anos em cima de um código que parece um circo multi colorido só para não olhar para um import do Spring ou de qualquer outro framework dentro da sua camada de Model? Na minha opinião não vale.

A minha sugestão vai abraçar o framework escolhido e deixar claro que está tudo bem seu código ficar acoplado a ele. O que você precisa é ter um olhar lógico sobre o quão preparado seu framework está para você decidir ficar amarrado com ele. Uma vez que decidimos, vamos tirar o máximo de proveito. 

Alguns pontos que você pode olhar na hora que tiver se acoplando com uma tecnologia específica em algumas ou várias partes da sua aplicação. Vamos pegar para análise um pouco do Spring. 

Você recebeu como parâmetro de um tipo específico do Spring. Este tipo tem uma interface pública facilmente mockavel? Como estamos falando de Java, você pode olhar se existe realmente uma interface para aquele tipo concreto. 
Existem implementações de testes específicas para aquelas interfaces que você está se acoplando?
Existe um suporte a realização de testes automatizados usando o contexto do framework?

Em geral, para tecnologias mais maduras, você vai ter boas respostas para as perguntas acima. 


### Proponho a divisão de responsabilidade dentro do código pelo olhar da carga cognitiva

Você já deve ter parado e pensado: de quem é a responsabilidade desse código? Em qual classe eu deixo isso aqui? Crio uma nova? 

Tal pensamento é interessante e ajuda na construção do sistema, mas não é suficiente. Quando pensamos em dividir responsabilidade, estamos tentando, na maioria das vezes, dar um jeito de fazer um código que seja mais fácil de entender e, consequentemente, evoluir. E aí é que entra ter o conhecimento básico sobre alguma frente de estudo sobre os mecanismos que nos levam a entender algo.

A teoria da carga cognitiva diz que temos um espaço super limitado de memória enquanto estamos tentando entender algo novo. Já falei sobre isso no texto anterior onde relaciono design de código e esta teoria(link). Mas o básico é: **se você estiver voando, na plenitude da sua forma, sua memória de trabalho aceita entre 5 e 9 coisas diferentes ao mesmo tempo na sua cabeça.**

E você, mesmo de forma inconsciente, já sabe um pouco disso. Você olha para um arquivo de código com muitas linhas e já pensa: deus, como que eu vou entender isso tudo? Sua reação é tentar ir refatorando para chegar em algo que seja minimamente aceitável. E o que é aceitável? Como saber? 

Minha sugestão é que nenhum arquivo tenha mais de 9 pontos de carga cognitiva. E aí temos um jeito lógico de ir quebrando nossa aplicação em arquivos e de vez em quando sistemas menores, não é mesmo microservices? Imagine que você pode colocar um limite de pontos de carga cognitiva por microservice :). Quem sabe esse é um jeito de você descobrir seus bounded contexts? 

**É necessário ter um motivo lógico para eu aumentar o número de arquivos do meu sistema**. Criamos novos arquivos para distribuir a carga cognitiva do sistema e não simplesmente porque achamos mais elegante/bonito etc. Alguns exemplos mais palpáveis:

Você divide o fluxo de uma lógica em dois arquivos já que a soma de pontos de carga intrínseca superou um certo limite. Vai depender do local da aplicação onde o código está sendo escrito.
Você cria um arquivo a mais para representar um conjunto de dados + possíveis comportamentos que ainda não existe na sua aplicação;
Você cria um arquivo novo para tirar proveito de um recurso da linguagem. Por exemplo uma nota entre 1 e 5 pode ser representada por uma Enum no Java. A criação do arquivo aumenta a carga cognitiva, porém faz você tirar proveito da compilação para garantir o range da nota;

//ainda vou criar um texto só falando de smells;

## Agora show me the code!

É claro que vou show you the code. Especificamente neste post eu vou trabalhar a distribuição da carga cognitiva pelo olhar de uma aplicação web escrita com usando o Spring, mas as práticas podem ser seguidas para qualquer combinação de linguagem O.O e um framework que realmente te abstraia o trabalho duro. 

### A carga intrínseca é contextual numa aplicação web

É importante que seja estabelecida uma régua de carga intrínseca que possa ser observada em toda a aplicação. Os projetos que analisam qualidade olham todo arquivo do mesmo jeito, entendo que esse é um jeito simplista e não condizente com o funcionamento de uma aplicação web. 

Inclusive Aniche, no artigo Tailoring Code Metric Thresholds for Different Software Architectures(http://pure.tudelft.nl/ws/files/12555765/TUD_SERG_2016_023.pdf), traz essa observação. As medidas de coesão, acoplamento entre outras são diferentes em diferentes contextos da mesma aplicação. 

Segue a minha sugestão de distribuição de carga intrínseca em função do contexto de uso de cada tipo de classe. 

Application Services(controllers) e Domain Services(services comumente usados) não devem passar de 7 pontos de carga intrínseca. Eles lidam com fluxo de informações e isso deveria ser facilmente entendido. Imagino que exista espaço inclusive para ser ainda mais restritivo
Classes que possuem estado temporário/transitório podem ter no máximo 9 pontos. Essas classes geralmente flertam com Value objects. Elas em geral tem poucos métodos que operam sobre o estado.
Classes que possuem estado persistente também podem ter no máximo 9 pontos. Essas classes geralmente são as entities. Essa é uma métrica que precisa de mais tempo para ficar mais apurada. 
Infrastructure Services podem ter a pontuação que você quiser. Elas geralmente são classes classificadas como helpers etc… Você quase não mexe e não tem problema gastar um pouco mais de tempo quando for dar manutenção. 
Repository deve ter carga de no máximo 3 pontos. A depender do framework e do método de análise que eu sugiro(link), seu repository pode até flertar com um ponto de carga intrínseca.

Todas as minhas sugestões de código vão ser baseadas nas pontuações sugeridas acima.



### Controllers 100% coesos

A primeira coisa é entender o significado da palavra coesão. Existem algumas métricas disponíveis para avaliar coesão, a que eu utilizo como base aqui é uma chamada Tight and Loose Class Cohesion(https://www.aivosto.com/project/help/pm-oo-cohesion.html#TCC_LCC). Não dá para aplicá-la exatamente igual o sugerido, já que um controller não é bem uma classe no sentido puro da palavra.

Um controller é apenas um recipiente de rotas que representam os endpoints expostos nas aplicações. Ele não mantém estado da aplicação e seus atributos deveriam ser, na verdade, variáveis locais de seus métodos. Uma rota é uma função, que recebe uma entrada e gera uma saída. Pela definição do artigo Tipos Abstratos de Dados(link) eles possuem as chamadas funções abstratas. O problema é que linguagens como Java e C# nos obrigam a escrever classes para que seja possível declarar funções(métodos estáticos). E os frameworks se apoiam nisso. Em vez de declararmos variáveis locais, declaramos atributos e recebemos seus valores injetados pelo framework.

Dado esse cenário, a minha sugestão é que todo método de um controller use todos os atributos declarados. Além disso a carga cognitiva dele não deveria passar de 7 pontos(acesse aqui para entender o que aumenta a carga cognitiva). O resultado dessa combinação é que você vai ter controllers enxutos e que não ultrapassam o limite da memória de trabalho(link aqui). 

O tradeoff é que você vai ter mais classes que representam controllers do que você tem atualmente. Na minha opinião isso não é um problema. Inclusive controllers inflados foi justamente um problema encontrado por Aniche(link) numa pesquisa que ele fez em cima de várias aplicações web encontradas no github. É importante lembrar que mais arquivos só são um problema se eles não estiverem distribuindo a carga cognitiva pelo sistema.

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

Dado a minha sugestão de que o único acoplamento que devemos levar em consideração é o feito com classes criadas no próprio sistema, temos a seguinte conta de carga intrínseca aqui:

* +1 CompraRepository;
* +1 PagamentoRepository;
* +1 VerificaSeCompraJaEstaConcluidaValidator;
+ +1 RetornoPagamentoPagSeguroRequest;
* +1 Compra;
* +1 FindById;
* +1 Pagamento;
* +1 NovoPagamentoEvent;

Temos 8 pontos de carga intrínseca. Ultrapassa em 1 ponto a minha sugerida pelo controller, mas continua dentro do limite aceitável. E perceba que você já fez tudo necessário para a execução da lógica de negócio também. 

Eu sugiro a carga do controller ficar em 7 justamente porque um controller está na borda mais externa da aplicação e, por ser um local onde as pessoas começam a olhar um código, deveria ser mais fácil de entender. Claro que você pode ser mais restritivo e baixar essa pontuação se achar interessante, experimente. Um detalhe legal é que só passamos de 7 pontos porque o foi decidido usar uma abstração chamada ```FindBy``` para isolar o tratamento do retorno Opcional da busca pelo id da ```Compra```.

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

Saímos de 8 para 5 sem criar novos arquivos, códigos etc. Usei o ```EntityManager``` direto no nosso mais novo ApplicationDomain Service? Usei, e daí :)?

E se eu puder processar compras através de outras entradas do sistema? Adapte o código :). Quando um sistema cresce, pouco importa se a arquitetura é monolítica ou distribuída, você vai perdendo o controle do que está pronto ou não. No fim, você não precisa ter medo de mudança, basta que ela seja mais fácil de ser realizada. Você agora tem um fluxo com carga intrínseca baixa e que pode ser fatiado em caso de necessidade. 


### Domain Services 100% coesos

Preciso achar um código de exemplo aqui. Na jornada todos controllers podem ser refatorados para simplesmente usarem mais o model, entitymanager etc para ficarem mais simples. Se eu não tiver outro exemplo, deixo só a explicação mesmo ou invento alguma coisa.

### Form Value Objects (minha adaptação de value object para classes de formulário)

Aqui é um caso clássico de fluxo de aplicações web. Você tem um formulário de cadastro ou de pesquisa na sua aplicação e precisa receber dados de modo que em algum momento eles possam ser convertidos em um objeto que faz parte do model, geralmente as entities. Durante um tempo, por conta da facilidade provida pelos frameworks, os objetos do model eram montados diretamente a partir dos parâmetros pelos próprios frameworks. 

```
 	@PostMapping(value = "/usuarios")
	@Transactional
	//abre uma transacao
	public void novo(@Valid Usuario novoUsuario) {		
		manager.persist(novoUsuario);
	}

```

A tragédia era anunciada, mas precisou acontecer um caso realmente impactante para que o mundo ficasse mais atento ao problema. Lá em 2012 o Github sofreu um hack((https://github.blog/2012-03-04-public-key-security-vulnerability-and-mitigation/).) através de uma técnica conhecida como mass assignment(https://en.wikipedia.org/wiki/Mass_assignment_vulnerability). 

Um outro problema é específico ao contrato. O parâmetro aqui é um contrato firmado com outra aplicação cliente. Neste exemplo, uma mudança no model pode acarretar uma mudança de contrato indesejada e o pior, que só seria percebida na hora do uso em si. 

Essa combinação deixou bem claro que era necessário controlar tudo que vem do cliente. Inclusive o Sonar, software que avalia qualidade de código, possui uma regra que negativa seu código quando entities aparecem como argumentos. O engraçado é que isso já era algo trabalhado no Struts 1, muito antes do Github. Tudo fica de novo de novo. 

A partir dali, o mesmo endpoint começou a ser escrito da seguinte forma:

```
	@PostMapping(value = "/usuarios")
	@Transactional
	//abre uma transacao
	public void novo(@Valid NovoUsuarioForm form) {	

		Usuario novoUsuario = NovoUsuarioConverter.converte(form);
		manager.persist(novoUsuario);
		
	}

```

A classe que converte entrada de dados em objetos de domínio ficou tão famosa que até bibliotecas foram criadas em cima dela. Um exemplo disso é a ModelMapper(http://modelmapper.org/getting-started/). Precisamos sempre ter em mente que criamos novos arquivos para distribuir a carga cognitiva do sistema ou para utilizar uma funcionalidade da linguagem que casa com o novo arquivo. No caso acima, o mesmo código poderia ser escrito da seguinte forma:

```
	@PostMapping(value = "/usuarios")
	@Transactional
	//abre uma transacao
	public void novo(@Valid NovoUsuarioForm form) {	
		Usuario novoUsuario = new Usuario(form.getNome(),form.getEmail())
		manager.persist(novoUsuario)		
	}

```

Não aumentamos em nenhum ponto a carga intrínseca do código e atingimos o mesmo resultado. E se esse form fosse muito maior, com muitos dados, relacionamentos etc? Vamos pegar um outro exemplo. 

```
@RestController
public class CrudPerformanceReviewController {
	
	@Autowired
	private GoalRepository goalRepository;
	@Autowired
	private PerformanceReviewRepository performanceReviewRepository;
	@Autowired
	private SystemUserRepository systemUserRepository;

	@PostMapping(value = "/api/performance/review")
	@Transactional
	public void save(@RequestBody @Valid NewPerformanceReviewForm form) {
		List<PerGoalPerformanceReviewRequiredInfo> reviews = form.getReviewsForm().stream().map(reviewForm -> {
return new PerGoalPerformanceReviewRequiredInfo() {

			@Override
			public String getPositivePoints() {
				return positivePoints;
			}

			@Override
			public String getImprovingPoints() {
				return improvingPoints;
			}

			@Override
			public NextGoalStep getNextGoalStep() {
				return nextGoalStep;
			}
			
			@Override
			public Goal getGoal() {
				return goalRepository.findById(goalId).get();
			}
			
		};

		}).collect(Collectors.toList());
		
		SystemUser employee = systemUserRepository.findById(employeeId).get();
		
		Optional<SalaryReviewInfo> salarayReviewInfo = salaryReview ? Optional.of(salaryReviewForm.toModel()) : Optional.empty();
		PerformanceReview newReview = new PerformanceReview(employee,reviews,salaryReview,salarayReviewInfo);

		performanceReviewRepository.save(newReview);
		
		
	}

}

```

Por conta do formulário com mais informações e com mais necessidades de transformação, naturalmente a carga intrínseca do código aumentou. Somando o acoplamento contextual e  branches(map + if ternario) temos 10 pontos de carga intrínseca. Passou de 9? Vamos precisar refatorar. De novo o converter poderia entrar no jogo:

```
@RestController
public class CrudPerformanceReviewController {
	
	@Autowired
	private NewPerformanceReviewConverter newPerformanceReviewConverter;

	@PostMapping(value = "/api/performance/review")
	@Transactional
	public void save(@RequestBody @Valid NewPerformanceReviewForm form) {
	PerformanceReview newReview = newPerformanceReviewConverter.convert(form);
		performanceReviewRepository.save(newReview); //aqui poderia ser o EntityManager direto :)				
	}

}

```
Essa solução resolve? Sem dúvida. Atende a carga cognitiva? Atende também. Mas no fim ela aumenta a carga cognitiva do sistema como um todo em 1 ponto, já que temos uma nova classe. E nós sabemos que não estamos falando apenas de um converter por sistema. O mesmo código poderia ser resolvido da seguinte forma:

```
@RestController
public class CrudPerformanceReviewController {
	
	@Autowired
	private PerformanceReviewRepository performanceReviewRepository;
@Autowired
	private GoalRepository goalRepository;
	@Autowired
	private SystemUserRepository systemUserRepository;

	@PostMapping(value = "/api/performance/review")
	@Transactional
	public void save(@RequestBody @Valid NewPerformanceReviewForm form) {
	PerformanceReview newReview = form.toModel(goalRepository,systemUserRepository);
		performanceReviewRepository.save(newReview);				
	}

}

```

Perceba que mantemos a carga cognitiva do controller abaixo de 7, evitamos a criação de uma nova classe e conseguimos implementar a mesma funcionalidade. O que machuca os olhos é esse método ```toModel``` combinado com argumentos que representam repositórios? O método ```toModel``` associa estado + comportamento combinando com parâmetros recebidos. Era justamente essa a proposta de Barbara Liskov no artigo Tipos Abstratos de Dados(link).  Admito que desconheço melhor uso do paradigma. 

E você pode se perguntar, então por que eu não deixo literalmente toda a inteligência dentro das classes de model e forms da aplicação? Porque você estouraria a carga intrínseca máxima por arquivo. Não vai estourar? Experimente. Se liberte da ditadura da divisão de responsabilidade e camada. Sem experimentação, não tem questionamento e nem geração de conhecimento. 

Para fechar, você pode olhar para a classe de formulário como uma extensão do Value Object do DDD. Ali ele cita que tais classes existem para guardar um estado que pode ser temporário ou persistente, mas que não necessariamente tem uma identidade. O formulário apenas não se encaixa no que ele chama de camada de model da aplicação, mas se relaciona com a explicação em si do pattern. Você pode chamar as classes de formulários de Form Value Objects :). 

### Maximize a manipulação de estado dentro de entities, value objects e suas variações.

Aqui é uma consequência das sugestões citadas e também que reforçada pelo DDD. Quando restringimos a carga intrínseca das partes procedurais da nossa aplicação web, naturalmente vamos mover parte da inteligência para nossas entities e value objects. Só que precisamos de algumas restrições. Para trabalharmos o exemplo, vamos pegar um código implementado para aceitar a participação de uma pessoa em um bolão entre amigos. 

```
    public ResultadoConfirmacao model(BolaoRepository bolaoRepository, ParticipanteRepository participanteRepository, UsuarioRepository usuarioRepository) {
        final Bolao bolao = bolaoRepository.findById(this.idBolao).get();
        final Participante participante = participanteRepository.findById(this.idParticipante).get();

        if (bolao.getDataExpiracao().isBefore(Instant.now())) {
            return ResultadoConfirmacao.conviteExpirado();
        }

        final Optional<Usuario> possivelUsuario = usuarioRepository.getByLogin(participante.getEmail());
        if (possivelUsuario.isEmpty()) {
            return ResultadoConfirmacao.usuarioNaoExiste();
        }

        final Usuario usuario = possivelUsuario.get();

        return new ResultadoConfirmacao(new Participacao(bolao, usuario));
    }



```

Esse trecho de código em si tem 8 pontos de carga cognitiva e está em um método de transformação num Form Value Object. A métrica de carga cognitiva ia deixar passar, o que eu até acho bom para entendermos que ainda não estamos num ponto infalível. Ainda precisamos combinar uma métrica complicada de análise de código O.O. 

Uma ```Participacao``` é resultado da combinação entre um usuário e um bolão específico. E é justamente  o que o código tenta fazer. O problema é a falta de uso de restrições oferecidas pela própria linguagem. Inclusive ainda faltou uma verificação para analisar se o participante está no mesmo no conjunto de convites do bolão. Teria mais um if :). O mesmo código poderia ser escrito da seguinte forma:

```
    public ResultadoConfirmacao model(EntityManager manager, UsuarioRepository usuarioRepository) {
        final Participante participante = manager.find(this.participanteId,Participante.class);
        final Optional<Usuario> possivelUsuario = usuarioRepository.findByEmail(participante.getEmail());

       if(!possivelUsuario.isPresent()){
 	//exception
       }

        final Bolao bolao = manager.find(this.bolaoId,Bolao.class);

        return  bolao.aceita(usuario);
    }

```

E agora o método ```aceita``` da classe ```Bolao``` poderia ser assim:

```
  public ResultadoConfirmacao aceita(Usuario participante) {
       if (this.dataExpiracao.isBefore(Instant.now())) {
            return ResultadoConfirmacao.conviteExpirado();
        }

        if (!this.emails.contains(participante.getEmail()))) {
            return ResultadoConfirmacao.naoConvidado();
        }        

        return new ResultadoConfirmacao(new Participacao(this, usuario));     
  }

```

O local de invocação do método ```aceita``` não está aderente ao sugerido na explicação do  Form Value Object, mas isso é apenas um detalhe aqui. O importante é que concentramos as operações sobre o estado em quem possui o estado em vez deixar para quem tem acesso externo ao estado.

O resultado final é uma carga intrínseca ainda melhor distribuída entre as partes da aplicação fazendo uso dos recursos providos pela linguagem. 

## Conclusão

A ideia do DDD do proletariado é realmente trazer as práticas sugeridas por Eric Evans para um cenário mais específico. Como aplicamos tais práticas em um cenário de aplicações web construídas em cima de frameworks que abstraem boa parte do trabalho repetitivo? 

Abraçamos o acoplamento inteligente com as tecnologias fundamentais para a aplicação, e entendemos que a análise de carga cognitiva precisa ser feita modo contextual. Inclusive sugerimos como podemos analisar de maneira lógica a carga intrínseca de cada ponto do nosso código. 

Para fechar foi deixado algumas sugestões de práticas de código que podem facilitar a aplicação dos conceitos que foram trabalhados. 

