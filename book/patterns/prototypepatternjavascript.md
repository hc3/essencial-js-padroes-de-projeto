# The Prototype Pattern

O livro do GOF se refere ao padrão prototype como uma maneira de criar objetos baseados em templates pré existentes para que objetos sejam criados atráves de clones.


Podemos pensar o padrão de prototype, como sendo baseado na herança prototípica onde criamos objetos que funcionam como protótipos para outros objetos.O próprio objeto prototype é efetivamente utilizado como um modelo para cada objeto criado pelo construtor.Se o prototype do construtor tiver uma propriedade chamada name por exemplo
então ada objeto criado por esse construtor tera a mesma propriedade.


Revendo as definições para esse padrão na literatura existente ( non-javascript ) , podemos encontrar referências a classes mais uma vez. A realidade é que a herança prototípica evita o uso de classes completamente. Não há um objeto "definido" , nem um objeto central na teoria. Estamos simplesmente a criando cópias de objetos funcionais existentes.


Um dos benefícios de usar o padrão de protótipo é que estamos trabalhando com os poderes que o prototype javascript tem a oferecer nativamente ao invés de tentar imitar características de outras linguagens, Com outros padrões de desenho , este não é sempre o caso.


Não só é o padrão de uma maneira fácil de implementar a herança , mas também pode vir com um aumento de desempenho , bem como: ao definir uma função em um objeto, eles estão todos criados por referência ( para que todos os objetos filho apontam para a mesma função) em vez de criar suas próprias cópias individuais.

Para deixar mais interessante, a herança prototipica foi definida no ECMAScript 5,e precisava usar o <b> Object.create </b> ( vamos ver nessa sessão como funciona ). Para nos lembrar, <b> Object.create </b> cria um objeto com um prototype específico e opcionalmente contem propriedades específicas ( <b>Object.create( prototype, descriçãoOpcional )</b> ).

Nos podemos demonstrar pelo exemplo a seguir:

````js
var myCar = {

  name: "Ford Escort",

  drive: function () {
    console.log( "Weeee. I'm driving!" );
  },

  panic: function () {
    console.log( "Wait. How do you stop this thing?" );
  }

};

// Use Object.create para criar uma instância de myCar
var yourCar = Object.create( myCar );

// Agora podemos ver que um é um protótipo do outro
console.log( yourCar.name );
````

<b>Object.create</b> também nos permite implementar conceitos avançados como herança diferencial onde objetos são habilitados para herdar de outros objetos. Vimos anteriormente que <b>Object.create</b> nos permite inicializar as propriedades do objeto usando o segundo argumento fornecido. exemplo:

````js
var vehicle = {
  getModel: function () {
    console.log( "The model of this vehicle is.." + this.model );
  }
};

var car = Object.create(vehicle, {

  "id": {
    value: MY_GLOBAL.nextId(),
    // writable:false, configurable:false by default
    enumerable: true
  },

  "model": {
    value: "Ford",
    enumerable: true
  }

});
````

Aqui as propriedades podem ser incializadas com o segundo argumento para <b>Object.create</b> usando o objeto literal com a sintaxe similar a usada em <b>Object.defineProperties</b> e <b>Object.defineProperty</b> como visto anteriormente.


Vale a pena notar que as relações prototipicas podem causar problemas ao enumerar as propriedades de objetos e (como Crockford recomenda ) envolvendo o conteúdo do laço em um <b>hasOwnProperty( )</b> exemplo:

````js
var vehiclePrototype = {

  init: function ( carModel ) {
    this.model = carModel;
  },

  getModel: function () {
    console.log( "The model of this vehicle is.." + this.model);
  }
};


function vehicle( model ) {

  function F() {};
  F.prototype = vehiclePrototype;

  var f = new F();

  f.init( model );
  return f;

}

var car = vehicle( "Ford Escort" );
car.getModel();
````

<b>Nota</b>: Esta alternativa não permite que o usuário defina propriedades somente leitura da mesma maneira ( como o vehiclePrototype pode ser alterada se não tomar cuidado ).

Uma alternativa final para implementação do padrão prototype pode ser vista aqui:

````js
var beget = (function () {

    function F() {}

    return function ( proto ) {
        F.prototype = proto;
        return new F();
    };
})();
````


Pode-se fazer referência a este método da função de veículo. Note-se, contudo, que veículo aqui está emulando um construtor , uma vez que o padrão prototype não inclui qualquer noção de inicialização além ligando um objeto para um protótipo.
