# The Facade Pattern


Quando colocamos uma fachada (facade) , apresentamos uma aparência para o mundo que pode esconder uma realidade muito diferente. Esta foi a inspiração para o nome por trás do padrão - o padrão <b>Facade</b>. Este padrão fornece uma interface de nível superior conveniente para um conjunto maior de código, escondendo sua verdadeira complexidade subjacente. Pense nisso como a simplificação da API que está sendo apresentado para outros desenvolvedores , algo que melhora quase sempre usabilidade.

Fachadas são um padrão estrutural que muitas vezes pode ser visto em bibliotecas JavaScript como jQuery , onde , apesar de uma implementação pode apoiar métodos com uma vasta gama de comportamentos , apenas uma " fachada " ou abstração limitada desses métodos é apresentado ao público para uso.

Isso nos permite interagir com a facade diretamente em vez de o subsistema nos bastidores. Sempre que usamos CSS $ (el) do jQuery () ou .animate $ (el) () métodos , na verdade estamos usando um Facade - a interface pública mais simples que evita que tenhamos que chamar manualmente os vários métodos internos no núcleo jQuery necessários para obter algum trabalho comportamento. Isso também evita a necessidade de interagir manualmente com DOM APIs e manter variáveis ​​de estado.

O core do jQuery pode ser considerado um intermediário da abstração. esse fato torna o jQuery tão fácil de usar.

Para construir sobre o que aprendemos , o padrão Facade tanto simplifica a interface de uma classe e também separa a classe a partir do código que utiliza -lo. Isso nos dá a capacidade de interagir indiretamente com os subsistemas de uma forma que às vezes pode ser menos propenso a erros do que acessar o subsistema diretamente. vantagens de um Facade incluem a facilidade de uso e muitas vezes um tamanho pequeno - pegada na implementação do padrão.

Vamos dar uma olhada no padrão em ação. Este é um exemplo de código unoptimized , mas aqui estamos utilizando uma fachada para simplificar uma interface para ouvir eventos cross- browser. Fazemos isso através da criação de um método comum que pode ser usado no código de um que faz a tarefa de verificar a existência de recursos para que ele possa fornecer uma solução segura e cross-browser compatível.

````js
var addMyEvent = function( el,ev,fn ){

   if( el.addEventListener ){
            el.addEventListener( ev,fn, false );
      }else if(el.attachEvent){
            el.attachEvent( "on" + ev, fn );
      } else{
           el["on" + ev] = fn;
    }

};
````

De uma maneira similar , estamos todos familiarizados com $ ( document) .ready do jQuery (..) . Internamente , este está efectivamente a ser alimentado por um método chamado bindReady ( ) , que está a fazer o seguinte:

````js
bindReady: function() {
    ...
    if ( document.addEventListener ) {
      // Use the handy event callback
      document.addEventListener( "DOMContentLoaded", DOMContentLoaded, false );

      // A fallback to window.onload, that will always work
      window.addEventListener( "load", jQuery.ready, false );

    // If IE event model is used
    } else if ( document.attachEvent ) {

      document.attachEvent( "onreadystatechange", DOMContentLoaded );

      // A fallback to window.onload, that will always work
      window.attachEvent( "onload", jQuery.ready );
               ...

````

Este é outro exemplo de uma fachada , onde o resto do mundo simplesmente usa a interface limitada exposto por $ (document) .ready ( .. ) ea implementação mais complexo alimentar é mantido escondido da vista .
