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
