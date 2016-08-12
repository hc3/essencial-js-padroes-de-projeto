# The Observer Pattern

O padrão Observer é o padrão onde o objeto ( subject ) mantém uma lista de objetos dependentes ( objservers ), que automaticamente são notificados de qualquer alteração de estado.

Quando o Subject precisa notificar observers sobre algo interessante que está acontecendo, ele transmite uma notificação, para os observadores ( que podem incluir dados específicos relacionados com o tema da notificação ).

Quando o subject não quiser mais ser notificado sobre alguma mudança de um observer em particular então o object é removido da lista que observers do subject.


É frequentemente útil remeter para definições publicadas de padrões de design que são independente de linguagem para ter uma noção  mais ampla de seu uso e as vantagens ao longo do tempo. A definição do padrão observer que está no livro do GOF é :

um ou mais objservers estão interesados no estado de um subject e então registram o interesse nesse subject. Quando algo muda no subject então o observer que está interessado é notificado e é enviado uma atualização para o méotodo de cada observer. Quando o observer não está mais interessado no estado do subject ele pode simplesmente remover esse registro de interesse.

<b>Pegando um gancho de um livro muito bom sobre design patterns o use a cabeça padroes de projeto tem uma alusão muito boa pra usarmos aqui que é o caso do 10 pessoas vão fazer uma entrevista de emprego e após a entrevista a pessoa do RH fica encarregada de ligar pra todos os que fizeram a entrevista para informar se passaram ou não, nesse caso o RH é o subject e os candidatos são observers. </b>

agora podemos expandir sobre o que aprendemos para implementar o padrão Observer com os seguintes componentes :

<ul>
<li><b>Subject:</b> mantém uma lista de observers, facilitando a adição ou remoção dos observers.</li>
<li><b>Observer:</b> prove atualização de interface para objetos que necessitem de ser notificados quando o estado do subject mudar.</li>
<li><b>ConcreteSubject:</b> notificações transmitidas aos observers sobre as mudanças de estados, ficam armazenadas em ConcreteObservers.</li>
<li><b>ConcreteObjserver:</b> armazena uma referência para o ConcreteSubject, implementa uma interface de atualização para o observer
garantindo o estado sempre consistente com o subject.</li>
</ul>

Primeiro, vamos modelar a lista de observadores dependentes um sujeito pode ter :

````js
function ObserverList(){
  this.observerList = [];
}

ObserverList.prototype.add = function( obj ){
  return this.observerList.push( obj );
};

ObserverList.prototype.count = function(){
  return this.observerList.length;
};

ObserverList.prototype.get = function( index ){
  if( index > -1 && index < this.observerList.length ){
    return this.observerList[ index ];
  }
};

ObserverList.prototype.indexOf = function( obj, startIndex ){
  var i = startIndex;

  while( i < this.observerList.length ){
    if( this.observerList[i] === obj ){
      return i;
    }
    i++;
  }

  return -1;
};

ObserverList.prototype.removeAt = function( index ){
  this.observerList.splice( index, 1 );
};
````

Em seguida, vamos modelar o Subject e a capacidade de adicionar, remover ou notificar observadores na lista observador.

````js
function Subject(){
  this.observers = new ObserverList();
}

Subject.prototype.addObserver = function( observer ){
  this.observers.add( observer );
};

Subject.prototype.removeObserver = function( observer ){
  this.observers.removeAt( this.observers.indexOf( observer, 0 ) );
};

Subject.prototype.notify = function( context ){
  var observerCount = this.observers.count();
  for(var i=0; i < observerCount; i++){
    this.observers.get(i).update( context );
  }
};
````

Em seguida, definir um esqueleto para a criação de novos Observers. A funcionalidade de <b>update</b> aqui será substituído mais tarde com comportamento personalizado.

````js
// The Observer
function Observer(){
  this.update = function(){
    // ...
  };
}
````

Em nosso exemplo de aplicativo utilizando os componentes Observer acima, agora vamos definir :

<ul>
<li>Um botão para adicionar novos observers para página.</li>
<li>A caixa de controle que irá funcionar como um assunto , notificando outras caixas de seleção devem ser verificados </li>
<li>Um recipiente para as novas caixas que serão adicionadas</li>
</ul>

