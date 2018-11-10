<h1 align = "center">Anti-Padrões</h1>


<p align = "justify">Se considerarmos que um padrão representa uma prática recomendada, um antipadrão representa uma lição que foi aprendida. O termo anti-padrões foi assinado em 1995 por Andrew Koenig no Relatório C++ de novembro daquele ano, inspirado no livro <b>Padrões de Design do GoF (Gang of Four)</b>. No relatório de Koenig, existem duas noções de anti-padrões que são apresentadas. Anti-Padrões:</p>

- Descreve uma **má** solução para um problema em particular que pode levar a uma situação chata.

- Descreve **como** sair da situação citada e como ir dali para uma boa solução.

<p align = "justify">Sobre este tópico, Alexander escreve sobre as dificuldades de alcançar um bom equilíbrio entre boa estrutura de design e bom contexto:</p>

> “Essas notas são sobre o processo de design; o processo de inventar coisas materiais que exibem uma nova ordem concreta, organização, forma, em resposta à função ... todo problema de design começa com um esforço para alcançar uma harmonia entre duas entidades: a forma em questão e seu contexto. A forma é a solução para o problema; o contexto define o problema ”.

<p align = "justify">Embora seja muito importante estar ciente dos padrões de design, pode ser igualmente importante entender os anti-padrões. Vamos analisar a razão por trás disso. Ao criar um aplicativo, o ciclo de vida de um projeto começa com a construção. No entanto, depois que você faz o lançamento inicial, ele precisa ser mantido. A qualidade de uma solução final será <b>boa</b> ou <b>ruim</b>, dependendo do nível de habilidade e tempo que a equipe investiu nela. Aqui, <b>bom</b> e <b>ruim</b> são considerados no contexto - um design "perfeito" pode se qualificar como um anti-padrão se aplicado no contexto errado.</p>

<p align = "justify">Os maiores desafios acontecem depois que um aplicativo atinge a produção e está pronto para entrar no modo de manutenção. Um desenvolvedor trabalhando em tal sistema que não tenha trabalhado no aplicativo antes, pode introduzir um projeto <b>ruim</b> por acidente. Se as <b>más</b> práticas forem criadas como anti-padrões, elas permitirão aos desenvolvedores um meio de reconhecê-las com antecedência, para que possam evitar erros comuns que venham a ocorrer - isso é paralelo à maneira como os padrões de projeto nos fornecem um modo de reconhecer técnicas comuns que são <b>úteis</b>.</p>

<p align = "justify">Resumindo, um anti-padrão é um design que é ruim, mas que vale a pena ser documentado. Os exemplos de anti-padrões em JavaScript são os seguintes:</p>

- Poluindo o namespace definindo um grande número de variáveis no contexto global

- Passando strings ao invés de funções para setTimeout ou setInterval, isso aciona o uso de ```eval()``` internamente.

- Modificando o protótipo da classe ```Object``` (este é um anti-padrão particularmente ruim)

- Usando JavaScript em uma forma inline, o que o torna inflexível

- O uso de document.write onde alternativas do próprio DOM, como document.createElement, são mais apropriadas. document.write tem sido muito usado ao longo dos anos e tem algumas desvantagens, como exemplo se ele for executado depois que a página carregou, pode acabar sobrescrevendo a página em que estamos, enquanto document.createElement não. <a href = "http://jsfiddle.net/addyosmani/6T9vX/">Aqui</a> podemos ver um exemplo disso em ação. Ele também não funciona com XHTML, que é outra razão para optar por métodos mais amigáveis ao DOM, como document.createElement que é favorável.

<p align = "justify">O conhecimento de anti-padrões é essencial para o sucesso. Uma vez que somos capazes de reconhecer tais anti-padrões, somos capazes de refatorar nosso código para evitá-los, para que a qualidade geral de nossas soluções melhore instantaneamente.</p>

<p align = "right"><b><a href = "https://github.com/ranielcsar/essencial-js-padroes-de-projeto/blob/master/book/categoriesofdesignpatterns.md">Categorias de Design Pattern »</a></b></p>
