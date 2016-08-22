# The Decorator Pattern

Decoradores são um padrão de design estrutural que visam promover a reutilização de código . Semelhante a Mixins , que pode ser considerada uma outra alternativa viável para opor sub -classes.

Classicamente, decoradores oferecido a capacidade de adicionar comportamento para classes existentes em um sistema dinamicamente. A ideia era que a própria decoração não era essencial para a funcionalidade básica da classe , caso contrário ele iria ser cozido no próprio superclasse.

Eles podem ser usados ​​para modificar os sistemas existentes onde desejam adicionar recursos adicionais para objetos sem a necessidade de modificar fortemente o código subjacente a usá-los . Uma razão comum pela qual os desenvolvedores usá-los é suas aplicações pode conter funcionalidades que requerem uma grande quantidade de diferentes tipos de objeto. Imagine ter que definir centenas de diferentes construtores de objetos para dizer, um jogo de JavaScript.

Os construtores de objetos poderia representar tipos de jogadores distintos, cada um com diferentes capacidades . Um Senhor do jogo Anéis poderia exigir construtores para Hobbit, Elf, Orc, Wizard, Mountain Giant, Stone Giant e assim por diante , mas pode facilmente ser centenas destes . Se, então, consignado em capacidades , imagine ter que criar sub- classes para cada combinação de tipo de capacidade por exemplo HobbitWithRing,HobbitWithSword, HobbitWithRingAndSword e assim por on.This não é muito prático e certamente não é controlável quando factor em um número crescente de diferentes habilidades.

O padrão Decorator não está fortemente ligada à forma como os objetos são criados , mas em vez disso se concentra sobre o problema de estender sua funcionalidade. Em vez de depender apenas de herança prototípica , trabalhamos com um único objeto base e progressivamente adicionar objetos decorador que fornecem as capacidades adicionais . A ideia é que ao invés de sub - classing , acrescentamos ( decoração ) propriedades ou métodos a um objeto de base, de modo que é um pouco mais simples.

A adição de novos atributos para objetos em JavaScript é um processo muito simples e direta assim com isto em mente, um decorador muito simplista pode ser implementada como segue:

## Exemplo 1: Decorando construtor com nova funcionalidade:

````js
// A vehicle constructor
function Vehicle( vehicleType ){

    // some sane defaults
    this.vehicleType = vehicleType || "car";
    this.model = "default";
    this.license = "00000-000";

}

// Test instance for a basic vehicle
var testInstance = new Vehicle( "car" );
console.log( testInstance );

// Outputs:
// vehicle: car, model:default, license: 00000-000

// Lets create a new instance of vehicle, to be decorated
var truck = new Vehicle( "truck" );

// New functionality we're decorating vehicle with
truck.setModel = function( modelName ){
    this.model = modelName;
};

truck.setColor = function( color ){
    this.color = color;
};

// Test the value setters and value assignment works correctly
truck.setModel( "CAT" );
truck.setColor( "blue" );

console.log( truck );

// Outputs:
// vehicle:truck, model:CAT, color: blue

// Demonstrate "vehicle" is still unaltered
var secondInstance = new Vehicle( "car" );
console.log( secondInstance );

// Outputs:
// vehicle: car, model:default, license: 00000-000
````

Este tipo de aplicação simplista é funcional, mas ele realmente não demonstrar todas as forças decoradores têm para oferecer. Para isso, estamos indo primeiro para passar pelo meu variação do exemplo de café de um excelente livro chamado Head First Design Patterns por Freeman , Serra e Bates, que é modelado em torno de uma compra Macbook.

## Exemplo 2 Decorando objetos com multiplus decoradores.

````js
// The constructor to decorate
function MacBook() {

  this.cost = function () { return 997; };
  this.screenSize = function () { return 11.6; };

}

// Decorator 1
function memory( macbook ) {

  var v = macbook.cost();
  macbook.cost = function() {
    return v + 75;
  };

}

// Decorator 2
function engraving( macbook ){

  var v = macbook.cost();
  macbook.cost = function(){
    return v + 200;
  };

}

// Decorator 3
function insurance( macbook ){

  var v = macbook.cost();
  macbook.cost = function(){
     return v + 250;
  };

}

var mb = new MacBook();
memory( mb );
engraving( mb );
insurance( mb );

// Outputs: 1522
console.log( mb.cost() );

// Outputs: 11.6
console.log( mb.screenSize() );
````
No exemplo acima , os nossos decoradores está substituindo o MacBook () super- classe objetos .cost function () para retornar o atual preço do Macbook mais o custo da atualização que está sendo especificado.

