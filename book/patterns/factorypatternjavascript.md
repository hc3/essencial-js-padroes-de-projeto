# The Factory Pattern

O padrão de fábrica é outro padrão criacional preocupado com a noção de criação de objetos. Onde ele difere dos outros padrões em sua categoria é que ele não exige explicitamente nos usar um construtor. Em vez disso, a fábrica pode fornecer uma interface genérica para a criação de objetos, onde podemos especificar o tipo de objeto de fábrica queremos ser criado.

Imagine que temos uma fábrica UI onde somos convidados a criar um tipo de componente UI. Em vez de criar esse componente diretamente usando o novo operador ou através de outro construtor criacional , pedimos um objeto de fábrica para um novo componente em vez disso. Informamos a Fábrica de que tipo de objeto é necessária (por exemplo, "Button" , "Painel " ) e instancia isso, devolvê-lo para nós para uso .

Isto é particularmente útil se o processo de criação do objecto é relativamente complexo , v.g. se depender fortemente de factores dinâmicos ou de configuração do aplicativo .

Exemplos de este padrão pode ser encontrado em bibliotecas de UI , como ExtJS onde os métodos para a criação de objectos ou componentes podem ser ainda subclasse .

O que se segue é um exemplo que se baseia em nossos trechos anteriores usando a lógica padrão Construtor para definir carros. Ele demonstra como uma fábrica de automóveis pode ser implementado usando o padrão de Fábrica :

````js
// Types.js - Constructors used behind the scenes

// A constructor for defining new cars
function Car( options ) {

  // some defaults
  this.doors = options.doors || 4;
  this.state = options.state || "brand new";
  this.color = options.color || "silver";

}

// A constructor for defining new trucks
function Truck( options){

  this.state = options.state || "used";
  this.wheelSize = options.wheelSize || "large";
  this.color = options.color || "blue";
}


// FactoryExample.js

// Define a skeleton vehicle factory
function VehicleFactory() {}

// Define the prototypes and utilities for this factory

// Our default vehicleClass is Car
VehicleFactory.prototype.vehicleClass = Car;

// Our Factory method for creating new Vehicle instances
VehicleFactory.prototype.createVehicle = function ( options ) {

  switch(options.vehicleType){
    case "car":
      this.vehicleClass = Car;
      break;
    case "truck":
      this.vehicleClass = Truck;
      break;
    //defaults to VehicleFactory.prototype.vehicleClass (Car)
  }

  return new this.vehicleClass( options );

};

// Create an instance of our factory that makes cars
var carFactory = new VehicleFactory();
var car = carFactory.createVehicle( {
            vehicleType: "car",
            color: "yellow",
            doors: 6 } );

// Test to confirm our car was created using the vehicleClass/prototype Car

// Outputs: true
console.log( car instanceof Car );

// Outputs: Car object of color "yellow", doors: 6 in a "brand new" state
console.log( car );
````

Abordagem # 1: Modificar uma instância VehicleFactory usar a classe Truck

````js
var movingTruck = carFactory.createVehicle( {
                      vehicleType: "truck",
                      state: "like new",
                      color: "red",
                      wheelSize: "small" } );

// Test to confirm our truck was created with the vehicleClass/prototype Truck

// Outputs: true
console.log( movingTruck instanceof Truck );

// Outputs: Truck object of color "red", a "like new" state
// and a "small" wheelSize
console.log( movingTruck );
````

Abordagem # 2: subclasse VehicleFactory para criar uma classe de fábrica que constrói caminhões

````js
function TruckFactory () {}
TruckFactory.prototype = new VehicleFactory();
TruckFactory.prototype.vehicleClass = Truck;

var truckFactory = new TruckFactory();
var myBigTruck = truckFactory.createVehicle( {
                    state: "omg..so bad.",
                    color: "pink",
                    wheelSize: "so big" } );

// Confirms that myBigTruck was created with the prototype Truck
// Outputs: true
console.log( myBigTruck instanceof Truck );

// Outputs: Truck object with the color "pink", wheelSize "so big"
// and state "omg. so bad"
console.log( myBigTruck );
````

## Quando usar o padrão factory


O padrão de fábrica pode ser especialmente útil quando aplicada às seguintes situações :
<ul>
  <li>Quando o nosso objeto ou configuração de componente envolve um alto nível de complexidade</li>
  <li>Quando precisamos de gerar facilmente diferentes instâncias de objetos , dependendo do ambiente em que estão em</li>
  <li>Quando estamos trabalhando com muitos pequenos objetos ou componentes que compartilham as mesmas propriedades</li>
  <li>Ao compor objetos com instâncias de outros objetos que só precisam satisfazer um contrato de API (aka , pato digitação) para trabalhar . Isto é útil para a dissociação .</li>
</ul>

## Quando não usar o padrão factory

Quando aplicado ao tipo errado de problema, este padrão pode introduzir um desnecessariamente grande quantidade de complexidade para um aplicativo. A menos que oferece uma interface para criação do objeto é um objetivo de design para a biblioteca ou estrutura que estamos escrevendo , eu sugiro que adere a construtores explícitos para evitar a sobrecarga desnecessária.

Devido ao facto de o processo de criação do objecto seja efectivamente captadas atrás de uma interface , este também pode apresentar problemas com o teste de unidade , dependendo apenas como complexo este processo pode ser.

# Abstract Factories

Também é útil estar ciente do padrão Abstract Factory , que pretende encapsular um grupo de fábricas individuais com um objetivo comum. Ele separa os detalhes da implementação de um conjunto de objetos de seu uso geral.

Um Abstract Factory deve ser usado quando um sistema deve ser independente da forma como os objetos que ele cria são gerados ou que precisa para trabalhar com vários tipos de objetos.

Um exemplo que é ao mesmo tempo simples e mais fácil de entender é uma fábrica de veículos , que define formas de obter ou matriculá tipos . A fábrica resumo pode ser chamado abstractVehicleFactory . A fábrica Abstract permitirá a definição de tipos de veículo como "carro " ou " caminhão " e as fábricas de betão irá implementar somente as classes que cumprir o contrato veículo (por exemplo Vehicle.prototype.drive e Vehicle.prototype.breakDown ).

````js
var abstractVehicleFactory = (function () {

  // Storage for our vehicle types
  var types = {};

  return {
      getVehicle: function ( type, customizations ) {
          var Vehicle = types[type];

          return (Vehicle ? new Vehicle(customizations) : null);
      },

      registerVehicle: function ( type, Vehicle ) {
          var proto = Vehicle.prototype;

          // only register classes that fulfill the vehicle contract
          if ( proto.drive && proto.breakDown ) {
              types[type] = Vehicle;
          }

          return abstractVehicleFactory;
      }
  };
})();


// Usage:

abstractVehicleFactory.registerVehicle( "car", Car );
abstractVehicleFactory.registerVehicle( "truck", Truck );

// Instantiate a new car based on the abstract vehicle type
var car = abstractVehicleFactory.getVehicle( "car", {
            color: "lime green",
            state: "like new" } );

// Instantiate a new truck in a similar manner
var truck = abstractVehicleFactory.getVehicle( "truck", {
            wheelSize: "medium",
            color: "neon yellow" } );
````
