<h1 align = "center">Escrevendo Padrões de Projeto</h1>


<p align = "justify">Apesar desse livro ser focado nos novatos em padrões de projeto, um entendimento fundamental de como um padrão de design é escrito pode trazer vários benefícios uteis. Para começar, podemos obter uma avaliação mais profunda da lógica por trás do motivo de um padrão ser necessário. Também podemos aprender quando um padrão (ou proto-padrão) está pronto para ser revisado para nossas necessidades.</p>

<p align = "justify">Escrever bons padrões é uma tarefa desafiadora. Os padrões não só precisam (idealmente) fornecer uma quantidade considerável de material de referência para os usuários finais, mas também precisam ser capazes de defender por que eles são necessários.</p>

<p align = "justify">Tendo lido a seção anterior sobre o que é um padrão, podemos pensar que isso em si é suficiente para nos ajudar a identificar padrões que vemos por aí. Isso não é totalmente uma verdade. Nem sempre fica claro se um pedaço de código está seguindo um padrão definido ou se só parece que está.</p>

<p align = "justify">Quando olhamos para um corpo de código que achamos estar usando um padrão, devemos considerar escrever alguns dos aspectos do código que acreditamos que pertença a um padrão ou a um conjunto de padrões específico existente.</p>

<p align = "justify">Em muitos casos da análise de padrões, podemos descobrir que estamos olhando apenas para um código que segue bons princípios e práticas que coincide com as regras de um padrão por acidente. Lembre-se: soluções nas quais nem as interações nem as regras definidas aparecem <b>não</b> são padrões.</p>

<p align = "justify">Se estiver interessado em se aventurar no caminho de escrever seus próprios padrões de design, recomendo aprender com outras pessoas que já passaram pelo processo e o fizeram bem. Passe o tempo absorvendo as informações de um número de diferentes descrições de padrões de design e absorva o que é significativo para você.</p>

<p align = "justify">Explore a estrutura e a semântica - isso pode ser feito verificando as interações e o contexto dos padrões nos quais você está interessado, para que possa identificar os princípios que auxiliam na organização desses padrões juntos em configurações úteis.</p>

<p align = "justify">Uma vez que nos expomos a uma riqueza de informações sobre literatura de padrões, podemos começar a escrever nosso padrão usando um formato existente e ver se podemos debater novas ideias para melhorá-lo ou integrar nossas ideias nele.</p>

<p align = "justify">Um exemplo de desenvolvedor que fez isso nos últimos anos é Christian Heilmann, que pegou o padrão <b>Module</b> existente e fez algumas mudanças fundamentais para criar o padrão <b>Revealing Module</b> (este é um dos padrões abordados mais adiante neste livro).</p>

<p align = "justify">Gostaria de sugerir as seguintes dicas, caso esteja interessado em criar um novo padrão de design:</p>

- **Quão prático é o padrão?**: Garanta que o padrão descreva soluções comprovadas para problemas recorrentes, em vez de apenas soluções especulativas que não foram qualificadas.

- **Mantenha as melhores práticas em mente**: As decisões que tomamos para o design devem basear-se em princípios que derivam de uma compreensão das melhores práticas.

- **Nosso padrão de projeto deve ser transparente para o usuário**: Os padrões de projeto devem ser totalmente transparentes para qualquer tipo de experiência do usuário. Em primeiro lugar eles estão lá para servir os desenvolvedores que os usam e não devem forçar mudanças de comportamento na experiência do usuário que não seriam comprometidas sem o uso de um padrão.

- **Lembre-se que originalidade não é fundamental no design de padrões**: Ao escrever um padrão, não precisamos ser o descobridor original das soluções que estão sendo documentadas, nem precisamos nos preocupar com o design que se sobrepõe a pequenas partes de outros padrões. Se a abordagem for forte o suficiente para ter ampla aplicabilidade útil, ela terá a chance de ser reconhecida como um padrão válido.

- **Padrões precisam de um forte conjunto de exemplos**: Uma boa descrição precisa ser seguida por um conjunto igualmente forte de exemplos que demonstram a aplicação bem-sucedida de nosso padrão. Para mostrar o amplo uso, exemplos que demonstram bons princípios de padrão são ideais.

<p align = "justify">A escrita de padrões é um cuidadoso processo entre a criação de um design que é universal, específico e, acima de tudo, útil. Tente garantir que, quando você escrever um padrão, cubra as áreas de aplicação mais amplas possíveis e estará bem. Espero que esta breve introdução aos padrões de escrita tenha lhe dado algumas noções que ajudarão no seu processo de aprendizado nas próximas seções deste livro.</p>

<p align = "right"><b><a href="https://github.com/ranielcsar/essencial-js-padroes-de-projeto/blob/master/book/antipatterns.md">Anti-Padrões »</b></p>
