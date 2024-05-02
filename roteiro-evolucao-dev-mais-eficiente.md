## Triturador(a) de Requisitos

### Leitura Científica

* [Naming the Pain in Requirements Engineering](https://elib.uni-stuttgart.de/bitstream/11682/8847/3/EMSE-D-15-00239_postprint.pdf) - Uma revisão muito interessante sobre os principais desafios quando falamos de refinar requisitos. É uma pesquisa que foi feita em vários países tentando buscar uma base mais sólida sobre este tema.
* [Integrated Requirements Engineering: A Tutorial](https://ifs.host.cs.st-andrews.ac.uk/Research/Publications/Papers-PDF/2005-09/IntegratedRETutorial.pdf) - Um tutorial escrito por Somerville em 2005 mais direto ao ponto sobre técnicas para levantamento de requisitos. Mesmo "antigo", ainda considero muito útil.
* [Status Quo in Requirements Engineering: A Theory and a Global Family of Surveys](https://arxiv.org/pdf/1805.07951) - Aqui é uma espécie de continuação do trabalho feito no artigo "Naming the Pain in Requirement Engineering".

### Livros que podem ajudar

* [Capítulo sobre engenharia de requisitos do livro Software Engineering](https://engineering.futureuniversity.com/BOOKS%20FOR%20IT/Software-Engineering-9th-Edition-by-Ian-Sommerville.pdf) - Um capítulo mais formal sobre engenharia de requisitos. Explica desde o básico e mesmo assim considero saudável.
* [Shape Up](https://basecamp.com/shapeup) - Livro muito interessante escrito pelas pessoas da 37 Signals, empresa criadora do Basecamp e Hey. A pegada forte em design faz com que este livro seja um guia muito útil para deixar só o que realmente faz diferença no produto. **Eu realmente recomendo você ler este livro**. Sem contar que é gratuito, quando lido online.

### Rotina 1 para trabalhar essa habilidade

Sempre que tiver a oportunidade, pegue uma história antiga e analise a qualidade dela. Aqui deixamos alguns detalhes para te ajudar a analisar, mas fique a vontade de seguir o que considerar melhorar levando em consideração as teorias que passamos. Segue as perguntas que podem ser feitas. 

* Existe um motivo detalhado para justificar a existência da história ?
* Qual o benefício esperado com aquela implementação ?
* Existe o contato para a fonte original que realmente pediu a implementação ?
* A descrição da história fala, detalhadamente, do que é obrigatório, opcional e possíveis fluxos para casos de erro ?
* Existe uma versão da história escrita em "modo dev" dando uma visão mais detalhada de como ela vai ser implementada ?
* A história segue um template que facilita a identificação dos itens obrigatórios ?

### Rotina 2: Para cada nova história, realize a análise de qualidade das histórias

* Existe um motivo detalhado para justificar a existência da história ?
* Qual o benefício esperado com aquela implementação ?
* Existe o contato para a fonte original que realmente pediu a implementação ?
* A descrição da história fala, detalhadamente, do que é obrigatório, opcional e possíveis fluxos para casos de erro ?
* Existe uma versão da história escrita em "modo dev" dando uma visão mais detalhada de como ela vai ser implementada ?
* A história segue um template que facilita a identificação dos itens obrigatórios ?

### Rotina 3: Para cada história crie uma versão "dev" dela

Para cada história, crie uma versão escrita do ponto de vista da pessoa que vai desenvolver. A sugestão aqui é se inspirar no método [Cognitive walkthrough](https://en.wikipedia.org/wiki/Cognitive_walkthrough). 

Uma vez que a história está escrita, você deve escrever um passo a passo bem detalhado de como seria a implementação daquilo. Essa é uma forma menos custosa do que implementar de verdade e só aí perceber que pode estar faltando alguma coisa. Durante esse processo, você pode perceber que falta informação, que o que foi pensado faz menos sentido do que deveria etc. 

Lembre que triturar requisito é a forma mais eficiente de você reduzir o custo de implementação. Você está buscando problemas antes de fazer o código de fato. 

Segue um exemplo que Alberto fez para uma funcionalidade relacionada a geração e aceite de convite para uma determinada conta. 

_Meu algoritmo para geração de convite_

1. Recebe o id global da conta, emails e data de expiração do convite no método do controller. Aqui já faz a validação básica...
2. Verifica se a pessoa logada é dona da conta. Só quem é dono da conta pode gerar convites.
3. Cria um novo convite com id global também e data de expiração
4. Grava o convite
5. Manda email (serviço de emails) para os emails que serão convidados
6. Suporta idempotencia com o lance do workflow implementado no desafio da hotmart
7. Retorna os links para aceites de convite

_Meu algoritmo para aceite do convite_

1. Pessoa logada acessa o link do convite
2. Verifica se a pessoa logada é a mesma do convite
3. Se for a mesma, adiciona na conta de destino do convite
4. Marca o convite como aceito.
5. Se a pessoa já estiver na conta, simplesmente retorna dizendo que está tudo certo, como se fosse o primeiro.
6. Retorna status com corpo dizendo que a pessoa foi adiciona na conta.

PS: De brinde, uma descrição muito bem feita pode servir de entrada para um LLM que talvez te ajude na implementação. 

## Excelência técnica em qualidade de código

* [Playlist com vídeos gerais sobre qualidade de código no canal](https://www.youtube.com/playlist?list=PLVHlvMRWE0Y4WmkV47XaWeF-Xfl_lGq8S)
* [Descubra o poder do CDD](https://arxiv.org/pdf/2206.10655)
* [Live refatorando código via CDD](https://youtube.com/live/HTk_--BLiSc?feature=share)
* [Playlist com lives de almoço revisando textos, artigos e fazendo algumas entrevistas](https://www.youtube.com/playlist?list=PLVHlvMRWE0Y4tqa1J67wlKzlGoOrEnjMn)
* [Revisão de artigos científicos e textos marcantes](https://www.youtube.com/playlist?list=PLVHlvMRWE0Y7dh2L8ncst42M9YjLMfcpx)
* [Mini curso sobre DDD](https://www.youtube.com/playlist?list=PLVHlvMRWE0Y5AYGpKv7-mlwd2xk8vDNEv)
* [Mini curso sobre SOLID](https://www.youtube.com/playlist?list=PLVHlvMRWE0Y6cFjgq6BPGyx_FW94NoZxg)
* [Realize sua inscrição na Jornada Dev + Eficiente :)](https://deveficiente.com/)

### Análise de código de projetos open source

Quer ser uma versão em melhor em entender código dos outros, identificar as construções e ainda sugerir caminhos de melhoria? Então é necessário analisar o máximo de código alheio possível. Aqui tem a nossa sugestão para você treinar essa habilidade. 

Escolha no mínimo três projetos open source diferentes que você utiliza no seu dia a dia, crie objetivos de aprendizagem considerando o nível de "avaliação" da taxonomia de bloom relacionados ao código fonte de cada um deles e siga o plano até ser capaz de:

* Explicar detalhadamente parte do código fonte identificando os componentes
* Sugerir um caminho de refatoração para melhorar determinada parte do código (Olha a chance de usar o CDD para guiar a refatoração)
* Abrir o código e de fato implementar a refatoração (aqui não precisa ser algo para mandar PR, é para você mexer no código alheio mesmo)

### Analisando os dez piores códigos do seu trabalho

Quer ser uma versão em melhor em entender código dos outros, identificar as construções e ainda sugerir caminhos de melhoria? Então é necessário analisar o máximo de código alheio possível. Aqui tem mais uma  sugestão para você treinar essa habilidade. 

Escolha no mínimo os dez piores arquivos com código(pode ser classes, funções mesmo etc) do seu trabalho e faça uma avaliação detalhada de cada um. Que tal definir uma métrica via CDD e usá-la para avaliar os "piores" arquivos ?

* Explicar detalhadamente parte do código fonte identificando os componentes
* Sugerir um caminho de refatoração para melhorar determinada parte do código (Olha a chance de usar o CDD para guiar a Refatoração)
* Implementar de fato a refatoração (aqui não precisa ser algo para mandar PR, é para você mexer no código alheio mesmo)

## Excelência técnica em uma ou mais tecnologias específicas

* Escolher uma tecnologia específica que é usada no seu trabalho e que você sente que manjar mais pode ser um diferencial.
* A cada semana faça um resumo de tudo que você estudou relacionado especificamente sobre esta tecnologia.
* Você vai ler toda a documentação oficial relacionada a esta tecnologia.
* Para cada "capítulo" dessa documentação, você vai ter que analisar o que já usou no trabalho e/ou analisar uma situação que poderia ter usado aquilo e não usou.
* Para cada capítulo você também precisa montar uma aula explicando aquilo. Pode ser em formato de blog, vídeo etc.
* Fica a sugestão de tentar achar o pedaço de código fonte da tecnologia que corrobora o que você vem estudando.

### Escolha no mínimo uma tecnologia que você quer se tornar referência

Cada tecnologia escolhida para desenvolver uma aplicação tem um papel fundamental no que diz respeito a aumentar a velocidade do desenvolvimento. Supostamente, cada escolha deve trazer consigo um conjunto de abstrações que simplifica o trabalho. 

Ao mesmo tempo, não é incomum, que estas tecnologias sejam usadas aquém da sua capacidade máxima. É até comum encontrarmos códigos implementados que poderiam ser substituídos pelo melhor uso de alguma das tecnologias já presentes no projeto. Sem dúvida isso diminui a eficiência da equipe, pois energia está sendo investida em algo que supostamente está pronto. 

A ideia aqui então é que você olhe para o ecossistema de tecnologias usadas no seu contexto e escolha pelo menos uma que você realmente quer dominar, tornar-se uma referência. 

Para cada possível escolha, reflita sobre o motivo. No final, priorize e decida qual é a mais importante para começar. 

### Construa planos de aprendizagem para realmente dominar as tecnologias selecionadas

Com sua lista de tecnologias priorizadas, agora chegou a hora de construir o plano de aprendizagem para cada uma delas. Deixamos a sugestão que você faça primeiro o plano para a tecnologia prioritária, siga e depois vá para a próxima. Se sentir que tem tempo, pode tentar seguir mais de um plano. 

Aqui invista tempo para construir o plano completo! O seu objetivo de aprendizagem deve buscar o nível analítico ou de avaliação. 

### Reflexão em cima das fontes teóricas

Para cada fonte teórica do seu plano de aprendizagem, você deve escrever um post de blog trazendo seu entendimento sobre o assunto. Se a fonte teórica for um livro, documentação ou algo similar, escreva posts por capítulos ou até seções, tudo vai depender da complexidade do tema tratado. 

Lembre que esas reflexões, geralmente, fazem parte das práticas acessórias do seu plano de aprendizagem. Elas te apoiam para ser uma versão melhor na hora de aplicar, analisar ou avaliar alguma situação em cima daquele tema. 

### Maximizando o ganho de conhecimento em cima das práticas prioritárias

Antes de começar a fazer de fato o exercício que você planejou dentro das práticas prioritárias, escreva um passo a passo cognitivo(Cognitive Walkthrough) do que você vai fazer. Aqui é algo bem similar ao que trabalhamos no pilar de triturador de requisitos. Você quer chegar na implementação com o máximo de visão possível. [Para saber mais sobre a técnica acessa aqui.](https://en.wikipedia.org/wiki/Cognitive_walkthrough) 

Depois de realizar a implementação faça o debreafing(post mortem) da sua atuação. Quais foram as principais lições? O que teve mais dificuldade? O que já pode levar para seu trabalho?

O exagero no processo de reflexão vai aumentar demais sua capacidade analítica. 

### Realize análises sobre o uso da tecnologia dentro do seu contexto de trabalho

O aumento do seu domínio sobre cada tecnologia que você priorizou para se aprofundar vai possibilitar, cada vez mais, que você enxergue oportunidades de melhoria no seu contexto. 

Transforme essa possibilidade num exercício regular da sua vida. Nem sempre você vai conseguir realizar a alteração que gostaria, mas o trabalho analítico contínuo vai jogar sua capacidade para outro nível. Afinal de contas você está sendo capaz de enxergar componentes e eventuais oportunidades de melhoria em cima de pedaços de código que não foi você que escreveu. 

## Especialização no domínio

### Construa um plano de aprendizagem para elevar seu domínio sobre o negócio

Buscar entender da maneira mais profunda possível o negócio que você está atuando é de fundamental importância para ampliar sua capacidade de tomar boas decisões quando falamos do código. Trabalhamos muito isso no módulo de DDD da Jornada Dev Eficiente. 

A ideia aqui é que você olhe para seu negócio e monte um plano de aprendizagem cujo objetivo de aprendizagem gire em torno de te deixar capaz de explicar com tranquilidade sobre o negócio que você faz parte.

Inclusive essa é uma prática que você deve levar para qualquer trabalho. 













