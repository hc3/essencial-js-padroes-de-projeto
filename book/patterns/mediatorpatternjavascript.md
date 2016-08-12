# The Mediator Pattern

Na sessão do padrão observer, nos fomos introduzidos a uma meneira de ligar multiplus eventos a um único objeto. Isso é também conhecido como publish/subscribe ou event aggregation. é comun para os desenvolvedores pensar nos Mediators quando vemos esse problema, então vamos explorar as diferenças entre eles.

O dicionário refere-se a um mediador como um partido neutro que auxilia nas negociações e resolução de conflitos. No nosso mundo , um mediador é um padrão de design comportamental que nos permite expor uma interface unificada através do qual as diferentes partes de um sistema pode comunicar.

Se ficar constatado que um sistema tem muitas relações diretas entre seus componentes, pode ser a hora de criar um ponto central de controle que os componentes se comuniquem através de um ponto. O mediator promove o acoplamento fraco, garantindo que em vez de componentes fazendo referência um ao outro explicitamente, sua interação seja feita através deste ponto central. Isso pode nos ajudar a dissociar os sistemas e melhorar o potencial de reutilização de componentes.

Trazendo para o mundo real podemos fazer uma analogia ao sistema de tráfego áreo. a torre ( mediator ) sabe qual avião deve sair e pousar isso porque toda a comunicação tem que passar pela torre e com isso temos todo o controle áreo centralizado.

Quando estamos com o padrão Mediator e Event aggregator, existem momentos em que a semelhaça entre esses dois padrões pode parecer que são apenas mais do mesmo, mas na prática não é bem assim a semântica e a interação desses padrões são muito diferentes.

E mesmo que a implementação de ambos use construções bem parecidas existe uma diferença nítida entre eles, eles não devem ser misturados.

<b> Um simples mediator </b>


Um mediador é um objeto que coordena as interações (lógica e comportamento ) entre vários objetos. Faz decisões sobre quando chamar quais objetos, com base nas ações (ou omissão ) de outros objetos e entrada.

Podemos criar um mediator usando uma única linha de código:

````js
var mediator = {};
````

Sim, é claro que isso é apenas um objeto literal em JavaScript. Mais uma vez , estamos falando sobre a semântica aqui.
O objetivo do mediador é o de controlar o fluxo de trabalho entre os objetos e nós realmente não precisamos de nada mais do que um objeto literal.

````js
var orgChart = {

  addNewEmployee: function(){

    // getEmployeeDetail provides a view that users interact with
    var employeeDetail = this.getEmployeeDetail();

    // when the employee detail is complete, the mediator (the 'orgchart' object)
    // decides what should happen next
    employeeDetail.on("complete", function(employee){

      // set up additional objects that have additional events, which are used
      // by the mediator to do additional things
      var managerSelector = this.selectManager(employee);
      managerSelector.on("save", function(employee){
        employee.save();
      });

    });
  },

  // ...
}
````

Esse exemplo mostra uma implementação basica do padrão mediator, o objeto com uma única utilidade, métodos que podem incluir ou remover eventos.

Eu sempre me refiria a este tipo de objeto como um objeto "workflow", mas a verdade é que é um mediador.
É um objeto que manipula o fluxo de trabalho entre muitos outros objetos , agregando a responsabilidade de que o conhecimento do fluxo de trabalho em um único objeto. O resultado é fluxo de trabalho que é mais fácil de entender e manter.
