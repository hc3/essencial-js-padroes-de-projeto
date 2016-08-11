# The Singleton Pattern

O padrão singleton é assim conhecido por restringir a criação de um e apenas um objeto para uma determinada classe, ele é implementado criando uma classe com um método que cria uma instancia de um objeto que não existe e se já existir uma instancia para esse objeto ele simplemente retorna a referência para esse objeto.

Singletons diferem das classes estáticas ( ou objetos ), como podemos atrasar sua inicialização isso ocorre geralmente porque eles exigem algumas inforomações que podem não estar disponíveis durante o tempo de inicialização. eles não fornecem uma maneira para o código que não tem conhecimento prévio da referencia anterior a eles para recupera-los facilmente. isso porque não é nem um objeto nem uma classe que é retornado por um singleton é uma estrutura.

Em javascript, Singletons são dividos com arquivos anmepsace com implementação isolada do código para um namespace global que prove um ponto único para acessar funções.

Podemos implementar singletons seguindo o código abaixo:

````js
var mySingleton = (function () {

  // Instance stores a reference to the Singleton
  var instance;

  function init() {

    // Singleton

    // Private methods and variables
    function privateMethod(){
        console.log( "I am private" );
    }

    var privateVariable = "Im also private";

    var privateRandomNumber = Math.random();

    return {

      // Public methods and variables
      publicMethod: function () {
        console.log( "The public can see me!" );
      },

      publicProperty: "I am also public",

      getRandomNumber: function() {
        return privateRandomNumber;
      }

    };

  };

  return {

    // cria uma nova instância se não existir ou retornar se já existir
    getInstance: function () {

      if ( !instance ) {
        instance = init();
      }

      return instance;
    }

  };

})();

var myBadSingleton = (function () {

  // Instance stores a reference to the Singleton
  var instance;

  function init() {

    // Singleton

    var privateRandomNumber = Math.random();

    return {

      getRandomNumber: function() {
        return privateRandomNumber;
      }

    };

  };

  return {

    // Always create a new Singleton instance
    getInstance: function () {

      instance = init();

      return instance;
    }

  };

})();


// Usage:

var singleA = mySingleton.getInstance();
var singleB = mySingleton.getInstance();
console.log( singleA.getRandomNumber() === singleB.getRandomNumber() ); // true

var badSingleA = myBadSingleton.getInstance();
var badSingleB = myBadSingleton.getInstance();
console.log( badSingleA.getRandomNumber() !== badSingleB.getRandomNumber() ); // true

// Note: as we are working with random numbers, there is a
// mathematical possibility both numbers will be the same,
// however unlikely. The above example should otherwise still
// be valid.
````

O que torna um objeto um singleton é o acesso global a instância ( geralmente através MySingleton.getInstance() ).

No livro do GOF a aplicabilidade do padrão singleton é descrita assim:

<ul>
<li>Existe exatamente uma instância da classe e ela pode ser acessível no cliente de qualquer lugar. ( global )</li>
<li>
Quando a única instância deve ser extensível por subclasses , e os clientes devem ser capazes de usar uma instância
estendida sem modificar seu código </li>
</ul>


O segundo destes pontos refere-se a um caso em que poderá ser necessário um código como :

````js
mySingleton.getInstance = function(){
  if ( this._instance == null ) {
    if ( isFoo() ) {
       this._instance = new FooSingleton();
    } else {
       this._instance = new BasicSingleton();
    }
  }
  return this._instance;
};
````

Aqui, getInstance torna-se um pouco como um método de fábrica e não precisa atualizar cada ponto no nosso código acessá-lo. FooSingleton acima seria uma subclasse de BasicSingleton e implementar a mesma interface.

Por que adiar a execução considerada importante para uma Singleton ?:


Em C ++ , serve para isolar a imprevisibilidade da ordem de inicialização dinâmica, retornando o controle para o programador .


É importante notar a diferença entre uma instância estática de uma classe ( objecto ) e uma Singleton : enquanto um Singleton pode ser implementado como um exemplo estática, ela também pode ser construído lazily ( preguiçosamente ), sem a necessidade de recursos nem memória até que esta é realmente necessário.


Se tivermos um objeto estático que pode ser inicializado diretamente , é preciso garantir que o código é sempre executada na mesma ordem (por exemplo, no caso objCar precisa objWheel durante sua inicialização ) e este não escala quando você tem um grande número de fonte arquivos.


Ambos os Singletons e objetos estáticos são úteis, mas eles não devem ser usado em demasia - da mesma forma em que não devemos fazer isso com outros padrões.


Na prática , o padrão Singleton é útil quando exatamente um objecto é necessário para coordenar os outros através de um sistema.

Aqui é um exemplo, com o padrão a ser usado neste contexto :

````js
var SingletonTester = (function () {

  // options: an object containing configuration options for the singleton
  // e.g var options = { name: "test", pointX: 5};
  function Singleton( options ) {

    // set options to the options supplied
    // or an empty object if none are provided
    options = options || {};

    // set some properties for our singleton
    this.name = "SingletonTester";

    this.pointX = options.pointX || 6;

    this.pointY = options.pointY || 10;

  }

  // our instance holder
  var instance;

  // an emulation of static variables and methods
  var _static = {

    name: "SingletonTester",

    // Method for getting an instance. It returns
    // a singleton instance of a singleton object
    getInstance: function( options ) {
      if( instance === undefined ) {
        instance = new Singleton( options );
      }

      return instance;

    }
  };

  return _static;

})();

var singletonTest = SingletonTester.getInstance({
  pointX: 5
});

// Log the output of pointX just to verify it is correct
// Outputs: 5
console.log( singletonTest.pointX );
````

O Singleton é um padrão válido para uso, mas quando precisamos dele frequentemente em nosso código javascript, pode ser que devamos rever nosso design.


Eles são frequentemente uma indicação de que os módulos em um sistema ou são fortemente acoplados ou que a lógica é excessivamente espalhados por várias partes de uma base de código. Singletons pode ser mais difíceis para testar devido a questões que vão desde dependências escondidas, a dificuldade em criar várias instâncias , dificuldade em arrancar as dependências e assim por diante.
