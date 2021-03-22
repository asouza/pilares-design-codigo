## Precisamos fazer código que dure mais que o próximo release

Aqui tem a minha sugestão de práticas que devem ser seguidas por equipes de desenvolvimento para que o código consiga evoluir na velocidade desejada pelo negócio
mantendo um excelente nível de entendimento e manutenibildiade. 

Erosão arquitetural, esforço exagerado de manutenção, bugs e atraso no ritrmo de geração de valor são alguns dos fatores que, na minha opinião, acontecem por conta
da falta de qualidade do código. 

## Qualidade não é negociável

## Limite de complexidade

Toda equipe deve definir uma forma clara de controlar a complexidade de código. Isso quer dizer que qualquer pessoa da equipe, pouco importa o nível de 
expertise, precisa ser capaz de olhar para uma unidade de código(arquivo,método,classe) e falar se aquilo está acima ou abaixo do limite definido pela equipe. 

Para ser sincero, não enxergo neste momento que existe a melhor maneira de definir o limite, apenas que devemos ter um limite claro. Também precisa ser 
clara a forma calcular o valor atual, para que se possa fazer a comparação contra o limite. 

## E como eu poderia definir um limite?

### Inspirado nas métricas formais

Vou deixar duas opções e a que prefiro. A primeira é utilizar o histórico da empresa em cima de um conjunto de métricas formais de software e estabelecer o limiar. 
Você achar valores médios de linhas de código, complexidade ciclomática, coesão, acoplamento etc e definir que nenhuma unidade de código pode determinados valores
para cada uma das métricas. 

Inclusive você pode fazer por tipo de unidade. Classes que representam endpoints tem limite X-Y-Z, classes que fazem parte exclusivamente do domínio tem limite
A-B-C e por aí vai.

Perceba que você não está dizendo para as pessoas como elas devem programar ou tirando a criatividade delas. Você está colocando limites claro na "criatividade". Deixando
esse limite claro, você abre margem para um real processo de melhoria contínua. A cada x dias a equipe se reune para analisar o código produzido, argumentar e, 
talvez, definir novos limites. Mais relaxados ou mais restrititivos. 

### Inspirado no Cognitive Driven Development

## O limite de claro complxedidade possibilita a refatoração direcionada

## Análise de causa raiz para cada funcionalidade