É considerado uma decoração como o Macbook objetos originais métodos construtores que não são substituídos (por exemplo screensize ()) , bem como quaisquer outras propriedades que podemos definir como uma parte do Macbook permanecem inalteradas e intactas.

Não há realmente uma interface definida no exemplo acima e nós estamos abandonando a responsabilidade de garantir um objeto encontra uma interface quando se deslocam do criador para o receptor.

## Pseudo classes decorators

Estamos indo para analisar uma variação do decorador apresentado pela primeira vez em uma forma JavaScript no Pro Patterns JavaScript Design ( PJDP ) por Dustin Diaz e Ross Harmes.


Ao contrário de alguns dos exemplos de mais cedo , Diaz e Harmes ficar mais de perto a forma como decoradores são implementados em outras linguagens de programação (tais como Java ou C ++ ), utilizando o conceito de "interface" , que irá definir com mais detalhes em breve.

Nota: Esta variação particular do padrão Decorator é fornecido para fins de referência. Se encontrá-lo excessivamente complexa , eu recomendo optando por uma das implementações mais simples abordados anteriormente.

## Interfaces

PJDP descreve o Decorator como um padrão que é usado para envolver de forma transparente objetos dentro de outros objetos da mesma interface. Uma interface é uma maneira de definir os métodos de um objeto deve ter, no entanto, que na verdade não especificar diretamente como esses métodos devem ser implementadas.

Eles também podem indicar quais os parâmetros dos métodos tomar , mas esta é considerada opcional.

Então, por que nós usaríamos uma interface em JavaScript ? A ideia é que eles são auto-documentado e promover a reutilização. Em teoria , as interfaces também tornar o código mais estável , garantindo alterações neles também deve ser feita nos objetos que os apliquem.

Abaixo está um exemplo de uma implementação de interfaces em JavaScript usando pato - digitando - uma abordagem que ajuda a determinar se um objeto é uma instância de construtor / object com base nos métodos que implementa.

````js
// Create interfaces using a pre-defined Interface
// constructor that accepts an interface name and
// skeleton methods to expose.

// In our reminder example summary() and placeOrder()
// represent functionality the interface should
// support
var reminder = new Interface( "List", ["summary", "placeOrder"] );

var properties = {
  name: "Remember to buy the milk",
  date: "05/06/2016",
  actions:{
    summary: function (){
      return "Remember to buy the milk, we are almost out!";
   },
    placeOrder: function (){
      return "Ordering milk from your local grocery store";
    }
  }
};

// Now create a constructor implementing the above properties
// and methods

function Todo( config ){

  // State the methods we expect to be supported
  // as well as the Interface instance being checked
  // against

  Interface.ensureImplements( config.actions, reminder );

  this.name = config.name;
  this.methods = config.actions;

}

// Create a new instance of our Todo constructor

var todoItem = new Todo( properties );

// Finally test to make sure these function correctly

console.log( todoItem.methods.summary() );
console.log( todoItem.methods.placeOrder() );

// Outputs:
// Remember to buy the milk, we are almost out!
// Ordering milk from your local grocery store
````

No exemplo acima, Interface.ensureImplements fornece a verificação completa de funcionalidade e de código para tanto esta como o construtor de interface pode ser encontrada aqui.

O maior problema com interfaces é que , como não é built-in suporte para eles em JavaScript, existe o perigo de nós tentar emular um recurso de outra língua que não pode ser um ajuste ideal . interfaces de leves podem ser usados ​​sem um grande custo desempenho no entanto e nós próxima olhar abstrato decoradores que utilizam esse mesmo conceito.

## Abstract Decorators

Para demonstrar a estrutura desta versão do padrão Decorator , vamos imaginar que temos uma superclasse que os modelos de um MacBook mais uma vez e uma loja que nos permite " decorar " o nosso Macbook com uma série de melhorias para uma taxa adicional.

Melhorias podem incluir upgrades para 4 GB ou 8 GB de RAM , gravura , Parallels ou um caso . Agora, se fôssemos para modelar isso usando uma sub- classe individual para cada combinação de opções de aprimoramento , que poderia ser algo como isto :

````js
var Macbook = function(){
        //...
};

