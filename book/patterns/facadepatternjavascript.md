# The Facade Pattern


Quando colocamos uma fachada (facade) , apresentamos uma aparência para o mundo que pode esconder uma realidade muito diferente. Esta foi a inspiração para o nome por trás do padrão - o padrão <b>Facade</b>. Este padrão fornece uma interface de nível superior conveniente para um conjunto maior de código, escondendo sua verdadeira complexidade subjacente. Pense nisso como a simplificação da API que está sendo apresentado para outros desenvolvedores , algo que melhora quase sempre usabilidade.

Fachadas são um padrão estrutural que muitas vezes pode ser visto em bibliotecas JavaScript como jQuery , onde , apesar de uma implementação pode apoiar métodos com uma vasta gama de comportamentos , apenas uma " fachada " ou abstração limitada desses métodos é apresentado ao público para uso.

Isso nos permite interagir com a facade diretamente em vez de o subsistema nos bastidores. Sempre que usamos ````jsCSS $ (el) do jQuery () ou .animate $ (el) () métodos ````, na verdade estamos usando um Facade - a interface pública mais simples que evita que tenhamos que chamar manualmente os vários métodos internos no núcleo jQuery necessários para obter algum trabalho comportamento. Isso também evita a necessidade de interagir manualmente com DOM APIs e manter variáveis ​​de estado.

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

Facade não só tem que ser usado por conta própria, no entanto. Eles também podem ser integrados com outros padrões , tais como o Module Pattern. Como se pode ver a seguir, a exemplo do Module Pattern contém um certo número de métodos que foram definidos em particular . A facade é então usada para fornecer uma API muito mais simples de acesso a esses métodos:

````js
var module = (function() {

    var _private = {
        i: 5,
        get: function() {
            console.log( "current value:" + this.i);
        },
        set: function( val ) {
            this.i = val;
        },
        run: function() {
            console.log( "running" );
        },
        jump: function(){
            console.log( "jumping" );
        }
    };

    return {

        facade: function( args ) {
            _private.set(args.val);
            _private.get();
            if ( args.run ) {
                _private.run();
            }
        }
    };
}());


// Outputs: "current value: 10" and "running"
module.facade( {run: true, val: 10} );
````

Neste exemplo , chamando module.facade () irá realmente desencadear um conjunto de comportamentos privados dentro do módulo , mas, novamente , o usuário não está preocupado com isso. nós criamos uma maneira de consumir um recurso sem a necessidade de se preocupar com detalhes de nível de implementação

## Notas sobre a abstração

Fachadas geralmente têm algumas desvantagens , mas uma preocupação digno de nota é o desempenho. Ou seja, é preciso determinar se há um custo implícito à abstração que uma facade oferece para a nossa aplicação e em caso afirmativo, se esse custo é justificável. Voltando para a biblioteca jQuery, a maioria de nós estamos conscientes de que tanto getElementById ( " identificador" ) e $ ( " # identificador" ) pode ser usado para consultar um elemento em uma página pelo seu ID.

Saiba, porém, que getElementById () no seu próprio é significativamente mais rápido por uma alta ordem de grandeza. Dê uma olhada neste teste JSPerf para ver os resultados em um nível per -browser :

http://jsperf.com/getelementbyid-vs-jquery-id. Agora, é claro , temos que ter em mente que jQuery (e Sizzle - seu motor selector ) estão fazendo muito mais por trás das cenas para otimizar nossa consulta (e que um objeto jQuery , não apenas um nó DOM é retornado) .

O desafio com este Fachada particular é que , a fim de proporcionar uma função de seletor elegante capaz de aceitar e analisar múltiplos tipos de consultas , existe um custo implícito de abstracção . O usuário não é obrigado a acessar jQuery.getById ( " identificador" ) ou jQuery.getByClass ( " identificador" ) e assim por diante . Dito isto, o trade-off no desempenho foi testado na prática ao longo dos anos e dado o sucesso do jQuery, uma simples fachada realmente funcionou muito bem para a equipe.

Ao usar o padrão , tentar estar ciente de quaisquer custos de desempenho envolvidos e fazer uma chamada sobre se eles valem o nível de abstração oferecido.
