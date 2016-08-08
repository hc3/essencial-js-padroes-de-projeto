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

The methods above are effectively namespaced inside basketModule.
