# Anti-Padrões

Se nós considerarmos que um padrão representa uma boa prática, um anti-padrão representa uma lição que foi aprendida. O termo anti-padrão foi cunhado em 1995 por Andrew Koenig no *November C++ Report* daquele ano, inspirado pelo livro GoF's *Design Patterns*. No artigo do Koenig, temos duas notações de anti-padrões representadas. Anti-padrões:

* Descrevem uma má solução para um problema particular que resulta em uma situação ruim ocorrida
* Descrevem como sair da situação citada e como ir para uma boa solução

Neste tópico, Alexander escreve sobre as dificuldades de alcançar um bom balanço entre uma boa estrutura de projeto e um bom contexto:

> Estas notas são sobre os processos de projeto; o processo de inventar coisas físicas que mostram uma nova ordem física, organização, forma, em resposta à função... todo problema do projeto começa com um esforço para conseguir a aptidão entre duas entidades: a forma em questão e o contexto. A forma é a solução do problema; o contexto define o problema.

Embora seja muit importante estar ciente de padrões de projeto, pode ser igualmente importante entender de anti-padrões. Vamos qualificar a razão por trás disso. Quando criamos uma aplicação, um ciclo de vida de um projeto começa com a construção, no entanto uma vez que você tem a versão inicial feita, ela precisa ser mantida. A qualidade de uma solução final vai ser boa ou ruim, dependendo do nível de habilidade e o tempo que o time investiu nesta. Aqui *bom* e *ruim* são considerados no contexto - um projeto "perfeito" deve qualificar como um anti-padrão se aplicado no contexto errado.

Os maiores desafios acontecem depois que uma aplicação atingiu a produção e está pronta para entrar em modo de manutenção. Um desenvolvedor trabalhando em um sistema desse tipo, que não trabalhou no aplicativo antes pode introduzir uma solução ruim para este projeto por acidente. Se *más* práticas forem criadas como anti-padrões, elas permitem aos desenvolvedores uma maneira de reconhecê-las com antecedência, para que possam evitar os erros mais comuns que possam aparecer - está uma forma paralela na qual os padrões de projeto nos fornecem um meio para reconhecer técnicas comuns que são úteis.

Para resumir, um anti-padrão é um projeto ruim que é válido para documentação. Exemplos de anti-padrões em JavaScript são os seguintes:

* Poluir o namespace global definindo um grande número de variáveis no contexto global.
* Passar strings em vez de funções, quer seja com setTimeout ou setInterval, desencadeando o uso de `eval()` internamente.
* Modificar a classe prototípica `Object` (isto é particularmente um ruim anti-padrão).
* Usar JavaScript de forma *inline* pois está é inflexível.
* O uso de `document.write` onde temos alternativas DOM nativas como `document.createElement` mais apropriadas. `document.write` tem sido grosseiramente mal utilizado ao longo dos anos e tem algumas desvantagens incluindo que se ele é executado depois que a página foi carregada, ele pode realmente subtituir a página que está, enquanto `document.createElement` não. Nós podemos ver [aqui](http://jsfiddle.net/addyosmani/6T9vX/) para um exemplo real desta ação. Ele também não funciona com XHTML que é outra razão de optar por métodos mais amigáveis ao DOM como `document.createElement` que é favorável.

Conhecer os anti-padrões é crítico para o sucesso. Uma vez que estivermos aptos a reconhecê-los, estaremos aptos a refatorar nosso código para negá-los, de modo que a qualidade global das nossas soluções melhorarão instantaneamente.
