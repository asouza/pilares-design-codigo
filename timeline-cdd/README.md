## Breve história

Aqui tem a timeline sobre a construção da tese de desenvolvimento de Cognitive Driven Development(CDD).

### Post no medium sobre combinação de ideias

No dia 28/04/2020 fiz um post no medium de título "DDD para aplicações web modernas". Ali foi a primeira vez que eu juntei tudo que estava na minha cabeça e coloquei em formei de texto. Você pode fazer a leitura completa [aqui](https://medium.com/@albertosouza_47783/ddd-para-aplica%C3%A7%C3%B5es-web-modernas-2be654932497). 

### Primeira publicação no ICSME

Depois do post do medium um doutorando da UFPE, Marcio Silva, me chamou no linkedin e disse que ali tinha material para ser construído um mestrado ou até um possível doutorado. Aquela conversa aconteceu logo depois que eu tinha acabado de entrar na Zup. 

Aquela conversa ficou na minha cabeça e, contando com o apoio da Zup, eu contratei um outro doutor, [Victor Santiago](https://www.linkedin.com/in/victorhugosantiago/). Ele já entrou trabalhando na tese, inclusive foi o próprio que sugeriu o nome Cognitive Driven Development. 

Depois de pesquisa extensa por vários trabalhos, ficou claro que tínhamos uma tese de design interessante e que não tinha sido explorada pela ciência ainda. Entender o "cognitive load" de um código era tema de pesquisa e tinha muito cruzamento com a psicologia da cognição, entretanto não tinha um trabalho na direção de realmente facilitar a escrita de código de modo a deixá-lo mais fácil de ser entendido. 

Apostando nisso, mandamos o nosso primeiro trabalho para o ICSME(International Conference on Software Maintenance and Evolution) e ele foi aceito! Você pode ler o artigo [aqui](https://github.com/asouza/pilares-design-codigo/blob/master/ICSME-2020-cognitive-driven-development.pdf). 

### Segunda publicação no ENASE

Depois do aceite na primeira conferência, inclusive com feedbacks muito bons, ficamos ainda mais empolgados e partimos para a construção do nosso primeiro experimento. Queríamos ter uma ideia dos efeitos do CDD na atividade de refatoração. Refatoração como vetor de melhoria de qualidade de código é uma das crenças exploradas pela área de Engenahria de software empírica. 

Existem alguns trabalhos mostrando que refatorações deixaram o sistema num estado pior do que foi encontrado. Entre alguns dos indicadores utilizados estão números de bugs naquela parte do código pós refatoração e piora nos números relativos a métricas mais formais como coesão, acoplamento, linhas de código, complexidade ciclomática etc. Claro que também existem os trabalhos mostrando a melhora na qualidade do código. 

Dado este contexto, bolamos um experimento com as pessoas da Zup onde parte tinha que refatorar dois códigos utilizando as técnicas convencionais e outra parte tinha que refatorar utilizando as técnicas convencionais somado ao CDD. O resultado foi muito interessante e mostrou que os códigos refatorados utilizando o CDD ficaram muito mais saudáveis pelo ponto de vista das métricas mais formais. 

Escrevemos um artigo sobre o experimento e ele foi aceito na ENASE(Conference on Evaluation of Novel Approaches to Software Engineering). O artigo pode ser lido [aqui](https://github.com/asouza/pilares-design-codigo/blob/master/CDD-Preliminary-results-Refactorings.pdf). 

### Terceira publicação, essa foi rejeitada

Continuando o trabalho sobre o CDD, também tentamos exlporar os efeitos da limitação de complexidade nos momentos iniciais da criação de um código. Para isso construímos outro experimento na Zup. 

Neste experimento a ideia foi dividir as pessoas em vários grupos onde alguns tinham que começar a codar usando um guia claro de métricas e outro tinha que fazer a mesma coisa com o mesmo guia, só que a diferença é que este grupo tinha um limite para o número somado da complexidade. 

O resultado não foi tão impactante quanto o experimento anterior, mas tivemos bons números em relação a dispersão da complexidade. Enquanto a maioria dos códigos que foram produzidos sem a ideia de limite tiveram médias com dispersões altas, os códigos que foram produzidos com a ajuda do CDD tiveram médias com dispersões mais baixas. Ou seja, a média era mais "real". 

Neste trabalho pecamos na escrita e na descrição do experimento, de acordo com o feedback da banca avaliadora. De toda forma, achamos que ele pode ser útil. Você pode ler o trabalho completo [aqui](https://github.com/asouza/pilares-design-codigo/blob/master/Effects-of-the-Cognitive-Driven-Development-in-the-early-stages-of-the-software-development-life-cycle.pdf). 

### Quarta publicação, SBES

A nossa última publicação foi no SBES(Simpósio brasileiro de engenharia de software). Nele foi apresentado o plugin construído por Victor e seu aluno Gherson para a IDE IntelliJ. O plugin permite que uma métrica seja configurada e um limite estabelecido. Com esta configuração ele analisa cada classe do projeto e vai dizendo se o limite está ok. 

Este foi o primeiro passo para tentarmos popularizar mais o CDD e também começar a olhar para eventuais novos trabalhos no futuro, principalmente no campo de sugestões de refatoração. Você pode ler o trabalho completo [aqui](https://github.com/asouza/pilares-design-codigo/blob/master/Cognitive-Load-Analyzer-A-Support-Tool-for-Cognitive-Driven-Development.pdf). 

## CDD no canal Dev eficiente

Você pode acessar diversos vídeos sobre a utilização do CDD no canal [Dev Eficiente](https://www.youtube.com/results?search_query=deveficiente+cdd). Nestes vídeos Alberto refatora um monte de código guiado pelo CDD. 

## Futuro da pesquisa sobre CDD

Temos o interesse de mostrar, através de pesquisa, que o CDD é o primeiro princípio de Design de código que de fato te ajuda a produzir código mais interessante de maneira sistemática. Os resultados até agora tem sido bem interessantes e esperamos ter mais evidências para suportar o nosso discurso. Entendemos que engenharia de software permite que tudo seja testado sem nenhuma evidência que aquilo funciona e achamos isso muito bom. Por outro lado, parece que temos uma boa oportunidade de usar um approach mais científico para sugerir algo que realmente possa ajudar. 

Um outro possível interesse é ampliação dos estudos sobre a aplicabilidade do CDD para outros níveis do ciclo de desenvolvimento de software. 

    * Será que é aplicável em nível arquitetural? Modular? 
    * Quais são os possíveis problemas que serão criados pelo fato de agora termos a possibilidade de dividir código pela limitação de processamento humano e não apenas pela eventual responsabilidade? 
    * Parece que o C#, por conta das partial classes suportadas pela linguagem, seria uma ótima linguagem para usarmos o CDD. Como que os experimentos ficariam por lá?
    * Será que é viável usar o CDD como guia de sugestões de refatoração automática?

## Entre em contato

Achou interessante? É só entrar em contato através [linkedin](https://www.linkedin.com/in/alberto-souza-953b0b7/) ou [twitter](https://twitter.com/alberto_souza). 