Em seguida, definir ConcreteSubject e manipuladores ConcreteObserver tanto para a adição de novos observadores para a página e implementar a interface de atualização. Veja abaixo para comentários na linha sobre o que estes componentes no contexto do nosso exemplo:

<b>HTML:</b>
````html
<button id="addNewObserver">Add New Observer checkbox</button>
<input id="mainCheckbox" type="checkbox"/>
<div id="observersContainer"></div>
````

<b>Sample script:</b>
````js
// Extend an object with an extension
function extend( obj, extension ){
  for ( var key in extension ){
    obj[key] = extension[key];
  }
}

// References to our DOM elements

var controlCheckbox = document.getElementById( "mainCheckbox" ),
  addBtn = document.getElementById( "addNewObserver" ),
  container = document.getElementById( "observersContainer" );


// Concrete Subject

// Extend the controlling checkbox with the Subject class
extend( controlCheckbox, new Subject() );

// Clicking the checkbox will trigger notifications to its observers
controlCheckbox.onclick = function(){
  controlCheckbox.notify( controlCheckbox.checked );
};

addBtn.onclick = addNewObserver;

// Concrete Observer

function addNewObserver(){

  // Create a new checkbox to be added
  var check = document.createElement( "input" );
  check.type = "checkbox";

  // Extend the checkbox with the Observer class
  extend( check, new Observer() );

  // Override with custom update behaviour
  check.update = function( value ){
    this.checked = value;
  };

  // Add the new observer to our list of observers
  // for our main subject
  controlCheckbox.addObserver( check );

  // Append the item to the container
  container.appendChild( check );
}
````

Nesse exemplo, nos vimos como implementar e utilizar o padrao observer usando os conceitos de Subject, Observer, ConcreteSubject e ConcreteObserver.

# FALTA TRADUZIR

## Diferenças entre Observerr e Publish/Subscribe Pattern.

Publish/Subscribe fits in very well in JavaScript ecosystems, largely because at the core, ECMAScript implementations are event driven. This is particularly true in browser environments as the DOM uses events as its main interaction API for scripting.

That said, neither ECMAScript nor DOM provide core objects or methods for creating custom events systems in implementation code (with the exception of perhaps the DOM3 CustomEvent, which is bound to the DOM and is thus not generically useful).

Luckily, popular JavaScript libraries such as dojo, jQuery (custom events) and YUI already have utilities that can assist in easily implementing a Publish/Subscribe system with very little effort. Below we can see some examples of this:

````js
// Publish

// jQuery: $(obj).trigger("channel", [arg1, arg2, arg3]);
$( el ).trigger( "/login", [{username:"test", userData:"test"}] );

// Dojo: dojo.publish("channel", [arg1, arg2, arg3] );
dojo.publish( "/login", [{username:"test", userData:"test"}] );

// YUI: el.publish("channel", [arg1, arg2, arg3]);
el.publish( "/login", {username:"test", userData:"test"} );


// Subscribe

// jQuery: $(obj).on( "channel", [data], fn );
$( el ).on( "/login", function( event ){...} );

// Dojo: dojo.subscribe( "channel", fn);
var handle = dojo.subscribe( "/login", function(data){..} );

// YUI: el.on("channel", handler);
el.on( "/login", function( data ){...} );


// Unsubscribe

// jQuery: $(obj).off( "channel" );
$( el ).off( "/login" );

// Dojo: dojo.unsubscribe( handle );
dojo.unsubscribe( handle );

// YUI: el.detach("channel");
el.detach( "/login" );
````

