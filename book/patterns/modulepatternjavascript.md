# Module Pattern

## Módulos

Os módulos são parte integral para criar uma aplicação robusta que siga um arquiterura organizada e que mantenha o código limpo, separado e organizado.

Em javascript existem algumas maneiras para implementar módulos:

<ul>
<li>Module Pattern</li>
<li>Object literal notation</li>
<li>ADM modules</li>
<li>CommonJS modules</li>
<li>ECMAScript Harmony modules</li>
</ul>

Nos vamos explorar outros mais adiante, nessa sessão vamos ver o Modern Modular Javascript Design Pattern.

### Object Literals

Na notação para criar um objeto literal, é criada uma variável que recebe ( {} ) os métodos são definidos dentro das chaves e são
separados por vírgula como no exemplo:

````js
var myObjectLiteral = {

    variableKey: variableValue,

    functionKey: function () {
      // ...
    }
};
````

Objetos literais nao precisam de instanciação usando o operador **new** mas não devemos nos esquecer de abrir e fechar chaves (**{}**)
objetos em javascript são dinâmicos então podemos adicionar métodos e atributos depois de sua criação:

````js
var myModule = {

  myProperty: "someValue",

  myConfig: {
    useCaching: true,
    language: "en"
  },

  saySomething: function () {
    console.log( "Where in the world is Paul Irish today?" );
  },

  reportMyConfig: function () {
    console.log( "Caching is: " + ( this.myConfig.useCaching ? "enabled" : "disabled") );
  },

  updateMyConfig: function( newConfig ) {

    if ( typeof newConfig === "object" ) {
      this.myConfig = newConfig;
      console.log( this.myConfig.language );
    }
  }
};

myModule.saySomething();

myModule.reportMyConfig();

myModule.updateMyConfig({
  language: "fr",
  useCaching: false
});

myModule.reportMyConfig();
````

Usando objetos literais podemos prover encapsulamento e organização do código, essa técnica sera otmizada usando o module pattern
que veremos adiante.

### Module Pattern

O module pattern foi originalmente definido para prover um meio de encapsular os campos como é feito em classes com o private.

Em javascript, o module patterns é usado para como uma forma de emular os conceitos de classes e com isso conseguimos criar
variáveis e métodos public/private dentro de objetos e com isso deixamos o escopo global mais limpo, evitando conflitos de nomes
entre funções ( lembre-se muitas variáveis no escopo global só terminar em merda ).

#### Privacidade

O module pattern encapsula privacidade, o estado e organização usando closures. isso prover uma maneira para mesclar public e private
e proteger parte do código que não deve ter acesso externo pelo escopo global ou uma colisão de nomes acidental. com esse padrão podemos
criar API's publicas e manter todo o acesso através de closures privadas.

Com isso temos uma solução limpa para projeteger nosso código deixando acessível o que deve ser acessível esse padrão é muito similar
ao IIFE ( immediately-invoked functional expression )exceto que o objeto é retornado pela função.

O javascript não tem modificadores de acesso como private ou public então o uso do module pattern é uma ótima saida para problema de acesso.

#### História

em uma perspectiva histórica o module pattern foi originalmente desenvolvido por várias pessoas incluindo Richard Cornford em 2003 e foi popularizado
um tempo depois por Douglas Crackford em publicações, é um padrão foi recorrente em YUI library.

Exemplos:

criaçao do module pattern.

````js
var testModule = (function () {

  var counter = 0;

  return {

    incrementCounter: function () {
      return counter++;
    },

    resetCounter: function () {
      console.log( "counter value prior to reset: " + counter );
      counter = 0;
    }
  };

})();

// Usage:

// Increment our counter
testModule.incrementCounter();
````

Podemos ver aqui que o acesso a variável counter está protegido, não é possível acessar diretamente a variável pelo escopo global, so
conseguimos acessar a variável através dos métodos <b>incrementCounter()</b> ou <b>resetCounter()</b> podemos fazer uma alusão ao modificador
private do java que nesse caso a idéia é a mesma só podemos acessar a variável counter através de métodos que vão intermediar esse acesso.

Exemplo:

````js
var myNamespace = (function () {

  var myPrivateVar, myPrivateMethod;

  // variável privada
  myPrivateVar = 0;

  // função privada recebendo argumentos.
  myPrivateMethod = function( foo ) {
      console.log( foo );
  };

  return {

    // variável publica
    myPublicVar: "foo",

    // função publica utilizando , privada
    myPublicFunction: function( bar ) {

      // incrementa contador privado
      myPrivateVar++;

      // chama método privado passando bar
      myPrivateMethod( bar );

    }
  };

})();
````

