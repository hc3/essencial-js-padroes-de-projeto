# Categorias de Padrão de Projeto

Um glossário do livro conhecido de projetos, *Domain-Driven Terms*, afirma com razão que:

> Um padrão de projeto nomeia, abstra e identifica os aspectos de uma comum estrutura de projeto que faz isto útil para criação e reutilização de projetos orientados a objeto. O padrão de projeto identifica as classes participantes e suas instâncias, suas funções e colaborações, e a distribuição de responsabilidades.

> Cada padrão de projeto foca em um problema ou questão particular de projetos orientados a objetos. Ele descreve quando se aplica, ou não pode ser aplicado em vista de outras restrições de projeto, e as consequências e os compromissos de se usá-lo. Desde que nós eventualmente implementemos nossos padrões, um padrão de projeto também fornece código de exemplo para ilustrar uma implementação.

> Embora padrões de projeto descrevam padrões orientado a objetos, eles são baseados em soluções práticas que devem ser implementadas em linguagens de programação orientada a objetos tradicionais..."

Padrões de projeto pode ser divididos em um número de diferentes categorias. Nesta seção vamos revisar 3 destas categorias e mencionar brevemente alguns exemplos de padrões que caem nestas categorias antes de explorar especificamente alguns com maiores detalhes.

## Padrões de projetos Criacionais

Padrões de projetos Criacionais focam em lidar com os mecanismos de criação de objetos onde os objetos são criados de maneira adequada para a situação que estamos trabalhando. A abordagem básica para criação de objeto pode legar a adicionar complexidade ao projeto, enquanto esses padrões têm como objetivo resolver problemas através do controle do processo de criação.

Alguns padrões que se encaixam nesta categoria são: Constructor, Factory, Abstract, Prototype, Singleton e Builder.

## Padrões de Projetos Estruturais

Padrões de projetos estruturais estão preocupados com composição de objetos e, normalmente, identificar maneiras simples de realizar as relações entre diferentes objetos. Eles ajudam a assegurar que quando uma parte do sistema muda, toda a estrutura do sistema não precisa fazer o mesmo. Eles também auxiliam na reformulação de partes do sistema que não encaixam em uma proposta particular dentro do que eles fazem.

Padrões que entram nesta categoria: Decorator, Facade, Flyweight, Adapter e Proxy.

## Padrões de projetos Comportamentais

Padrões de projetos comportamentais focam em melhorar ou simplificar a comunicação entre os objetos díspares em um sistema.

Alguns padrões comportamentais são: Iterator, Mediator, Observer e Visitor.
