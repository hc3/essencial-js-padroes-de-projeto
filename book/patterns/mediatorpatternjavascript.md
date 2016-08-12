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

<b> Similaridades e diferenças </b>

Há , sem dúvida, semelhanças entre o agregador evento e o mediador que eu mostrei aqui. As semelhanças se resumem a dois itens principais :
eventos e objetos de terceiros. Estas diferenças são superficiais na melhor das hipóteses , no entanto. Quando se mergulha na intenção do padrão e
vemos que as implementações podem ser dramaticamente diferentes, a natureza dos padrões tornam-se mais evidente.

<b>Events</b>

Both the event aggregator and mediator use events, in the above examples. An event aggregator obviously deals with events – it’s in the name after all. The mediator only uses events because it makes life easy when dealing with modern JavaScript webapp frameworks. There is nothing that says a mediator must be built with events. You can build a mediator with callback methods, by handing the mediator reference to the child object, or by any of a number of other means.

The difference, then, is why these two patterns are both using events. The event aggregator, as a pattern, is designed to deal with events. The mediator, though, only uses them because it’s convenient.

<b>Third-Party Objects</b>

Both the event aggregator and mediator, by design, use a third-party object to facilitate things. The event aggregator itself is a third-party to the event publisher and the event subscriber. It acts as a central hub for events to pass through. The mediator is also a third party to other objects, though. So where is the difference? Why don’t we call an event aggregator a mediator? The answer largely comes down to where the application logic and workflow is coded.

In the case of an event aggregator, the third party object is there only to facilitate the pass-through of events from an unknown number of sources to an unknown number of handlers. All workflow and business logic that needs to be kicked off is put directly into the object that triggers the events and the objects that handle the events.

In the case of the mediator, though, the business logic and workflow is aggregated into the mediator itself. The mediator decides when an object should have its methods called and attributes updated based on factors that the mediator knows about. It encapsulates the workflow and process, coordinating multiple objects to produce the desired system behaviour. The individual objects involved in this workflow each know how to perform their own task. But it’s the mediator that tells the objects when to perform the tasks by making decisions at a higher level than the individual objects.

An event aggregator facilitates a “fire and forget” model of communication. The object triggering the event doesn’t care if there are any subscribers. It just fires the event and moves on. A mediator, though, might use events to make decisions, but it is definitely not “fire and forget”. A mediator pays attention to a known set of input or activities so that it can facilitate and coordinate additional behavior with a known set of actors (objects).

<b>Relationships: When To Use Which</b>

Understanding the similarities and differences between an event aggregator and mediator is important for semantic reasons. It’s equally as important to understand when to use which pattern, though. The basic semantics and intent of the patterns does inform the question of when, but actual experience in using the patterns will help you understand the more subtle points and nuanced decisions that have to be made.

<b>Event Aggregator Use</b>

In general, an event aggregator is used when you either have too many objects to listen to directly, or you have objects that are entirely unrelated.

When two objects have a direct relationship already – say, a parent view and child view – then there might be little benefit in using an event aggregator. Have the child view trigger an event and the parent view can handle the event. In JavaScript framework terms, this is most commonly seen in Backbone’s Collection and Model, where all Model events are bubbled up to and through its parent Collection. A Collection often uses model events to modify the state of itself or other models. Handling “selected” items in a collection is a good example of this.

jQuery’s on method as an event aggregator is a great example of too many objects to listen to. If you have 10, 20 or 200 DOM elements that can trigger a “click” event, it might be a bad idea to set up a listener on all of them individually. This could quickly deteriorate performance of the application and user experience. Instead, using jQuery’s on method allows us to aggregate all of the events and reduce the overhead of 10, 20, or 200 event handlers down to 1.

Indirect relationships are also a great time to use event aggregators. In modern applications, it is very common to have multiple view objects that need to communicate, but have no direct relationship. For example, a menu system might have a view that handles the menu item clicks. But we don’t want the menu to be directly tied to the content views that show all of the details and information when a menu item is clicked. Having the content and menu coupled together would make the code very difficult to maintain, in the long run. Instead, we can use an event aggregator to trigger “menu:click:foo” events, and have a “foo” object handle the click event to show its content on the screen.

