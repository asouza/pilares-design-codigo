Aqui tem minhas notas para a gravação do episódio do youtube sobre o artigo Dynamics of Software Maintenance.

**Resumo**

O artigo é de 2004 e ele abre dizendo que o crescimento da indústira de tecnologia faz com que a quantidade de software que necessita de manutenção vai crescer rapidamente.  Isso abre oportunidade para que as empresas de prestação de serviço sejam as responsáveis pela manutenção de tais softwares. 

Por outro lado é muito complicado fazer uma estimativa de esforço de manutenção dado que não se sabe a complexidade, confiabilidade  e outros fatores sobre o software.  Além disso, a atividade ainda é complicada por conta de fatores humanos e gerenciais. Possibilidade de colher feedback direto do cliente em um tempo aceitável, atitude do time de engenharia relacionada a atividade de manutenção etc. 

O artigo diz que a atividade de manutenção é igual ou mais importante do que a atividade de criar o software em si e que tal atividade ainda não ganhou a importância que deveria. 

Dado esse contexto, o estudo tenta descrever um approach holístico para tentar estimar o nível de esforço necessário para realizar a manutenção do software.


**Introdução e motivação**

_IEEE [11] defines software maintenance as:  The modification of a software product after delivery to correct faults, to improve performance or other attributes or to adapt the product to a modified environment. _

E ele abre falando que é sabido que o esforço colocado na etapa de desenvolvimento corresponde entre 25% a 33% do tempo dentro do ciclo de vida do software, que o resto é para manutenção. 

Na parte de introdução e motivação o artigo chama atenção sobre o estilo de consultoria que as empresas desejam no que tange as atividades de manutenção de software. É citado que em um momento mais inicial, as empresas pagavam sob demanda pelo número de pessoas que estavam envolvidas nas atividades de manutenção. Só que agora elas querem receber um valor fixo por determinado período de alocação e, neste novo cenário, faz-se  necessário calcular de uma forma melhor o esforço necessário para sustentar o software.

Também é citado o fato que muito já foi feito para calcular o esforço do desenvolvimento de um software, mas que o trabalho foi mais focado na fase de desenvolvimento do que de manutenção. E aí a seção do artigo fala que essas atividades são bem diferentes. Na maioria das vezes a equipe que vai manter o software é diferente da equipe que criou.  Sem contar que as tecnologias sendo usadas para a criação estão ficando vez mais complexas(isso em 2004, ele não sabia o que ia acontecer nos próximos 16 anos).

Para fechar é explicado que softwares de mercado geralmente sofrem com mudanças de escopo em função das necessidades de negócios que vão mudando. 

**Issues and Factors Affecting Software Maintenance **

Aqui é tentado listar os fatores que influenciam a manutenção do software. 

_Programador_:

Muitas vezes o time envolvido nas atividades de manutenção consideram que tal atividade é menos interessante do que a fase de criação. Isso pode levar a um foco maior no uso da tecnologia apenas por usar, sem o link necessário com a necessidade de negócio em questão. 

Além disso, influencia também a falta de domínio do negócio, de habilidade técnica, de domínio da aplicação em si. 

Para fechar aqui, ainda é falado que times espalhados pelo mundo podem sofrer de falta de comunicação(nÃo precisa espalhar pelo mundo não).

_Management Attitude_

Aqui é citado que a liderança geralmente separa pouco tempo que o time possa conhecer o software que já foi escrito e, em cima disso, muitas vezes cobrar prazos de manutenção completamente surreais. O artigo também cita que existe uma espécie de separação entre as pessoas que são alocadas para desenvolver coisas do zero Vs alocação para manutenção. 

_Code Baseline _

Aqui é citado o básico: o código é a peça central de manutenção e a falta de qualidade interna, documentação, falta de padrões de manutenção etc influenciam demais no esforço de manutenção. 

_Usuários_ 

É citado que é de fundamental importância ter a participação dos usuários no processo de manutenção. 

**Proposed Estimation Model **

Dado o contexto, segue o objetivo do artigo:

_Define, calibrate and validate a holistic model to predict the effort expended in maintenance activities over a specified period, for a software system in light of the impact of various dynamic factors influencing it. _

Aqui é falado que será aplicado a ideia de System Dynamics(dinâmicas de sistemas), a qual eu não vou explicar aqui por falta de conhecimento, para conseguir achar um modelo de estimativa de esforço para manutenção. 

Uma breve defnição, trazida no próprio artigo:

** Systems dynamics [6], [7], [8] is a method for studying the world
in a holistic manner because it looks at systems as a whole, rather
than small fragments. The interaction of objects within a system, is
extremely important in systems dynamics. Systems dynamics attempts to understand the basic structure of a system, and thus understand the behavior it can produce**

Numa visão de alto nível, os dois fatores que mais influenciam na manutenção são

* Capability 
* Attitude

Agora, dando um zoom nos fatores, temos:

* Dentro de capability
  * Software Baseline Characteristics
  * Maintenance Team Capability (MTC) 

* Dentro de attitude
  * Customer attitude
  * Management attitude 

E ele deixa claro que o fator principal é a capability. 

**Dentro do SBC, o que é importante ser considerado**

* Tamanho do código
* Análise de complexidade através de métricas formais
* Índice de manutenibilidade (um exemplo é Mean time to recover(MTTC)) 
* Histórico de manutenção(número de problemas encontrados no software)
* Documentação(qualidade e múltiplas perspectivas de visão)

**Dentro do MTC, o que é importante ser considerado**

* Technical Expertise 
* Domain Expertise
* Application Knowledge
* Programmer Attitude(aqui podemos olhar pelo espectro da motivação)

**Customer Attitude**

* Business IT Alignment (time técnico está completamente alinhado com o interesse de negócio. Muitas vezes essa falta de alinhamento leva a interpretação equivocada de funcionalidades etc.)