var  MacbookWith4GBRam = function(){},
     MacbookWith8GBRam = function(){},
     MacbookWith4GBRamAndEngraving = function(){},
     MacbookWith8GBRamAndEngraving = function(){},
     MacbookWith8GBRamAndParallels = function(){},
     MacbookWith4GBRamAndParallels = function(){},
     MacbookWith8GBRamAndParallelsAndCase = function(){},
     MacbookWith4GBRamAndParallelsAndCase = function(){},
     MacbookWith8GBRamAndParallelsAndCaseAndInsurance = function(){},
     MacbookWith4GBRamAndParallelsAndCaseAndInsurance = function(){};
````

e assim por diante.

Esta seria uma solução impraticável como uma nova subclasse seria necessário para cada combinação possível de aperfeiçoamentos que estão disponíveis . Como nós preferimos manter as coisas simples, sem a manutenção de um grande conjunto de subclasses , vamos ver como podem ser utilizados decoradores para resolver este problema melhor.

Em vez de exigir todas as combinações que vimos anteriormente, devemos simplesmente tem que criar cinco novas classes decorador . Métodos que são chamados em destas classes de melhoria seria passada para a nossa classe Macbook.

Em nosso próximo exemplo, decoradores transparente envolver em torno de seus componentes e podem ser intercambiados interessante como eles usam a mesma interface.

Aqui está a interface vamos definir para o Macbook :

````js
var Macbook = new Interface( "Macbook",
  ["addEngraving",
  "addParallels",
  "add4GBRam",
  "add8GBRam",
  "addCase"]);

// A Macbook Pro might thus be represented as follows:
var MacbookPro = function(){
    // implements Macbook
};

MacbookPro.prototype = {
    addEngraving: function(){
    },
    addParallels: function(){
    },
    add4GBRam: function(){
    },
    add8GBRam:function(){
    },
    addCase: function(){
    },
    getPrice: function(){
      // Base price
      return 900.00;
    }
};
````

Para tornar mais fácil para nós para adicionar como muitas mais opções , conforme necessário , mais tarde, uma classe abstrata Decorator é definido com métodos padrão necessários para implementar a interface Macbook , que o resto das opções será sub- classe. Abstratos decoradores assegurar que podemos decorar uma classe base de forma independente com o maior número de decoradores , conforme necessário em diferentes combinações ( lembre-se do exemplo anterior ? ) , Sem necessidade de derivar uma classe para cada combinação possível

````js
// Macbook decorator abstract decorator class

var MacbookDecorator = function( macbook ){

    Interface.ensureImplements( macbook, Macbook );
    this.macbook = macbook;

};

MacbookDecorator.prototype = {
    addEngraving: function(){
        return this.macbook.addEngraving();
    },
    addParallels: function(){
        return this.macbook.addParallels();
    },
    add4GBRam: function(){
        return this.macbook.add4GBRam();
    },
    add8GBRam:function(){
        return this.macbook.add8GBRam();
    },
    addCase: function(){
        return this.macbook.addCase();
    },
    getPrice: function(){
        return this.macbook.getPrice();
    }
};
````

O que está acontecendo no exemplo acima é que o Macbook Decorator aceita um objeto ( um Macbook ) para usar como nosso componente base. Ele está usando a interface Macbook que definimos anteriormente e para cada método é apenas chamar o mesmo método no componente. Agora podemos criar nossas classes de opções para o que pode ser adicionado , apenas usando o Macbook Decorator.

````js
// First, define a way to extend an object a
// with the properties in object b. We'll use
// this shortly!
function extend( a, b ){
    for( var key in b )
        if( b.hasOwnProperty(key) )
            a[key] = b[key];
    return a;
}

var CaseDecorator = function( macbook ){
   this.macbook = macbook;
};

// Let's now extend (decorate) the CaseDecorator
// with a MacbookDecorator
extend( CaseDecorator, MacbookDecorator );

CaseDecorator.prototype.addCase = function(){
    return this.macbook.addCase() + "Adding case to macbook";
};

CaseDecorator.prototype.getPrice = function(){
    return this.macbook.getPrice() + 45.00;
};
````

O que estamos fazendo aqui é substituir o addCase () e getPrice () métodos que precisam ser decorados e nós estamos alcançando isso, primeiro chamar esses métodos no MacBook original e , em seguida, simplesmente anexando uma cadeia ou valor numérico (por exemplo, 45.00 ) para eles em conformidade.

Como tem havido um monte de informações apresentadas nesta seção até agora, vamos tentar trazê-lo todos juntos em um único exemplo que esperamos destacar o que aprendemos .

````js
// Instantiation of the macbook
var myMacbookPro = new MacbookPro();