<b>Mediator Use</b>

A mediator is best applied when two or more objects have an indirect working relationship, and business logic or workflow needs to dictate the interactions and coordination of these objects.

A wizard interface is a good example of this, as shown with the “orgChart” example, above. There are multiple views that facilitate the entire workflow of the wizard. Rather than tightly coupling the view together by having them reference each other directly, we can decouple them and more explicitly model the workflow between them by introducing a mediator.

The mediator extracts the workflow from the implementation details and creates a more natural abstraction at a higher level, showing us at a much faster glance what that workflow is. We no longer have to dig into the details of each view in the workflow, to see what the workflow actually is.

<b>Event Aggregator (Pub/Sub) And Mediator Together</b>

The crux of the difference between an event aggregator and a mediator, and why these pattern names should not be interchanged with each other, is illustrated best by showing how they can be used together. The menu example for an event aggregator is the perfect place to introduce a mediator as well.

Clicking a menu item may trigger a series of changes throughout an application. Some of these changes will be independent of others, and using an event aggregator for this makes sense. Some of these changes may be internally related to each other, though, and may use a mediator to enact those changes.

A mediator, then, could be set up to listen to the event aggregator. It could run its logic and process to facilitate and coordinate many objects that are related to each other, but unrelated to the original event source.

````js
var MenuItem = MyFrameworkView.extend({

  events: {
    "click .thatThing": "clickedIt"
  },

  clickedIt: function(e){
    e.preventDefault();

    // assume this triggers "menu:click:foo"
    MyFramework.trigger("menu:click:" + this.model.get("name"));
  }

});

// ... somewhere else in the app

var MyWorkflow = function(){
  MyFramework.on("menu:click:foo", this.doStuff, this);
};

MyWorkflow.prototype.doStuff = function(){
  // instantiate multiple objects here.
  // set up event handlers for those objects.
  // coordinate all of the objects into a meaningful workflow.
};
````

In this example, when the MenuItem with the right model is clicked, the “menu:click:foo” event will be triggered. An instance of the “MyWorkflow” object, assuming one is already instantiated, will handle this specific event and will coordinate all of the objects that it knows about, to create the desired user experience and workflow.

An event aggregator and a mediator have been combined to create a much more meaningful experience in both the code and the application itself. We now have a clean separation between the menu and the workflow through an event aggregator and we are still keeping the workflow itself clean and maintainable through the use of a mediator.

#### Advantages & Disadvantages

The largest benefit of the Mediator pattern is that it reduces the communication channels needed between objects or components in a system from many to many to just many to one. Adding new publishers and subscribers is relatively easy due to the level of decoupling present.

Perhaps the biggest downside of using the pattern is that it can introduce a single point of failure. Placing a Mediator between modules can also cause a performance hit as they are always communicating indirectly. Because of the nature of loose coupling, it's difficult to establish how a system might react by only looking at the broadcasts.

That said, it's useful to remind ourselves that decoupled systems have a number of other benefits - if our modules communicated with each other directly, changes to modules (e.g another module throwing an exception) could easily have a domino effect on the rest of our application. This problem is less of a concern with decoupled systems.

At the end of the day, tight coupling causes all kinds of headaches and this is just another alternative solution, but one which can work very well if implemented correctly.

#### Mediator Vs. Facade

We will be covering the Facade pattern shortly, but for reference purposes some developers may also wonder whether there are similarities between the Mediator and Facade patterns. They do both abstract the functionality of existing modules, but there are some subtle differences.

The Mediator centralizes communication between modules where it's explicitly referenced by these modules. In a sense this is multidirectional. The Facade however just defines a simpler interface to a module or system but doesn't add any additional functionality. Other modules in the system aren't directly aware of the concept of a facade and could be considered unidirectional.
