# Flyweight

O padrão Flyweight é uma solução estrutural clássica para otimizar o código que é repetitivo , lento e compartilha de forma ineficiente de dados. Destina-se a minimizar o uso de memória em um aplicativo através da partilha de dados, tanto quanto possível com objetos relacionados ( configuração do aplicativo por exemplo , estado e assim por diante ) .

O padrão foi concebido por Paul Calder e Mark Linton em 1990 e foi nomeado após a classe de peso do boxe , que inclui lutadores com peso inferior a 112lb . O próprio Flyweight nome é derivado dessa classificação peso, pois refere-se ao peso pequeno ( espaço de memória ) o padrão visa nos ajudar a alcançar.

Na prática, Flyweight compartilhamento de dados pode envolver levando vários objetos semelhantes ou construções de dados usados ​​por um número de objetos e colocando esses dados em um único objeto externo. Nós podemos passar por este objecto para aqueles dependendo estes dados , em vez de armazenar dados de forma idêntica em cada uma.

## Usando Flyweights

Existem duas maneiras em que a FLYWEIGHT podem ser aplicadas . A primeira é a camada de dados, onde lidamos com o conceito de partilha de dados entre grandes quantidades de objetos semelhantes armazenados na memória.

A segunda é a de camada DOM em que a Mosca pode ser usado como um evento de gestor central para evitar anexar manipuladores de eventos para cada elemento filho num recipiente pai que deseja ter um comportamento semelhante.

Como a camada de dados é onde o FLYWEIGHT é a mais utilizada tradicionalmente , vamos dar uma olhada neste primeiro.

## Flyweights and sharing data

Para esta aplicação , existem mais alguns conceitos em torno do padrão Flyweight clássica que temos de ter em conta. No FLYWEIGHT há um conceito de dois estados - intrínseca e extrínseca . informações intrínseca pode ser exigido por métodos internos em nossos objetos que eles absolutamente não pode funcionar sem . informação extrínseca pode no entanto ser removida e armazenada externamente.

Objectos com os mesmos dados intrínsecas podem ser substituídos por um único objecto partilhado , criado por um método de fábrica . Isso nos permite reduzir a quantidade global de dados implícitos sendo armazenados de forma bastante significativa.

O benefício disso é que somos capazes de manter um olho em objetos que já foram instanciado para que novas cópias são sempre apenas criado caso o estado intrínseco diferente do objeto que já temos.

Nós usamos um gerente para lidar com os estados extrínsecos. Como isso é implementado pode variar , mas uma abordagem a este para ter o objeto do gerenciador de conter uma base de dados central dos estados extrínsecos e os objetos flyweight que a que pertencem.

## Implementação classica Flyweights

Como o padrão Flyweight não tem sido muito utilizado em JavaScript nos últimos anos, muitas das implementações que poderiam usar para a inspiração vem dos mundos Java e C ++.

