# Motivo de todo o texto

É importante termos em mente que o melhor código é aquele que não precisa ser escrito. Nas situações que não conseguimos nos livras de criar um sistema, o segundo ponto mais importante é
que tal sistema funcione da maneira mais simples possível. 

O quero dizer com simples? Que faça o que precisa fazer, respeitando todas as necessidades, restrições e através de um código que exija o mínimo de esforço mental de quem vai precisar entender, manter e evoluir o bendito do sistema.

## E como eu sei que um código está mais fácil de entender, manter e evoluir? 

Para isso eu uso como base a Teoria da Carga Cognitiva. Esta teoria tenta descrever como que nós, seres humanos, entendemos as coisas. 

Sem entrar em muitos detalhes(referência lá embaixo), a teoria diz que usamos dois tipos de memória durante nosso processo de aprendizado e aplicação de tal conhecimento. A memória de trabalho e a memória de longo prazo. Enquanto a memória de longo prazo tem um armazenamento considerado infinito, a memória de trabalho é super limitada. Imagine que quando você está no ápice da sua potência cognitiva, essa memória de trabalho suporta, geralmente, sete coisas distintas. 

Caso queira, ande sempre com um baldinho e com um estojo de capacidade máxima de 7 canetas. Durante a análise de um código, leitura de um livro, post de blog ou qualquer outra coisa, coloque uma caneta para cada item que você considera que está exigindo esforço da sua memória de trabalho.

## E quais são essas coisas que exigem esforço da minha memória de trabalho?

Essa também é uma boa pergunta. Tudo envolvido naquele exato momento do consumo de informação e tentativa de entendimento de algo. Alguns exemplos:

* Tem alguma distração rolando? Só de você perceber a distração, quer dizer que sua memória de trabalho está sendo utilizada. Pense em músicas, conversas paralelas etc. 
* Tem alguma classe sendo utilizada no código e você não conhece? Pimba, está ocupando.
* Tem algum método que está sendo utilizado que você não conhece? Pimba, está ocupando. 
* Tem algum fluxo de código que você bateu o olho e não ficou claro? Pimba, está ocupando. 
* Tem algum conhecimento anterior que você precisa ter para entender aquilo e você não tem?

Para cada exemplo acima você pode adicionar uma caneta no seu baldinho. 

## Finalmente, como eu avalio isso dentro do meu código?

Você precisa ter um caminho para distribuir a carga cognitiva pelo seu código. Isso é o que, na minha opinião, deveria guiar tudo. E, se você parar para pensar, é isso que os livros tentam fazer não é? Alguns exemplos:

* O DDD sugere você utiliar a tal linguagem ubíqua. Por que será que todos devem falar a mesma lingua dentro de um contexto de negócio?
* O mesmo DDD sugere diversos padrões que inclusive são bem aceitos e utilizados por toda a industria. Por que será que todo mundo usa Service?
* Por que você quer que todo mundo use os padrões sugeridos pelo CleanCode?
* Por que colocamos no nome das classes um sufixo referenciando o padrão que inspirou a criação dela?

Olhando pelo viés especificamente do código, **acho que a melhor pergunta é**: por que você não cria uma classe de dez mil linhas em vez de criar cem classes de cem linhas cada? Qual é a lógica por trás que te faz achar que ter cem arquivos é mais fácil do que ter um arquivo? Provavelmente é porque você acha que assim será mais fácil. Por sinal você já deve ter ouvido esse argumento em equipes que utilizam arquitetura distribuída, através de microservices por exemplo. E aí volta a pergunta: como avaliar a complexidade de um código? 

Como expliquei no começo do texto, eu sugiro avaliar usando como base a Teoria da Carga Cognitiva. A complexidade de um material(no nosso caso estamos falando de código) é medida pelo que chamamos de carga intrínseca do material. Em abril de 2020, o que eu entendo que aumenta a carga intrínseca de um código é o seguinte:

* Classes necessárias para habilitar a execução de um determinado ponto do nosso código. Aqui eu só levo em consideração classes que foram criadas pelo próprio sistema. Classes do runtime da linguagem, frameworks etc devem ser conhecidas a priori. Não sabe? Precisa estudar. 
* Número de branches no código. Ifs, loops, ifs e/ou loops aninhados. 


### Conclusão

Neste texto eu descrevi a forma como eu escrevo e avalio código atualmente. Considero que tenho tido bons resultados e sinto que é um caminho que vou continuar a explorar nos próximos tempos. 

Que fique claro, eu respeito e li integral ou parcialmente diversos dos livros sobre arquitetura, design e práticas de código, assim como algumas publicações científicas sobre o tema. Todas elas me inspiraram mas, a partir de um ponto, eu comecei a fazer alguns dos seguintes questionamentos: 

* Como aquele Eric Evans chegou no diabo dos padrões sugeridos no DDD(https://domainlanguage.com/ddd/reference/)?
* Como que Fowler chegou no livro de Refactoring(https://martinfowler.com/books/refactoring.html)? 
* Como Chidamber & Kemerer chegaram no seu conjunto de métricas(https://www.aivosto.com/project/help/pm-oo-ck.html)? 
* Como que a métrica de coesão foi questionada(https://www.aivosto.com/project/help/pm-oo-cohesion.html#LCOM1)?
* Como que a super incrível Barbara Liskov chegou na ideia dos tipos abstratos de dados(https://dl.acm.org/doi/pdf/10.1145/942572.807045)?

Confesso que não fui atrás de todas as referências utilizadas por trás de cada sugestão dada por essas pessoas que são muito mais inteligentes que eu. Por outro lado, eu também não vejo nenhum livro famoso entre a comunidade de desenvolvimento que trate da complexidade envolvida sobre a criação de um sistema através da ótica da teoria da carga cognitiva(por mais que vários falem que é necessário diminuir o fator cognitivo do código). 

### Um pouco mais sobre mim

Você pode me achar em alguns lugares, vou deixar caso se interesse pelo meu trabalho:

* Através do meu projeto pessoal chamado Jornada do(a) Dev eficiente - https://deveficiente.com/
* Github - https://github.com/asouza
* Twitter - https://twitter.com/alberto_souza
