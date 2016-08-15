#The Command Pattern

O padrão Command pretende encapsular invocação de métodos, requests ou operações em um único objeto e nos dá a capacidade de parametrizar e passar métodos chamados em torno de que podem ser executados a nosso critério.
Além disso, permite-nos separar objetos invocando a ação dos objetos que as implementam, dando-nos um maior grau de flexibilidade global em trocar classes concretas (objetos).

Classes concretas são melhor explicadas em termos de linguagens de programação baseadas em classe e estão relacionados com a idéia de classes abstratas. Uma classe abstrata define uma interface, mas não necessariamente
fornece implementações para todas as suas funções. Ele atua como uma classe base a partir da qual as outras são derivadas. Uma classe derivada que implementa a funcionalidade ausente é chamado de uma classe concreta.


A ideia geral por trás do padrão Command é que ele nos fornece um meio para separar as responsabilidades de emissão de comandos, delegando essa responsabilidade para diferentes objetos.


Implementação, objetos de comando simples unem tanto uma ação e o objeto que desejam chamar a ação . Eles sempre incluem uma operação de execução (como o run () ou executar ()).
Todos os objetos de comando com a mesma interface podem ser facilmente trocados conforme o necessário e isso é considerado uma das maiores vantagens do padrão.

Para demonstrar o padrão de comando vamos criar um serviço de carro para compra simples.

````js
(function(){

  var carManager = {

    // request information
    requestInfo: function( model, id ){
      return "The information for " + model + " with ID " + id + " is foobar";
    },

    // purchase the car
    buyVehicle: function( model, id ){
      return "You have successfully purchased Item " + id + ", a " + model;
    },

    // arrange a viewing
    arrangeViewing: function( model, id ){
      return "You have successfully booked a viewing of " + model + " ( " + id + " ) ";
    }

  };

})();
````

Dando uma olhada no código acima, seria trivial para invocar os nossos métodos carManager acessando diretamente o objeto. Todos nós podemos ser perdoados por pensar que não há nada de errado com isso
tecnicamente , é completamente válido no JavaScript. No entanto, existem situações em que isto pode ser desvantajoso.


Por exemplo, imagine se a API do core de carManager fosse alterada. Isso exigiria todos os objetos que acessam diretamente esses métodos dentro de nossa aplicação precisariam ser modificados.
Isto pode ser visto como uma camada de acoplamento que efectivamente vai contra a metodologia OOP de objetos fracamente acoplados, tanto quanto possível . Em vez disso, poderia resolver este problema, abstraindo a API ainda mais.


Vamos agora expandir em nosso carManager para que a nossa aplicação dos resultados padrão de comando no seguinte : aceitar quaisquer métodos chamados que podem ser executadas no objeto carManager , passando ao longo de todos os dados que podem ser utilizadas, tais como o modelo do carro e ID .


Aqui está o que nós gostaríamos de ser capaz de alcançar :

````js
carManager.execute( "buyVehicle", "Ford Escort", "453543" );
````

De acordo com esta estrutura , devemos agora acrescentar uma definição para o método <b>carManager.execute</b> da seguinte forma:

````js
carManager.execute = function ( name ) {
    return carManager[name] && carManager[name].apply( carManager, [].slice.call(arguments, 1) );
};
````

Nossas chamadas amostra final seria assim se parecem como se segue :

````js
carManager.execute( "arrangeViewing", "Ferrari", "14523" );
carManager.execute( "requestInfo", "Ford Mondeo", "54323" );
carManager.execute( "requestInfo", "Ford Escort", "34232" );
carManager.execute( "buyVehicle", "Ford Escort", "34232" );
````