For those wishing to use the Publish/Subscribe pattern with vanilla JavaScript (or another library) AmplifyJS includes a clean, library-agnostic implementation that can be used with any library or toolkit. Radio.js (http://radio.uxder.com/), PubSubJS (https://github.com/mroderick/PubSubJS) or Pure JS PubSub by Peter Higgins (https://github.com/phiggins42/bloody-jquery-plugins/blob/55e41df9bf08f42378bb08b93efcb28555b61aeb/pubsub.js) are also similar alternatives worth checking out.

jQuery developers in particular have quite a few other options and can opt to use one of the many well-developed implementations ranging from Peter Higgins's jQuery plugin to Ben Alman's (optimized) Pub/Sub jQuery gist on GitHub. Links to just a few of these can be found below.

<ul>
  <li>Ben Alman's Pub/Sub gist https://gist.github.com/661855 (recommended)</li>
  <li>Rick Waldron's jQuery-core style take on the above https://gist.github.com/705311</li>
  <li>Peter Higgins" plugin http://github.com/phiggins42/bloody-jquery-plugins/blob/master/pubsub.js.</li>
  <li>AppendTo's Pub/Sub in AmplifyJS http://amplifyjs.com</li>
  <li>Ben Truyman's gist https://gist.github.com/826794</li>
</ul>

So that we are able to get an appreciation for how many of the vanilla JavaScript implementations of the Observer pattern might work, let's take a walk through of a minimalist version of Publish/Subscribe I released on GitHub under a project called pubsubz. This demonstrates the core concepts of subscribe, publish as well as the concept of unsubscribing.

I've opted to base our examples on this code as it sticks closely to both the method signatures and approach of implementation I would expect to see in a JavaScript version of the classic Observer pattern.

### A Publish/Subscribe Implementation

````js
var pubsub = {};

(function(myObject) {

    // Storage for topics that can be broadcast
    // or listened to
    var topics = {};

    // An topic identifier
    var subUid = -1;

    // Publish or broadcast events of interest
    // with a specific topic name and arguments
    // such as the data to pass along
    myObject.publish = function( topic, args ) {

        if ( !topics[topic] ) {
            return false;
        }

        var subscribers = topics[topic],
            len = subscribers ? subscribers.length : 0;

        while (len--) {
            subscribers[len].func( topic, args );
        }

        return this;
    };

    // Subscribe to events of interest
    // with a specific topic name and a
    // callback function, to be executed
    // when the topic/event is observed
    myObject.subscribe = function( topic, func ) {

        if (!topics[topic]) {
            topics[topic] = [];
        }

        var token = ( ++subUid ).toString();
        topics[topic].push({
            token: token,
            func: func
        });
        return token;
    };

    // Unsubscribe from a specific
    // topic, based on a tokenized reference
    // to the subscription
    myObject.unsubscribe = function( token ) {
        for ( var m in topics ) {
            if ( topics[m] ) {
                for ( var i = 0, j = topics[m].length; i < j; i++ ) {
                    if ( topics[m][i].token === token ) {
                        topics[m].splice( i, 1 );
                        return token;
                    }
                }
            }
        }
        return this;
    };
}( pubsub ));
````

### Example: Using Our Implementation

We can now use the implementation to publish and subscribe to events of interest as follows:

````js
// Another simple message handler

// A simple message logger that logs any topics and data received through our
// subscriber
var messageLogger = function ( topics, data ) {
    console.log( "Logging: " + topics + ": " + data );
};

// Subscribers listen for topics they have subscribed to and
// invoke a callback function (e.g messageLogger) once a new
// notification is broadcast on that topic
var subscription = pubsub.subscribe( "inbox/newMessage", messageLogger );

// Publishers are in charge of publishing topics or notifications of
// interest to the application. e.g:

pubsub.publish( "inbox/newMessage", "hello world!" );

// or
pubsub.publish( "inbox/newMessage", ["test", "a", "b", "c"] );

// or
pubsub.publish( "inbox/newMessage", {
  sender: "hello@google.com",
  body: "Hey again!"
});

// We can also unsubscribe if we no longer wish for our subscribers
// to be notified
pubsub.unsubscribe( subscription );

// Once unsubscribed, this for example won't result in our
// messageLogger being executed as the subscriber is
// no longer listening
pubsub.publish( "inbox/newMessage", "Hello! are you still there?" );
````

#### Example: User-Interface Notifications

Next, let's imagine we have a web application responsible for displaying real-time stock information.

The application might have a grid for displaying the stock stats and a counter for displaying the last point of update. When the data model changes, the application will need to update the grid and counter. In this scenario, our subject (which will be publishing topics/notifications) is the data model and our subscribers are the grid and counter.

When our subscribers receive notification that the model itself has changed, they can update themselves accordingly.

In our implementation, our subscriber will listen to the topic "newDataAvailable" to find out if new stock information is available. If a new notification is published to this topic, it will trigger gridUpdate to add a new row to our grid containing this information. It will also update a last updated counter to log the last time data was added

````js
// Return the current local time to be used in our UI later
getCurrentTime = function (){

   var date = new Date(),
         m = date.getMonth() + 1,
         d = date.getDate(),
         y = date.getFullYear(),
         t = date.toLocaleTimeString().toLowerCase();

        return (m + "/" + d + "/" + y + " " + t);
};

// Add a new row of data to our fictional grid component
function addGridRow( data ) {

   // ui.grid.addRow( data );
   console.log( "updated grid component with:" + data );

}

// Update our fictional grid to show the time it was last
// updated
function updateCounter( data ) {

   // ui.grid.updateLastChanged( getCurrentTime() );
   console.log( "data last updated at: " + getCurrentTime() + " with " + data);

}

// Update the grid using the data passed to our subscribers
gridUpdate = function( topic, data ){

  if ( data !== undefined ) {
     addGridRow( data );
     updateCounter( data );
   }

};

// Create a subscription to the newDataAvailable topic
var subscriber = pubsub.subscribe( "newDataAvailable", gridUpdate );

// The following represents updates to our data layer. This could be
// powered by ajax requests which broadcast that new data is available
// to the rest of the application.

// Publish changes to the gridUpdated topic representing new entries
pubsub.publish( "newDataAvailable", {
  summary: "Apple made $5 billion",
  identifier: "APPL",
  stockPrice: 570.91
});

pubsub.publish( "newDataAvailable", {
  summary: "Microsoft made $20 million",
  identifier: "MSFT",
  stockPrice: 30.85
});
````

#### Example: Decoupling applications using Ben Alman's Pub/Sub implementation

In the following movie ratings example, we'll be using Ben Alman's jQuery implementation of Publish/Subscribe to demonstrate how we can decouple a user interface. Notice how submitting a rating only has the effect of publishing the fact that new user and rating data is available.

It's left up to the subscribers to those topics to then delegate what happens with that data. In our case we're pushing that new data into existing arrays and then rendering them using the Underscore library's .template() method for templating.

<b>HTML/Templates</b>

````HTML
<script id="userTemplate" type="text/html">
   <li><%= name="" %=""></li>
</script>


<script id="ratingsTemplate" type="text/html">
   <li><strong><%= %=""></strong> was rated <%= rating="" %="">/5</li>
</script>


<div id="container">

   <div class="sampleForm">
       <p>
           <label for="twitter_handle">Twitter handle:</label>
           <input type="text" id="twitter_handle" />
       </p>
       <p>
           <label for="movie_seen">Name a movie you've seen this year:</label>
           <input type="text" id="movie_seen" />
       </p>
       <p>

           <label for="movie_rating">Rate the movie you saw:</label>
           <select id="movie_rating">
                 <option value="1">1</option>
                  <option value="2">2</option>
                  <option value="3">3</option>
                  <option value="4">4</option>
                  <option value="5" selected>5</option>

          </select>
        </p>
        <p>

            <button id="add">Submit rating</button>
        </p>
    </div>



    <div class="summaryTable">
        <div id="users"><h3>Recent users</h3></div>
        <div id="ratings"><h3>Recent movies rated</h3></div>
    </div>

 </div>

<!--%=--><!--%=--><!--%=-->
````

<b>JavaScript</b>

````js
(function( $ ) {

  // Pre-compile templates and "cache" them using closure
  var
    userTemplate = _.template($( "#userTemplate" ).html()),
    ratingsTemplate = _.template($( "#ratingsTemplate" ).html());

  // Subscribe to the new user topic, which adds a user
  // to a list of users who have submitted reviews
  $.subscribe( "/new/user", function( e, data ){

    if( data ){

      $('#users').append( userTemplate( data ));

    }

  });

  // Subscribe to the new rating topic. This is composed of a title and
  // rating. New ratings are appended to a running list of added user
  // ratings.
  $.subscribe( "/new/rating", function( e, data ){

    if( data ){

      $( "#ratings" ).append( ratingsTemplate( data ) );

    }

  });

  // Handler for adding a new user
  $("#add").on("click", function( e ) {

    e.preventDefault();

    var strUser = $("#twitter_handle").val(),
       strMovie = $("#movie_seen").val(),
       strRating = $("#movie_rating").val();

    // Inform the application a new user is available
    $.publish( "/new/user", { name: strUser } );

    // Inform the app a new rating is available
    $.publish( "/new/rating", { title: strMovie, rating: strRating} );

    });

})( jQuery );
````

#### Example: Decoupling an Ajax-based jQuery application

In our final example, we're going to take a practical look at how decoupling our code using Pub/Sub early on in the development process can save us some potentially painful refactoring later on.

Quite often in Ajax-heavy applications, once we've received a response to a request we want to achieve more than just one unique action. One could simply add all of their post-request logic into a success callback, but there are drawbacks to this approach.

Highly coupled applications sometimes increase the effort required to reuse functionality due to the increased inter-function/code dependency. What this means is that although keeping our post-request logic hardcoded in a callback might be fine if we're just trying to grab a result set once, it's not as appropriate when we want to make further Ajax-calls to the same data source (and different end-behavior) without rewriting parts of the code multiple times. Rather than having to go back through each layer that calls the same data-source and generalizing them later on, we can use pub/sub from the start and save time.

Using Observers, we can also easily separate application-wide notifications regarding different events down to whatever level of granularity we're comfortable with - something which can be less elegantly done using other patterns.

Notice how in our sample below, one topic notification is made when a user indicates they want to make a search query and another is made when the request returns and actual data is available for consumption. It's left up to the subscribers to then decide how to use knowledge of these events (or the data returned). The benefits of this are that, if we wanted, we could have 10 different subscribers utilizing the data returned in different ways but as far as the Ajax-layer is concerned, it doesn't care. Its sole duty is to request and return data then pass it on to whoever wants to use it. This separation of concerns can make the overall design of our code a little cleaner.

<b>HTML/Templates:</b>

````HTML
<form id="flickrSearch">

   <input type="text" name="tag" id="query"/>

   <input type="submit" name="submit" value="submit"/>

</form>



<div id="lastQuery"></div>

<ol id="searchResults"></ol>



<script id="resultTemplate" type="text/html">
    <% _.each(items,="" function(="" item="" ){="" %="">
        <li><img src="<%= item.media.m="" %="">"/></li>
    <% });%="">
</script>

<!--%--><!--%=--><!--%-->
````

<b>JavaScript:</b>

````js
(function( $ ) {

   // Pre-compile template and "cache" it using closure
   var resultTemplate = _.template($( "#resultTemplate" ).html());

   // Subscribe to the new search tags topic
   $.subscribe( "/search/tags", function( e, tags ) {
       $( "#lastQuery" )
                .html("<p>Searched for:<strong>" + tags + "</strong></p>");
   });

   // Subscribe to the new results topic
   $.subscribe( "/search/resultSet", function( e, results ){

       $( "#searchResults" ).empty().append(resultTemplate( results ));

   });

   // Submit a search query and publish tags on the /search/tags topic
   $( "#flickrSearch" ).submit( function( e ) {

       e.preventDefault();
       var tags = $(this).find( "#query").val();

       if ( !tags ){
        return;
       }

       $.publish( "/search/tags", [ $.trim(tags) ]);

   });


   // Subscribe to new tags being published and perform
   // a search query using them. Once data has returned
   // publish this data for the rest of the application
   // to consume

   $.subscribe("/search/tags", function( e, tags ) {

       $.getJSON( "http://api.flickr.com/services/feeds/photos_public.gne?jsoncallback=?", {
              tags: tags,
              tagmode: "any",
              format: "json"
            },

          function( data ){

              if( !data.items.length ) {
                return;
              }

              $.publish( "/search/resultSet", { items: data.items } );
       });

   });


})( jQuery );
````

The Observer pattern is useful for decoupling a number of different scenarios in application design and if you haven't been using it, I recommend picking up one of the pre-written implementations mentioned today and just giving it a try out. It's one of the easier design patterns to get started with but also one of the most powerful.