// Outputs: 900.00
console.log( myMacbookPro.getPrice() );

// Decorate the macbook
var decoratedMacbookPro = new CaseDecorator( myMacbookPro );

// This will return 945.00
console.log( decoratedMacbookPro.getPrice() );
````

Como decoradores são capazes de modificar objetos dinamicamente , eles são um padrão perfeito para mudar os sistemas existentes. Ocasionalmente , é apenas mais simples para criar decoradores torno de um objeto em relação ao trabalho de manutenção sub-classes individuais para cada tipo de objeto. Isso faz manutenção de aplicações que podem exigir um grande número de objetos sub - classificada significativamente mais simples.

Uma versão funcional deste exemplo pode ser encontrado no [JSBin](http://jsbin.com/UMEJaXu/1/edit?html,js,output).

## Decorators com jQuery

Tal como acontece com outros padrões que cobrimos , há também exemplos do padrão Decorator que pode ser implementado com jQuery. jQuery.extend () nos permite estender ( ou mesclar ) dois ou mais objetos ( e suas propriedades ) juntos em um único objeto em tempo de execução.

Neste cenário, um objeto de destino pode ser decorado com novas funcionalidades , sem necessariamente quebrar ou substituir os métodos existentes no / object superclasse fonte ( embora isso possa ser feito).

No exemplo a seguir , definimos três objetos : padrões, opções e configurações . O objetivo da tarefa é decorar os padrões objeto com funcionalidade adicional encontrado em optionssettings . Nós devemos:

( A) Deixar " defaults " em um estado intocado em que não perdem a capacidade de acessar as propriedades ou funções encontrados em um ponto posterior ( b) obter a capacidade de usar as propriedades decorados e funções encontradas em "opções"

````js
var decoratorApp = decoratorApp || {};

// define the objects we're going to use
decoratorApp = {

    defaults: {
        validate: false,
        limit: 5,
        name: "foo",
        welcome: function () {
            console.log( "welcome!" );
        }
    },

    options: {
        validate: true,
        name: "bar",
        helloWorld: function () {
            console.log( "hello world" );
        }
    },

    settings: {},

    printObj: function ( obj ) {
        var arr = [],
            next;
        $.each( obj, function ( key, val ) {
            next = key + ": ";
            next += $.isPlainObject(val) ? printObj( val ) : val;
            arr.push( next );
        } );

        return "{ " + arr.join(", ") + " }";
    }

};

// merge defaults and options, without modifying defaults explicitly
decoratorApp.settings = $.extend({}, decoratorApp.defaults, decoratorApp.options);

// what we have done here is decorated defaults in a way that provides
// access to the properties and functionality it has to offer (as well as
// that of the decorator "options"). defaults itself is left unchanged

$("#log")
    .append( decoratorApp.printObj(decoratorApp.settings) +
          + decoratorApp.printObj(decoratorApp.options) +
          + decoratorApp.printObj(decoratorApp.defaults));

// settings -- { validate: true, limit: 5, name: bar, welcome: function (){ console.log( "welcome!" ); },
// helloWorld: function (){ console.log( "hello world" ); } }
// options -- { validate: true, name: bar, helloWorld: function (){ console.log( "hello world" ); } }
// defaults -- { validate: false, limit: 5, name: foo, welcome: function (){ console.log("welcome!"); } }
````

## Vantagens e desvantagens

Desenvolvedores gostam de usar esse padrão como ele pode ser usado de forma transparente e também é bastante flexível - como vimos , os objetos podem ser acondicionados ou " decorado" com o novo comportamento e , em seguida, continuar a ser utilizados sem a necessidade de se preocupar com o objeto de base que está sendo modificado . Num contexto mais amplo , esse padrão também evita -nos a necessidade de contar com um grande número de subclasses para obter os mesmos benefícios.

No entanto, existem desvantagens que devemos ter em conta quando implementar o padrão . Se mal gerida , pode complicar significativamente nossa arquitetura de aplicação, pois apresenta muitos objetos pequenos , mas semelhantes em nossa namespace. A preocupação aqui é que, além de tornar-se difícil de gerir , outros desenvolvedores não familiarizados com o padrão pode ter muita dificuldade para entender por que ele está sendo usado.

Suficiente comentando ou de pesquisa padrão deve ajudar com o último, no entanto , enquanto nós manter uma alça sobre como generalizada usamos o decorador em nossas aplicações que deve estar bem em ambas as contagens.
