# Skills revisoras

Skills de revisão de código no formato [Agent Skills](https://agentskills.io/specification). Cada pasta tem um `SKILL.md` que um agente (Claude Code e compatíveis) carrega quando a tarefa pede aquele tipo de revisão.

| Skill | O que revisa | Linguagens |
|---|---|---|
| [`revisora-cdd`](revisora-cdd/SKILL.md) | Complexidade cognitiva por unidade, sob a ótica de Cognitive-Driven Development (CDD): conta os ICP (Intrinsic Complexity Points) e sinaliza as unidades acima do limite combinado pelo time. | Qualquer uma |
| [`revisora-coesao`](revisora-coesao/SKILL.md) | Coesão e encapsulamento: Tell Don't Ask, Lei de Demeter e inveja de funcionalidade. | Qualquer uma |
| [`revisora-testes`](revisora-testes/SKILL.md) | Práticas de testes automatizados: objetos reais, dublês restritos às fronteiras, sem matchers coringa, testes de fronteira e MC/DC. | Qualquer uma |
| [`revisora-testes-java`](revisora-testes-java/SKILL.md) | As mesmas regras da `revisora-testes`, aplicadas a JUnit 5, Mockito e Spring. | Java |

Cada skill olha um aspecto só, e elas se referenciam pelo nome. Os relatórios usam o campo `Categoria` (CDD, Coesão, Testes) para poderem ser combinados numa revisão única.

## Instalação

Copie a pasta da skill para o diretório de skills do seu agente. No Claude Code:

```bash
# para todos os seus projetos
cp -r revisora-cdd ~/.claude/skills/

# ou só para um projeto
cp -r revisora-cdd <projeto>/.claude/skills/
```

Mantenha o nome da pasta igual ao `name` do frontmatter.

## `revisora-cdd` precisa de um `cdd.json`

A skill não decide sozinha o que conta como ponto de complexidade: ela lê as regras de um `cdd.json` no projeto revisado. Sem esse arquivo ela não conta nada.

Para começar, copie [`revisora-cdd/exemplos/cdd.json`](revisora-cdd/exemplos/cdd.json) para a raiz do projeto. Ele é independente de linguagem e segue a tabela de ICP do artigo que definiu o CDD. Na mesma pasta há modelos escritos para Java, Python, TypeScript e Go. O formato está descrito em [`revisora-cdd/cdd.schema.json`](revisora-cdd/cdd.schema.json).

## Referências sobre CDD

- Souza, A. L. O. T.; Pinto, V. H. S. C. *Toward a Definition of Cognitive-Driven Development*. ICSME 2020. [PDF](https://github.com/asouza/pilares-design-codigo/blob/master/ICSME-2020-cognitive-driven-development.pdf)
- Pereira, J. H. A.; Souza, A. L. O. T.; Pinto, V. H. S. C. *Cognitive Load Analyzer: A Support Tool for Cognitive-Driven Development*. SBES 2021. [PDF](https://github.com/asouza/pilares-design-codigo/blob/master/Cognitive-Load-Analyzer-A-Support-Tool-for-Cognitive-Driven-Development.pdf)
- [Linha do tempo da pesquisa sobre CDD](https://github.com/asouza/pilares-design-codigo/tree/master/timeline-cdd)