Vamos ver outro exemplo, abaixo podemos ver shopping basket aplication implemetada usando o module pattern. O module é completamente
auto suficiente com a vairável global chamada <b>basketModule</b>. dentro do module temos um array chamado <b>basket</b> como uma variável
privada, mas outras partes da apliação podem alterar essa variável esse é o efeito das closures:

````js
var basketModule = (function () {

  // privado

  var basket = [];

  function doSomethingPrivate() {
    //...
  }

  function doSomethingElsePrivate() {
    //...
  }

  // retorna objeto e expoem como publico
  return {


    addItem: function( values ) {
      basket.push(values);
    },


    getItemCount: function () {
      return basket.length;
    },

    // alias publico para função privada
    doSomething: doSomethingPrivate,


    getTotal: function () {

      var q = this.getItemCount(),
          p = 0;

      while (q--) {
        p += basket[q].price;
      }

      return p;
    }
  };
})();
````

Dentro desse modulo vai ser retornado um objeto que sera automáticamente assinalado apra <b>basketModule</b> e a partir da ai podemos interagir:

````js

// basketModule retorna um objeto que pode ser usado como uma API publica

basketModule.addItem({
  item: "bread",
  price: 0.5
});

basketModule.addItem({
  item: "butter",
  price: 0.3
});


console.log( basketModule.getItemCount() );

console.log( basketModule.getTotal() );

// retornará undefined pois basket não está disponível para o escopo publico
console.log( basketModule.basket );

// This also won't work as it only exists within the scope of our
// basketModule closure, but not in the returned public object
console.log( basket );
````

Podemos ver como o scopo da função acima são chamadas imediatamente (IFFE) retornando o valor. Isso nos da uma série de vantagens
incluindo:

<ul>
<li>A liberdade de ter funções e atributos privados que só podem ser consumidos por nosso módulo. e com isso deixamos exposto apenas
a nossa API e isso é considerado verdadeiramente privada</li>
<li>Dado que as funções são declaradas normalmente como named functions, isso deixa o código mais fácil de depurar</li>
<li>
Como T.J Crowder assinalou no passado , também nos permite retornar diferentes funções, dependendo do ambiente. No passado ,
 eu vi os desenvolvedores usar isso para realizar testes UA , a fim de fornecer um código -path no seu módulo específico para o IE ,
 mas podemos facilmente optar por detecção de recurso nos dias de hoje para alcançar um objetivo similar.</li>
</ul>

#### Variações do module Pattern

<b> Import mixins </b>
Essa variação mostra como variáveis globais ( jQuery , Underscore ) podem ser passadas como argumento para funções anônimas.
e com isso podemos importar alias locais.

````js

// módulo global
var myModule = (function ( jQ, _ ) {

    function privateMethod1(){
        jQ(".container").html("test");
    }

    function privateMethod2(){
      console.log( _.min([10, 5, 100, 2, 1000]) );
    }

    return{
        publicMethod: function(){
            privateMethod1();
        }
    };

// Pull in jQuery and Underscore
})( jQuery, _ );

myModule.publicMethod();
````

<b> Exports </b>

A próxima variação permite que declaremos variáveis globais sem consumi-las que é um concento semelhante ao conceito de importar globais.

````js
// Módulo global
var myModule = (function () {

  // Módulo Objeto
  var module = {},
    privateVariable = "Hello World";

  function privateMethod() {
    // ...
  }

  module.publicProperty = "Foobar";
  module.publicMethod = function () {
    console.log( privateVariable );
  };

  return module;

})();
````

### aguardando tradução:
<b>dojo</b>

Dojo provides a convenience method for working with objects called dojo.setObject(). This takes as its first argument a dot-separated string such as myObj.parent.child which refers to a property called "child" within an object "parent" defined inside "myObj". Using setObject() allows us to set the value of children, creating any of the intermediate objects in the rest of the path passed if they don't already exist.

For example, if we wanted to declare basket.core as an object of the store namespace, this could be achieved as follows using the traditional way:

````js
var store = window.store || {};

if ( !store["basket"] ) {
  store.basket = {};
}

if ( !store.basket["core"] ) {
  store.basket.core = {};
}