Our first look at Flyweights in code is my JavaScript implementation of the Java sample of the Flyweight pattern from Wikipedia (http://en.wikipedia.org/wiki/Flyweight_pattern).

Nós vamos estar fazendo uso de três tipos de componentes Flyweight desta aplicação , que estão listados abaixo:
<ul>
  <li><b>Flyweight</b> corresponde a uma interface através da qual flyweights são capazes de receber e agir em estados extrínsecos</li>
  <li><b>Concrete flyweight</b> actually implements the Flyweight interface and stores intrinsic state. Concrete Flyweights need to be sharable and capable of manipulating state that is extrinsic</li>
  <li><b>Flyweight factory</b>gerencia objetos flyweight e cria -los também. Ele garante que nossos flyweights são compartilhados e gerencia -los como um grupo de objetos que podem ser consultados se precisarmos de casos individuais. Se um objeto já foi criado no grupo devolve -lo , caso contrário, ele adiciona um novo objeto para a piscina e devolve-lo .</li>
</ul>

Estas correspondem às seguintes definições em nossa implementação :

<ul>
  <li>CoffeeOrder: Flyweight</li>
  <li>CoffeeFlavor: Concrete Flyweight</li>
  <li>CoffeeOrderContext: Helper</li>
  <li>CoffeeFlavorFactory: Flyweight Factory</li>
  <li>testFlyweight: Utilization of our Flyweights</li>
</ul>

## Duck punching "implements"

Duck perfuração nos permite estender as capacidades de um idioma ou uma solução sem necessariamente precisar modificar a origem de tempo de execução. Como esta próxima solução requer o uso de uma palavra-chave Java ( implementos ) para a implementação de interfaces e não é encontrado em JavaScript nativamente , vamos primeiro pato acioná-lo.

Function.prototype.implementsFor trabalha em uma empresa de construção objeto e aceitará uma classe pai (função) ou o objeto e quer herdar desta usando herança normal ( para funções ) ou herança virtual ( para objetos ).

````js
// Simulate pure virtual inheritance/"implement" keyword for JS
Function.prototype.implementsFor = function( parentClassOrObject ){
    if ( parentClassOrObject.constructor === Function )
    {
        // Normal Inheritance
        this.prototype = new parentClassOrObject();
        this.prototype.constructor = this;
        this.prototype.parent = parentClassOrObject.prototype;
    }
    else
    {
        // Pure Virtual Inheritance
        this.prototype = parentClassOrObject;
        this.prototype.constructor = this;
        this.prototype.parent = parentClassOrObject;
    }
    return this;
};
````

Podemos usar isso para corrigir a falta de uma palavra-chave implementos por ter uma função de herdar uma interface explicitamente. Abaixo , CoffeeFlavor implementa a interface CoffeeOrder e deve conter os seus métodos de interface para que nós para atribuir a funcionalidade de ligar essas implementações a um objeto.

````js
// Flyweight object
var CoffeeOrder = {

  // Interfaces
  serveCoffee:function(context){},
    getFlavor:function(){}

};


// ConcreteFlyweight object that creates ConcreteFlyweight
// Implements CoffeeOrder
function CoffeeFlavor( newFlavor ){

    var flavor = newFlavor;

    // If an interface has been defined for a feature
    // implement the feature
    if( typeof this.getFlavor === "function" ){
      this.getFlavor = function() {
          return flavor;
      };
    }

    if( typeof this.serveCoffee === "function" ){
      this.serveCoffee = function( context ) {
        console.log("Serving Coffee flavor "
          + flavor
          + " to table number "
          + context.getTable());
    };
    }

}


// Implement interface for CoffeeOrder
CoffeeFlavor.implementsFor( CoffeeOrder );


// Handle table numbers for a coffee order
function CoffeeOrderContext( tableNumber ) {
   return{
      getTable: function() {
         return tableNumber;
     }
   };
}


function CoffeeFlavorFactory() {
    var flavors = {},
    length = 0;

    return {
        getCoffeeFlavor: function (flavorName) {

            var flavor = flavors[flavorName];
            if (typeof flavor === "undefined") {
                flavor = new CoffeeFlavor(flavorName);
                flavors[flavorName] = flavor;
                length++;
            }
            return flavor;
        },

        getTotalCoffeeFlavorsMade: function () {
            return length;
        }
    };
}

// Sample usage:
// testFlyweight()

function testFlyweight(){


  // The flavors ordered.
  var flavors = new CoffeeFlavor(),

  // The tables for the orders.
    tables = new CoffeeOrderContext(),

  // Number of orders made
    ordersMade = 0,

  // The CoffeeFlavorFactory instance
    flavorFactory;

  function takeOrders( flavorIn, table) {
     flavors[ordersMade] = flavorFactory.getCoffeeFlavor( flavorIn );
     tables[ordersMade++] = new CoffeeOrderContext( table );
  }

   flavorFactory = new CoffeeFlavorFactory();

   takeOrders("Cappuccino", 2);
   takeOrders("Cappuccino", 2);
   takeOrders("Frappe", 1);
   takeOrders("Frappe", 1);
   takeOrders("Xpresso", 1);
   takeOrders("Frappe", 897);
   takeOrders("Cappuccino", 97);
   takeOrders("Cappuccino", 97);
   takeOrders("Frappe", 3);
   takeOrders("Xpresso", 3);
   takeOrders("Cappuccino", 3);
   takeOrders("Xpresso", 96);
   takeOrders("Frappe", 552);
   takeOrders("Cappuccino", 121);
   takeOrders("Xpresso", 121);

   for (var i = 0; i < ordersMade; ++i) {
       flavors[i].serveCoffee(tables[i]);
   }
   console.log(" ");
   console.log("total CoffeeFlavor objects made: " + flavorFactory.getTotalCoffeeFlavorsMade());
}
````

## Converting code to use the Flyweight pattern

Em seguida, vamos continuar nossa olhada contrapesos através da implementação de um sistema para gerenciar todos os livros em uma biblioteca. O importante meta- dados para cada livro poderia provavelmente ser repartidos da seguinte forma:

<ul>
  <li>ID</li>
  <li>Title</li>
  <li>Author</li>
  <li>Genre</li>
  <li>Page count</li>
  <li>Publisher ID</li>
  <li>ISBN</li>
</ul>

Também vamos exigir as seguintes propriedades para manter o controle de qual membro tenha verificado um livro especial, a data que eles checar a , bem como a data de retorno esperada.

<ul>
  <li>checkoutDate</li>
  <li>checkoutMember</li>
  <li>dueReturnDate</li>
  <li>availability</li>
</ul>

Cada livro , assim, ser representado da seguinte forma , antes de qualquer optimização usando o FLYWEIGHT:

````js
var Book = function( id, title, author, genre, pageCount,publisherID, ISBN, checkoutDate, checkoutMember, dueReturnDate,availability ){

   this.id = id;
   this.title = title;
   this.author = author;
   this.genre = genre;
   this.pageCount = pageCount;
   this.publisherID = publisherID;
   this.ISBN = ISBN;
   this.checkoutDate = checkoutDate;
   this.checkoutMember = checkoutMember;
   this.dueReturnDate = dueReturnDate;
   this.availability = availability;

};

Book.prototype = {

  getTitle: function () {
     return this.title;
  },

  getAuthor: function () {
     return this.author;
  },

  getISBN: function (){
     return this.ISBN;
  },

  // For brevity, other getters are not shown
  updateCheckoutStatus: function( bookID, newStatus, checkoutDate, checkoutMember, newReturnDate ){

     this.id = bookID;
     this.availability = newStatus;
     this.checkoutDate = checkoutDate;
     this.checkoutMember = checkoutMember;
     this.dueReturnDate = newReturnDate;

  },

  extendCheckoutPeriod: function( bookID, newReturnDate ){

      this.id = bookID;
      this.dueReturnDate = newReturnDate;

  },

  isPastDue: function(bookID){

     var currentDate = new Date();
     return currentDate.getTime() > Date.parse( this.dueReturnDate );

   }
};
````

Isso provavelmente funciona bem inicialmente para pequenas coleções de livros , porém, como a biblioteca se expande para incluir um inventário maior com várias versões e cópias de cada livro disponível , podemos encontrar o sistema de gestão rodando mais lento e mais lento ao longo do tempo . Usando milhares de objetos do livro podem sobrecarregar a memória disponível , mas podemos otimizar nosso sistema usando o padrão Flyweight para melhorar isso.

agora podemos separar os nossos dados em estados intrínsecos e extrínsecos da seguinte forma: os dados relevantes para o objeto livro ( título, autor etc) é intrínseco , enquanto os dados de verificação geral ( checkoutMember , dueReturnDate etc) é considerado extrínseca. Efetivamente , isto significa que apenas um objeto livro é necessária para cada combinação de propriedades de livros. ainda é uma quantidade considerável de objetos , mas um número significativamente menor do que tínhamos anteriormente .

O seguinte exemplo único de nossos combinações meta- dados do livro será compartilhado entre todos os exemplares de um livro com um título particular.

````js
// Flyweight optimized version
var Book = function ( title, author, genre, pageCount, publisherID, ISBN ) {

    this.title = title;
    this.author = author;
    this.genre = genre;
    this.pageCount = pageCount;
    this.publisherID = publisherID;
    this.ISBN = ISBN;

};
````

Como podemos ver , os estados extrínsecos foram removidos. Tudo a ver com a biblioteca check-outs será movido para um gestor e como os dados do objeto está agora segmentada , uma fábrica pode ser usado para instanciação.

## A Basic Factory

Vamos agora definir uma fábrica muito básico. O que nós vamos ter que fazer é executar uma verificação para ver se um livro com um título em particular tenha sido previamente criado dentro do sistema ; se ele tem , vamos devolvê-lo - se não, um novo livro será criado e armazenado de modo que possa ser acessado mais tarde. Isso garante que apenas criar uma única cópia de cada peça intrínseca única de dados :

````js
// Book Factory singleton
var BookFactory = (function () {
  var existingBooks = {}, existingBook;

  return {
    createBook: function ( title, author, genre, pageCount, publisherID, ISBN ) {

      // Find out if a particular book meta-data combination has been created before
      // !! or (bang bang) forces a boolean to be returned
      existingBook = existingBooks[ISBN];
      if ( !!existingBook ) {
        return existingBook;
      } else {

        // if not, let's create a new instance of the book and store it
        var book = new Book( title, author, genre, pageCount, publisherID, ISBN );
        existingBooks[ISBN] = book;
        return book;

      }
    }
  };

})();
````

## Managing the extrinsic states

Em seguida, é necessário armazenar os estados que foram removidos os objetos Book algum lugar - felizmente um gerente (que estaremos definindo como um Singleton ) pode ser utilizado para encapsular -los. Combinações de um objeto Livro e do membro da biblioteca que é verificado-los será chamado registros do livro . Nosso gerente estará armazenando tanto e incluirá também check-out relacionadas lógica nós retirados durante a otimização de peso mosca da classe Book.

````js
// BookRecordManager singleton
var BookRecordManager = (function () {

  var bookRecordDatabase = {};

  return {
    // add a new book into the library system
    addBookRecord: function ( id, title, author, genre, pageCount, publisherID, ISBN, checkoutDate, checkoutMember, dueReturnDate, availability ) {

      var book = bookFactory.createBook( title, author, genre, pageCount, publisherID, ISBN );

      bookRecordDatabase[id] = {
        checkoutMember: checkoutMember,
        checkoutDate: checkoutDate,
        dueReturnDate: dueReturnDate,
        availability: availability,
        book: book
      };
    },
    updateCheckoutStatus: function ( bookID, newStatus, checkoutDate, checkoutMember, newReturnDate ) {

      var record = bookRecordDatabase[bookID];
      record.availability = newStatus;
      record.checkoutDate = checkoutDate;
      record.checkoutMember = checkoutMember;
      record.dueReturnDate = newReturnDate;
    },

    extendCheckoutPeriod: function ( bookID, newReturnDate ) {
      bookRecordDatabase[bookID].dueReturnDate = newReturnDate;
    },

    isPastDue: function ( bookID ) {
      var currentDate = new Date();
      return currentDate.getTime() > Date.parse( bookRecordDatabase[bookID].dueReturnDate );
    }
  };

})();
````

O resultado dessas mudanças é que todos os dados que tem sido extraída da classe livro está agora a ser armazenado em um atributo do solteirão BookManager ( BookDatabase ) - algo consideravelmente mais eficiente do que o grande número de objetos que estávamos usando antes. Métodos relacionados reservar checkouts também se baseiam agora aqui como eles lidam com dados que são extrínsecos ao invés de intrínseco.

Este processo não adicionar uma pequena complexidade para a solução final , no entanto, é uma preocupação pequena quando comparada com os problemas de desempenho que foram abordados . Dados sábio, se temos 30 exemplares do mesmo livro , estamos agora só armazená-lo uma vez. Além disso, cada função tem memória. Com o FLYWEIGHT existem essas funções em um só lugar ( no gerenciador ) e não em cada objeto , poupando assim no uso de memória . Para a versão unoptimized flyweight acima mencionada armazenamos apenas um link para o objeto função que usamos protótipo do Livro construtor , mas se ele foi implementado em outra maneira , as funções seriam criadas para cada instância livro.

## The Flyweight pattern and the DOM

O DOM ( Document Object Model) suporta duas abordagens que permitem que objetos para detectar eventos - tanto de cima para baixo (captura de evento) ou bottom-up (evento borbulhando).

No caso de captura , o primeiro evento é capturado por o elemento mais exterior e propagada para o interior elemento mais . Na subida do evento , o evento é capturado e dado ao elemento mais interna e , em seguida, propagadas para os elementos- exteriores .

Uma das melhores metáforas para descrever flyweights neste contexto foi escrito por Gary Chisholm e vai um pouco assim:

<p>
Tente pensar no peso mosca , em termos de uma lagoa. Um peixe abre a boca (o evento ) , bolhas sobem à superfície ( o borbulhar ) uma mosca sentada no topo voa para longe quando a bolha alcança a superfície (a ação ) . Neste exemplo , podemos facilmente transpor o peixe abrindo sua boca para um botão que está sendo clicado , as bolhas como o efeito borbulhante ea mosca afastado para alguma função que está sendo executado
</p>

Borbulhante foi introduzido para lidar com situações em que um único evento (por exemplo, um clique ) podem ser manipulados por vários manipuladores de eventos definidos em diferentes níveis da hierarquia DOM. Onde isso acontece, evento bolha executa manipuladores de eventos definidos para elementos específicos no nível mais baixo possível. A partir daí , o evento bolhas até que contém elementos antes de ir para os ainda mais para cima.

Contrapesos pode ser usado para ajustar o processo de subida do evento ainda mais , como veremos em breve.

## Exemplo 1: centralizado o tratamento de eventos

Para o nosso primeiro exemplo prático , imaginar que temos um número de elementos semelhantes em um documento com comportamento semelhante executado quando um usuário ação ( por exemplo clique, mouse-over ) é executada contra eles.

Normalmente o que fazemos quando a construção de nosso componente própria acordeão, menu ou outro widget baseado em lista é ligar um evento de clique para cada elemento link no recipiente pai (por exemplo $ ( ' ul li a') . Em (..) . Em vez de ligação do clique para vários elementos , podemos facilmente conectar um Flyweight ao topo do nosso recipiente que possa escutar eventos que vêm de baixo. Estes podem então ser manipulados usando a lógica que é tão simples ou complexo , conforme necessário.

Como os tipos de componentes mencionados muitas vezes têm a mesma marcação de repetição para cada seção (por exemplo, cada seção de um acordeão ) , há uma boa chance de o comportamento de cada elemento que pode ser clicado vai ser bastante semelhante e em relação às classes semelhantes nas proximidades. Vamos usar esta informação para construir um acordeão muito básico usando o Flyweight abaixo.

Um namespace StateManager é usado aqui para encapsular nossa lógica flyweight enquanto jQuery é usado para ligar o clique inicial para uma div container. A fim de assegurar que nenhuma outra lógica na página é fixar pegas semelhantes ao recipiente , um evento de desagregar é aplicado pela primeira vez.

Agora para estabelecer exatamente o elemento filho no recipiente é clicado , fazemos uso de um cheque -alvo que fornece uma referência ao elemento que foi clicado , independentemente de seu pai. Em seguida, usamos essa informação para manipular o evento clique, sem , na verdade, a necessidade de ligar o evento para crianças específicas quando a nossa página é carregada.

````html
<div id="container">
   <div class="toggle" href="#">More Info (Address)
       <span class="info">
           This is more information
       </span></div>
   <div class="toggle" href="#">Even More Info (Map)
       <span class="info">
          <iframe src="http://www.map-generator.net/extmap.php?name=London&amp;address=london%2C%20england&amp;width=500...gt;"</iframe>
       </span>
   </div>
</div>
````

<b>Javascript</b>

````js
var stateManager = {

  fly: function () {

    var self = this;

    $( "#container" )
          .unbind()
          .on( "click", "div.toggle", function ( e ) {
            self.handleClick( e.target );
          });
  },

  handleClick: function ( elem ) {
    elem.find( "span" ).toggle( "slow" );
  }
};
````

A vantagem aqui é que nós estamos convertendo muitas ações independentes numa aqueles compartilhados ( potencialmente salvando na memória ).

## Exemplo 2: Usando o Flyweight para otimização de desempenho

Em nosso segundo exemplo , vamos referenciar alguns novos ganhos de desempenho que podem ser alcançados com contrapesos com jQuery.

James Padolsey anteriormente escreveu um artigo chamado 76 bytes para jQuery mais rápido onde ele nos lembrou que cada vez jQuery dispara uma chamada de retorno , independentemente do tipo (filtro , cada um, manipulador de eventos ) , somos capazes de acessar contexto da função ( o elemento DOM relacionado a ele ), através da palavra-chave.

Infelizmente, muitos de nós tornaram-se acostumar com a idéia de envolver isso em $ () ou jQuery () , o que significa que uma nova instância do jQuery é desnecessariamente construída de cada vez, ao invés de simplesmente fazer isso:

````js
$("div").on( "click", function () {
  console.log( "You clicked: " + $( this ).attr( "id" ));
});

// we should avoid using the DOM element to create a
// jQuery object (with the overhead that comes with it)
// and just use the DOM element itself like this:

$( "div" ).on( "click", function () {
  console.log( "You clicked:"  + this.id );
});
````

James queria usar jQuery.text do jQuery no seguinte contexto, no entanto, ele não concordou com a noção de que um novo objeto jQuery teve que ser criado em cada iteração :

````js
$( "a" ).map( function () {
  return $( this ).text();
});
````

Agora, com relação ao envolvimento redundante, sempre que possível com métodos utilitários do jQuery , é melhor usar jQuery.methodName (por exemplo jQuery.text ) em oposição a jQuery.fn.methodName (por exemplo jQuery.fn.text ) , onde methodName representa um utilitário como o each () ou texto. Isso evita a necessidade de chamar um outro nível de abstração ou construir um novo jQuery objeto cada vez que nossa função é chamado como como jQuery.methodName é o que a própria biblioteca usa em um nível inferior ao jQuery.fn.methodName poder.

Porque no entanto nem todos os métodos do jQuery têm funções de nó simples correspondentes , Padolsey concebeu a idéia de um utilitário jQuery.single.

A ideia aqui é que um único objeto jQuery é criado e usado para cada chamada para jQuery.single (efetivamente o que significa que apenas um objeto jQuery é já criado ) . A implementação para isso pode ser encontrado abaixo e como estamos consolidando dados para vários objetos possíveis em uma estrutura singular mais central , é tecnicamente também um Flyweight.

````js
jQuery.single = (function( o ){

   var collection = jQuery([1]);
   return function( element ) {

       // Give collection the element:
       collection[0] = element;

        // Return the collection:
       return collection;

   };
})();
````

Um exemplo disto em ação com o encadeamento é:

````js
$( "div" ).on( "click", function () {

   var html = jQuery.single( this ).next().html();
   console.log( html );

});
````

Nota: Embora possamos acreditar que simplesmente cache nosso código jQuery pode oferecer apenas como ganhos de desempenho equivalentes , Padolsey afirma que $ .single () ainda vale a pena usar e pode ter um melhor desempenho . Isso não quer dizer que não aplicar qualquer cache em tudo, basta estar consciente de que esta abordagem pode ajudar. Para mais detalhes sobre $ .single , eu recomendo ler o post completo de Padolsey.
