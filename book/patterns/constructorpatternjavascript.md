# Padrão construtor.

Nas classicas linguagens orientadas a objetos um construtor é uma método especial usado apra incializar um novo objeto criado. Em Javascript tudo são objetos, e nessa sessão vamos nos focar em objetos construtores.

Objetos construtores são usados para criar tipos específicos de objetos, preparados para usar os argumentos passados para o construtor
e podemos usar esse argumentos tanto para propriedades como métodos quando o objeto for criado.

<b> Três maneiras de criar um objeto <b>
````js
// criando objeto vazio de três formas diferentes.

var newObject = {};

// ou
var newObject = Object.create( Object.prototype );

// ou
var newObject = new Object();
````

No código acima foi exibida três formas diferentes para se criar um objeto vazio, abaixo vamos criar o objeto com alguns valores

````js
// adicionando propriedades
newObject.someKey = "Hello World";

// recuperando propriedades
var value = newObject.someKey;

// adicionando propriedades
newObject["someKey"] = "Hello World";

// recuperando propriedades
var value = newObject["someKey"];

// usando o método fineProperty para criar uma nova propriedade
Object.defineProperty( newObject, "someKey", {
    value: "for more control of the property's behavior",
    writable: true,
    enumerable: true,
    configurable: true
});

var defineProp = function ( obj, key, value ){
  var config = {
    value: value,
    writable: true,
    enumerable: true,
    configurable: true
  };
  Object.defineProperty( obj, key, config );
};

// criando objeto vazio
var person = Object.create( Object.prototype );

// populando objeto person com dados.
defineProp( person, "car", "Delorean" );
defineProp( person, "dateOfBirth", "1981" );
defineProp( person, "hasBeard", false );

console.log(person);

// criando novas propriedades
Object.defineProperties( newObject, {

  "someKey": {
    value: "Hello World",
    writable: true
  },

  "anotherKey": {
    value: "Foo bar",
    writable: false
  }

});
````
Como veremos mais adiante esse métodos podem ser usados para herança:

````js

// cria o objeto driver e faz herança com objeto person.
var driver = Object.create( person );

// criando propriedades para objeto driver.
defineProp(driver, "topSpeed", "100mph");

// Pegando herança do objeto person.
console.log( driver.dateOfBirth );

// recuperando valor da propriedade topSpeed.
console.log( driver.topSpeed );

````

## Construtores básicos

O javascript não tem o conceito de classes mas suporta funções construtores que podem criar objetos usando a palavra reservada **this** para referenciar os
atributos do próprio objeto criado e com o operador **new** podemos criar um novo objeto vamos ver o código:

````js
function Car( model, year, miles ) {

  this.model = model;
  this.year = year;
  this.miles = miles;

  this.toString = function () {
    return this.model + " has done " + this.miles + " miles";
  };
}

// Uso:

// podemos criar novas instâncias do objeto car.
var civic = new Car( "Honda Civic", 2009, 20000 );
var mondeo = new Car( "Ford Mondeo", 2010, 5000 );

console.log( civic.toString() );
console.log( mondeo.toString() );
````

Com essa pequena versão do padrão construtor resolvemos o problema da criação de objetos, mas ainda temos alguns problemas. Um deles
é a dificuldade de herdar já que para todo novo carro criado temos uma referenciação para o método toString() e isso não é algo bom
para a performance.

### Construtores com prototypes.

Podemos criar objetos com funções construtoras mas ainda existe o atributo "prototype" que todo o objeto tem esse atributo, quando
invocamos um construtor para criar um objeto todos as propriedades do prototype do construtor estão disponíveis para o novo objeto
podemos criar vários objetos com acesso ao mesmo prototype, vamos ver o exemplo:

````js
function Car( model, year, miles ) {

  this.model = model;
  this.year = year;
  this.miles = miles;

}


// veja que vamos usar Object.prototype.newMethod para criar um método que será herdado
Car.prototype.toString = function () {
  return this.model + " has done " + this.miles + " miles";
};

// Uso:

var civic = new Car( "Honda Civic", 2009, 20000 );
var mondeo = new Car( "Ford Mondeo", 2010, 5000 );

console.log( civic.toString() );
console.log( mondeo.toString() );
````
Veja que agora só existe uma instância de toString() que está no construtor **Car** e sera compartilhada com todas as instâncias
do objeto.