store.basket.core = {
  // ...rest of our logic
};
````

Or as follows using Dojo 1.7 (AMD-compatible version) and above:

````js
require(["dojo/_base/customStore"], function( store ){

  // using dojo.setObject()
  store.setObject( "basket.core", (function() {

      var basket = [];

      function privateMethod() {
          console.log(basket);
      }

      return {
          publicMethod: function(){
                  privateMethod();
          }
      };

  })());

});
````

For more information on dojo.setObject(), see the official documentation.

<b>ExtJS</b>

For those using Sencha's ExtJS, an example demonstrating how to correctly use the Module pattern with the framework can be found below.

Here, we see an example of how to define a namespace which can then be populated with a module containing both a private and public API. With the exception of some semantic differences, it's quite close to how the Module pattern is implemented in vanilla JavaScript:

````js
// create namespace
Ext.namespace("myNameSpace");

// create application
myNameSpace.app = function () {

  // do NOT access DOM from here; elements don't exist yet

  // private variables
  var btn1,
      privVar1 = 11;

  // private functions
  var btn1Handler = function ( button, event ) {
      console.log( "privVar1=" + privVar1 );
      console.log( "this.btn1Text=" + this.btn1Text );
    };

  // public space
  return {
    // public properties, e.g. strings to translate
    btn1Text: "Button 1",

    // public methods
    init: function () {

      if ( Ext.Ext2 ) {

        btn1 = new Ext.Button({
          renderTo: "btn1-ct",
          text: this.btn1Text,
          handler: btn1Handler
        });

      } else {

        btn1 = new Ext.Button( "btn1-ct", {
          text: this.btn1Text,
          handler: btn1Handler
        });

      }
    }
  };
}();
````

<b>YUI</b>

Similarly, we can also implement the Module pattern when building applications using YUI3. The following example is heavily based on the original YUI Module pattern implementation by Eric Miraglia, but again, isn't vastly different from the vanilla JavaScript version:

````js
Y.namespace( "store.basket" ) ;
Y.store.basket = (function () {

    var myPrivateVar, myPrivateMethod;

    // private variables:
    myPrivateVar = "I can be accessed only within Y.store.basket.";

    // private method:
    myPrivateMethod = function () {
        Y.log( "I can be accessed only from within YAHOO.store.basket" );
    }

    return {
        myPublicProperty: "I'm a public property.",

        myPublicMethod: function () {
            Y.log( "I'm a public method." );

            // Within basket, I can access "private" vars and methods:
            Y.log( myPrivateVar );
            Y.log( myPrivateMethod() );

            // The native scope of myPublicMethod is store so we can
            // access public members using "this":
            Y.log( this.myPublicProperty );
        }
    };

})();
````

<b>jQuery</b>

There are a number of ways in which jQuery code unspecific to plugins can be wrapped inside the Module pattern. Ben Cherry previously suggested an implementation where a function wrapper is used around module definitions in the event of there being a number of commonalities between modules.

In the following example, a library function is defined which declares a new library and automatically binds up the init function to document.ready when new libraries (ie. modules) are created.

````js
function library( module ) {

  $( function() {
    if ( module.init ) {
      module.init();
    }
  });

  return module;
}

var myLibrary = library(function () {

  return {
    init: function () {
      // module implementation
    }
  };
}());
````

<b>Advantages</b>

We've seen why the Constructor pattern can be useful, but why is the Module pattern a good choice? For starters, it's a lot cleaner for developers coming from an object-oriented background than the idea of true encapsulation, at least from a JavaScript perspective.

Secondly, it supports private data - so, in the Module pattern, public parts of our code are able to touch the private parts, however the outside world is unable to touch the class's private parts (no laughing! Oh, and thanks to David Engfer for the joke).

<b>Disadvantages</b>

The disadvantages of the Module pattern are that as we access both public and private members differently, when we wish to change visibility, we actually have to make changes to each place the member was used.

We also can't access private members in methods that are added to the object at a later point. That said, in many cases the Module pattern is still quite useful and when used correctly, certainly has the potential to improve the structure of our application.

Other disadvantages include the inability to create automated unit tests for private members and additional complexity when bugs require hot fixes. It's simply not possible to patch privates. Instead, one must override all public methods which interact with the buggy privates. Developers can't easily extend privates either, so it's worth remembering privates are not as flexible as they may initially appear.

For further reading on the Module pattern, see Ben Cherry's excellent in-depth article on it.