* Team Stability (ficar trocando o tempo todo as pessoas responsáveis por facilitarem o entendimento do negócio tende a ser prejudicial)

* Involvement (o quão disponível o cliente está para responder para o time de produto)

* Scope Creep (clássica mudança maluca de escopo o tempo todo... não tem agilidade que sobreviva)

**Management Attitude**

* Assignment Priority (deixar claro que manter o sistema é igual ou mais importante do que desenvolver coisas do zero. A liderança precisa deixar isso claro). Um exemplo dado no artigo é treinar a galera em atividades de manutenção

* Investment Motivation (volta para o lance da manutenção)

* Target Definition (aqui é falado sobre deadline malucos e que causam uma pressão desnecessária na equipe)

* Developer Involvement (maximizar a chance de conseguir colocar as pessoas que desenvolveram também para manter o sistema, ou pelo menos participar disso)


**Ameaças**

* Será que o que foi levantado aqui realmente vai fazer sentido em projetos reais? Inclusive tem um outro artigo, como se fosse continuação desse, que traz uma pesquisa sobre o tema analisando as correlações. 

**Minha percepção**

* Eu confesso que minha observação me faz acreditar que o modelo proposto meio que aconteceu e continua acontecendo no mundo de hoje. Já é meio que nítido que o maior esforço no ciclo de vida do software está nas atividades que envolvem manutenção preventiva, corretiva etc. 

* Também considero, como falei no vídeo sobre o que esperar do canal em 2021, que ainda nos preocupamos como velocidade de deploy, CI/CD e não sei mais o que... Olhamos para Mean Time To Recovery(MTTC) e tal e não damos a devida atenção para a saúde do paciente... Prevenção é, e provavelmente vai continuar sendo, o melhor remédio. 

* Em empresas gigantes e mais antigas, ainda existe muito o modelo de terceirização do serviço de criação e posterior manutenção(nos mais variados aspectos) do software. E o desafio de estimar esforço meio que continua... No mundo das big corps, ainda encontramos mutações do ponto de função(eu nem sei calcular), como o Business Complexity Point. E para ser sincero, tal medida de esforço usada de maneira isolada não conta muito, mas é um indicador... Podemos olhar sempre a relação de esforço com entrega de valor. Agora se a medida for usada unicamente para julgar gente sem a devida adequação de contexto, aí fica perversa. 

* No mundo startupeiro, o modelo que vejo é a da própria empresa tomar conta da manutenção do seu software. Mas isso não significa que o mesmo time que criou vai manter, de maneira alguma. A rotação é grande dentro do mundo de desenvolvimento e você quase nunca tem continuidade na equipe. O desafio de aprendizado sobre o negócio, sistema e tecnologias envolvidas continua. 

* Sobre o modelo proposto para estimar... Admito que gostei da macro divisão sugerida: Capacidade X Atitude. Também acho que faz sentido que os fatores conectados com Capacidade influenciam mais do que com atitude. Vou explicar pq? A teoria da autoeficácia diz que quando você se sente capaz, você tende a performar melhor dentro daquele contexto. Eu vi isso regularmente na minha vida. A lista de itens dentro de Capacidade falam muito com o quão a pessoa ou equipe seria capaz de manter o software. 

* Pelo olhar da capacidade em cima da perspectiva do software em si, para mim tudo faz bastante sentido. 
   * A baseline size não medida apenas pelo olhar de linhas de código, é muito interessante. Considerando o mundo de hoje, faz você olhar de maneira mais crítica para o debate entre microservices x serverless x monolitos etc... Ele traz a quantidade de coisas no software, desde linhas de código, tabelas, scripts e porque não considerar softwares de observabilidade, definições de contratos de integração, etc. 

  * Complexidade -> Pelo menos, em 2020, eu ainda aposto que  certas métricas de complexidade combinadas falam sobre a possível dificuldade manutenção. 

  * Manutenibilidade -> Aqui existe uma divisão entre a complexidade e manuntenibilidade. Esse requisito é mais olhado pelo viés de tempo de recuperação. Também acho que faz sentido, se o tempo for olhado por múltiplas dimensões. 

 * Histórico de erros -> Parece que faz sentido que um software com registro de muitos erros ser analisado como mais complicado de ser mantido. 

 * Documentão -> também continuo apostando que uma boa documentação, cuidada como cidadã de primeiro nível, colabora como facilitadora para manutenção do software. 

* Pelo olhar da capacidade em cima do time de manutenção

 * Technical Expertise –> Aposto totalmente que o aumento da expertise técnica diminui o esforço necessário para a atividade de manutenção. Uma das dimensões de avaliação de performance que uso é qualidade pelo olhar de esforço necessário para realizar uma atividade. Quanto melhor você é, menos esforço vai ter para fazer algo. 
 
 * Domain Expertise -> Outra coisa óbvia para mim... Se vocÊ não entende do negócio, a chance é alta de ter mais dificuldade para fazer tudo. 

 * Application Knowledge -> Óbvio também para mim. Você precisa ter uma estratégia para gradativamente entender o código que você está mexendo. Isso vai facilitar sua análise em cima de falhas, prever situações etc. 

**Fechando**

* Considero o texto bem interessante e vejo conexão com a realidade. Volto a ressaltar que esse é foco do canal e jornada esse ano... Promover a ideia que o desafio atual é muito mais achar boas formas de manter o software num estado que facilite correção e evolução do que necessariamente ficar achando forma de colocar código no ar e gerar valor para o negócio. Acho que esse último já está de certa forma bem avançado... 

**Fonte**

https://www.rose-hulman.edu/class/csse/OldFiles/csse575/Resources/OutsourcedMaint-p3-bhatt.pdf

