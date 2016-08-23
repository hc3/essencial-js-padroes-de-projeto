# MVVM

MVVM ( Model View ViewModel ) é um padrão de arquitetura baseada em MVC e MVP , que tenta separar mais claramente o desenvolvimento de interfaces de usuário (UI) do da lógica de negócios e comportamento em um aplicativo. Para este fim , muitas implementações deste teste padrão fazem uso de ligações de dados declarativa para permitir uma separação de trabalho em Vistas de outras camadas.

Isso facilita a interface do usuário e trabalho de desenvolvimento que ocorre quase simultaneamente dentro da mesma base de código. desenvolvedores UI escrever ligações para o ViewModel dentro de sua marcação de documentos (HTML), onde o modelo e ViewModel são mantidos pelos desenvolvedores que trabalham na lógica do aplicativo.

## História

MVVM ( pelo nome) foi inicialmente definido pela Microsoft para uso com o Windows Presentation Foundation (WPF) e Silverlight, tendo sido anunciada oficialmente em 2005 por John Grossman em um post no blog sobre Avalon ( o codinome para WPF) . Ele também encontrou alguma popularidade na comunidade Adobe Flex como uma alternativa para simplesmente usando MVC.

Antes da Microsoft adotando o nome MVVM , houve no entanto um movimento na comunidade para ir de MVP para MVPM : Model View PresentationModel . Martin Fowler escreveu um artigo sobre PresentationModels em 2004 para os interessados ​​em ler mais sobre o assunto. A ideia de um PresentationModel tinha sido em torno de muito mais tempo do que este artigo , no entanto, ele foi considerado o grande quebra na ideia e muito ajudou a popularizar -lo.

Houve bastante alvoroço nos círculos " ALT.NET " depois que a Microsoft anunciou MVVM como uma alternativa para MVPM . Muitos reivindicavam o domínio da empresa no mundo dos GUI foi dando-lhes a oportunidade de assumir a comunidade como um todo , renomeando conceitos existentes como quisessem para fins de marketing . Uma multidão progressiva reconheceu que, embora MVVM e MVPM foram efetivamente a mesma idéia, eles vieram em pacotes ligeiramente diferentes.

Nos últimos anos, MVVM foi implementado em JavaScript na forma de quadros estruturais, tais como KnockoutJS , Kendo MVVM e Knockback.js , com uma resposta global positiva por parte da comunidade .

Vamos agora analisar os três componentes que compõem MVVM .

## Model

Tal como com outros membros da família MV * , o modelo em MVVM representa dados ou informações que nossa aplicação estará trabalhando com específicas de domínio . Um exemplo típico de dados específicos do domínio pode ser uma conta de usuário (por exemplo, nome, avatar, e- mail) ou uma faixa de música (por exemplo, título, ano , álbum ) .

Modelos armazenar informações , mas normalmente não lidar com o comportamento . Eles não formatar informações ou influência como os dados aparece no navegador como este não é sua responsabilidade. Em vez disso, a formatação dos dados é tratada pelo Vista, enquanto o comportamento é considerado lógica de negócios que deve ser encapsulado em uma outra camada que interage com o modelo - o ViewModel.

A única exceção a essa regra tende a ser de validação e é considerado aceitável para Modelos para validar dados que estão sendo usados ​​para definir ou atualizar os modelos existentes (por exemplo, se um e -mail de entrada de endereço sendo satisfazer as exigências de uma expressão regular particular? ) .

Em KnockoutJS , Modelos abrangidos pela definição acima, mas muitas vezes fazem Ajax chama a um serviço do lado do servidor para ler e gravar dados do modelo .

Se estivéssemos construindo um aplicativo Todo simples, um modelo KnockoutJS que representa um único item Todo poderia olhar como se segue:

````js
var Todo = function ( content, done ) {
    this.content = ko.observable(content);
    this.done = ko.observable(done);
    this.editing = ko.observable(false);
};
````

Nota: Pode-se notar no trecho acima que estamos chamando o método observável () no KnockoutJS namespace ko . Em KnockoutJS , observáveis ​​são objetos especiais de JavaScript que podem notificar os assinantes sobre as mudanças e automaticamente detectar dependências. Isto permite-nos para sincronizar Modelos e ViewModels quando o valor de um atributo Modelo é modificado .

## View

Tal como acontece com MVC , a vista é a única parte do aplicativo que os usuários realmente interagir com . Eles são uma interface interativa que representa o estado de um ViewModel . Neste sentido , a visão é considerada ativa em vez de passiva, mas isso também é verdade para os pontos de vista em MVC e MVP . No MVC, MVP e MVVM vista também pode ser passiva , mas o que isso significa ?

A View passiva somente mostra uma exibição e não aceita qualquer entrada do usuário .

Tal visão também podem ter nenhum conhecimento real dos modelos em nossa aplicação e pode ser manipulado por um apresentador . de MVVM Active View contém os dados - ligações , eventos e comportamentos que requer uma compreensão do ViewModel . Embora esses comportamentos podem ser mapeadas para propriedades , a vista é ainda responsável pelo tratamento de eventos do ViewModel .

É importante lembrar o Vista não é responsável aqui para manipulação estado - ele mantém isso em sincronia com o ViewModel .

A KnockoutJS vista é simplesmente um documento HTML com ligações declarativas para vinculá-lo ao ViewModel . KnockoutJS Visualizações exibir informações do ViewModel , passar comandos para ele (por exemplo um usuário clicar em um elemento ) e atualização como o estado das mudanças ViewModel . Modelos gerando marcação usando dados de ViewModel pode no entanto também ser utilizado para esta finalidade .

Para dar um breve exemplo inicial, podemos olhar para os KnockoutJS quadro JavaScript MVVM para a forma como ele permite a definição de um ViewModel e suas ligações relacionados na marcação :

ViewModel:
````js
var aViewModel = {
    contactName: ko.observable("John")
};
ko.applyBindings(aViewModel);
````
view:
````HTML
<p><input id="source" data-bind="value: contactName, valueUpdate: 'keyup'" /></p>
<div data-bind="visible: contactName().length > 10">
    You have a really long name!
</div>
<p>Contact name: <strong data-bind="text: contactName"></strong></p>

````

Nosso texto - caixa de entrada ( fonte ) obtém seu valor inicial a partir contactName , atualizando automaticamente este valor sempre que contactName mudanças . Como a ligação de dados é bidirecional , digitando em texto - caixa irá atualizar contactName nesse sentido, para os valores são sempre em sincronia.

Embora a implementação específica para KnockoutJS , o < div> contendo o "Você tem um nome muito longo ! " texto também contém validação simples (mais uma vez sob a forma de ligações de dados ) . Se a entrada exceder 10 caracteres , será exibido , caso contrário, ele permanecerá oculto.

Passando para um exemplo mais avançado , podemos voltar ao nosso aplicativo Todo . A aparadas para baixo KnockoutJS Ver para isso, incluindo todos os dados - ligações necessárias podem olhar como se segue.

````HTML
<div id="todoapp">
    <header>
        <h1>Todos</h1>
        <input id="new-todo" type="text" data-bind="value: current, valueUpdate: 'afterkeydown', enterKey: add"
               placeholder="What needs to be done?"/>
    </header>
    <section id="main" data-bind="block: todos().length">

        <input id="toggle-all" type="checkbox" data-bind="checked: allCompleted">
        <label for="toggle-all">Mark all as complete</label>

        <ul id="todo-list" data-bind="foreach: todos">

           <!-- item -->
            <li data-bind="css: { done: done, editing: editing }">
                <div class="view" data-bind="event: { dblclick: $root.editItem }">
                    <input class="toggle" type="checkbox" data-bind="checked: done">
                    <label data-bind="text: content"></label>
                    <a class="destroy" href="#" data-bind="click: $root.remove"></a>
                </div>
                <input class="edit' type="text"
                       data-bind="value: content, valueUpdate: 'afterkeydown', enterKey: $root.stopEditing, selectAndFocus: editing, event: { blur: $root.stopEditing }"/>"""
            </li>

        </ul>

    </section>
</div>
````

Note-se que o layout básico do mark-up é relativamente simples , contendo uma caixa de texto de entrada ( nova -todo ) para a adição de novos itens, de alternância para marcar itens como ( - lista de tarefas ) completo e uma lista com um modelo para um Todo produto na forma de um li .

As ligações de dados na marcação acima pode ser dividido da seguinte forma:
<ul>
  <li>A entrada de caixa de texto nova -todo tem um - ligação de dados para a propriedade atual, que é onde o valor do item atual que está sendo adicionado é armazenado. Nossa ViewModel (mostrado em breve) observa a propriedade atual e também tem uma ligação contra o evento add. Quando a tecla Enter é pressionada , o evento add é acionado eo nosso ViewModel pode então cortar o valor do atual e adicioná-lo à lista Todo conforme necessário</li>
  <li>A caixa de entrada de alternância -all pode marcar todos os itens atuais como concluída se clicado . Se marcado , ele aciona o evento allCompleted , que pode ser visto no nosso ViewModel</li>
  <li>O li item tem a classe feito. Quando uma tarefa é marcada como concluída , a edição de classe CSS é marcada em conformidade. Se clicar duas vezes sobre o item, o retorno de chamada $ root.editItem será executado</li>
  <li>A caixa de seleção com a alternância de classes mostra o estado da propriedade feito</li>
  <li>Um rótulo contém o valor de texto do item Todo (conteúdo)</li>
  <li>Há também um botão Remover que irá chamar o callback root.remove $ quando clicado.</li>
  <li>Uma caixa de texto de entrada utilizado para o modo de edição também contém o valor do conteúdo do item Todo . O evento enterkey irá definir a propriedade de edição para verdadeiro ou falso</li>
</ul>

## ViewModel

O ViewModel pode ser considerado um controlador especializado que actua como um conversor de dados . Ele muda as informações modelo para visualização de informações, passando comandos do Vista para o modelo .

Por exemplo , vamos imaginar que temos um modelo que contenha um atributo data no formato unix (por exemplo 1333832407 ) . Ao invés de nossos modelos de estar ciente de vista de um usuário da data (por exemplo, 04/07/2012 @ 17:00 ), onde seria necessário para converter o atributo ao seu formato de exibição , o nosso modelo simplesmente mantém o formato bruto dos dados . Nossa Ver contém a data formatada e nossa ViewModel funciona como um meio-homem entre os dois.

Neste sentido , o ViewModel pode ser encarado como mais de um modelo de vista, mas ele lida com a maioria de lógica de exibição do Vista . O ViewModel também pode expor métodos para ajudar a manter o estado do Vista, atualizar o modelo com base na ação é de uma visão e de gatilho eventos na View.

Em resumo, o ViewModel se senta atrás de nossa camada de interface do usuário. Ela expõe dados necessários para a View ( a partir de um modelo ) e pode ser visto como a fonte de nossos pontos de vista ir para ambos os dados e ações.

KnockoutJS interpreta o ViewModel como a representação de dados e operações que podem ser executadas em uma UI. Esta não é a própria IU , nem o modelo de dados que persiste , mas sim uma camada que também pode armazenar os dados ainda não foram salvos de um utilizador está a trabalhar. ViewModels de Bonito são implementadas objetos JavaScript sem nenhum conhecimento de marcação HTML . Esta abordagem abstrata para a sua implementação lhes permite permanecer simples , o que significa um comportamento mais complexo pode ser mais facilmente gerenciados on-top , conforme necessário.

A KnockoutJS ViewModel parcial para o nosso aplicativo Todo poderia, assim, ter a seguinte aparência :

````js
// our main ViewModel
    var ViewModel = function ( todos ) {
        var self = this;

    // map array of passed in todos to an observableArray of Todo objects
    self.todos = ko.observableArray(
    ko.utils.arrayMap( todos, function ( todo ) {
        return new Todo( todo.content, todo.done );
    }));

    // store the new todo value being entered
    self.current = ko.observable();

    // add a new todo, when enter key is pressed
    self.add = function ( data, event ) {
        var newTodo, current = self.current().trim();
        if ( current ) {
            newTodo = new Todo( current );
            self.todos.push( newTodo );
            self.current("");
        }
    };

    // remove a single todo
    self.remove = function ( todo ) {
        self.todos.remove( todo );
    };

    // remove all completed todos
    self.removeCompleted = function () {
        self.todos.remove(function (todo) {
            return todo.done();
        });
    };

    // writeable computed observable to handle marking all complete/incomplete
    self.allCompleted = ko.computed({

        // always return true/false based on the done flag of all todos
        read:function () {
            return !self.remainingCount();
        },

        // set all todos to the written value (true/false)
        write:function ( newValue ) {
            ko.utils.arrayForEach( self.todos(), function ( todo ) {
                //set even if value is the same, as subscribers are not notified in that case
                todo.done( newValue );
            });
        }
    });

    // edit an item
    self.editItem = function( item ) {
        item.editing( true );
    };
 ..
````

Acima nós estamos fornecendo basicamente os métodos necessários para adicionar, editar ou remover itens , bem como a lógica para marcar todos os itens restantes como tendo sido concluída Nota : A única diferença real digno de nota a partir de exemplos anteriores em nossas ViewModel são matrizes observáveis. Em KnockoutJS , se quisermos detectar e responder a alterações em um único objeto , usaríamos observáveis ​​. Se, no entanto , queremos detectar e responder a alterações de uma coleção de coisas , podemos usar uma observableArray vez . Um exemplo simples de como usar observáveis ​​matrizes pode parecer da seguinte forma:

````js
// Define an initially an empty array
var myObservableArray = ko.observableArray();

// Add a value to the array and notify our observers
myObservableArray.push( 'A new todo item' );
````

Nota: O aplicativo Knockout.js Todo completa analisamos acima pode ser agarrado por TodoMVC se interessado.

## Recap : The View e ViewModel

Pontos de vista e ViewModels comunicam através de data- ligações e eventos. Como vimos em nosso exemplo inicial ViewModel , o ViewModel não apenas expor os atributos do modelo , mas também o acesso a outros métodos e recursos como validação.

Nossos pontos de vista lidar com seus próprios eventos de interface do usuário , mapeando -os para o ViewModel conforme necessário. Modelos e atributos no ViewModel são sincronizados e atualizados através de duas vias de ligação de dados .

Gatilhos ( -disparadores de dados ) também nos permitem reagir ainda mais a mudanças no estado de nossos atributos modelo .

## Recap : O ViewModel eo Modelo

Recap : O ViewModel eo ModelWhilst ele pode aparecer o ViewModel é completamente responsável pelo Modelo de MVVM , existem algumas sutilezas com este relacionamento digno de nota. O ViewModel pode expor um modelo ou atributos do modelo para fins de de ligação de dados e pode também conter interfaces para buscar e manipular propriedades expostas na vista.

## Pros and Cons

Nós agora esperamos ter uma melhor apreciação para o que MVVM é e como funciona . Vamos agora analisar as vantagens e desvantagens de empregar o padrão:

## vantagens
<ul>
  <li>MVVM Facilita desenvolvimento paralelo mais fácil de um interface do usuário e os blocos de construção de que o poder que</li>
  <li>Abstrai a vista e , portanto, reduz a quantidade de lógica de negócios (ou cola) exigida no código por trás dele</li>
  <li>O ViewModel pode ser mais fácil de teste de unidade do que o código orientado a eventos</li>
  <li>O ViewModel ( sendo mais Modelo de Vista ) podem ser testados sem preocupações de automação de interface do usuário e interação</li>
</ul>
## desvantagens
<ul>
  <li>Para UIs mais simples, MVVM pode ser um exagero</li>
  <li>Enquanto data- ligações pode ser declarativa e agradável para trabalhar , eles podem ser mais difíceis de depurar de código imperativo que nós simplesmente definir pontos de interrupção</li>
  <li>Data- ligações em aplicações não - triviais podem criar um monte de contabilidade . Nós também não quer acabar em uma situação onde as ligações são mais pesados ​​do que os objetos que estão sendo obrigados a</li>
  <li>Em aplicações maiores , pode ser mais difícil de projetar o ViewModel na frente para obter a quantidade necessária de generalização</li>
</ul>

## MVVM With Looser Data-Bindings

Não é incomum para os desenvolvedores JavaScript de um fundo MVC ou MVP a avaliar MVVM e queixam-se sua verdadeira separação de interesses . Ou seja , a quantidade de - ligações de dados em linha mantida na marcação HTML de uma vista .

Devo admitir que quando eu primeira avaliação implementações de MVVM (por exemplo KnockoutJS , Knockback ) , fiquei surpreso que qualquer desenvolvedor iria querer voltar para os dias de idade onde nós misturados lógica (JavaScript) com a nossa marcação e encontrou rapidamente insustentável . A realidade , porém, é que MVVM faz isso por uma série de boas razões (que nós cobrimos ), incluindo facilitando designers para ligar-se mais facilmente à lógica de sua marcação.

Para os puristas entre nós , você vai ficar feliz em saber que agora podemos também reduzir significativamente o quão dependentes somos on- ligações de dados graças a um recurso conhecido como prestadores de ligação personalizada , introduzido em KnockoutJS 1.3 e está disponível em todas as versões desde então.

KnockoutJS por padrão tem um fornecedor de ligação de dados que procura por quaisquer elementos com dados -bind atributos sobre eles , como no exemplo abaixo .

````HTML
<input id="new-todo" type="text" data-bind="value: current, valueUpdate: 'afterkeydown', enterKey: add" placeholder="What needs to be done?"/>
````

Quando o provedor localiza um elemento com esse atributo, ele analisa -lo e transforma-lo em um objeto de ligação usando o contexto de dados atual. Esta é a maneira KnockoutJS sempre trabalhou , o que nos permite adicionar declarativamente ligações a elementos que KnockoutJS se liga aos dados nessa camada.

Assim que começar a construir Visualizações que não são mais trivial, que pode acabar com um grande número de elementos e atributos cujas ligações na marcação pode tornar-se difícil de gerir. Com provedores de ligação personalizada no entanto, isso não é mais um problema.

Um provedor de ligação está interessado principalmente em duas coisas :
<ul>
  <li>Quando dada um nó DOM , não contém quaisquer - ligações de dados ?</li>
  <li>Se o nó passou esta primeira pergunta, o que é que o objeto de ligação semelhante no contexto de dados atual?</li>
</ul>

Binding prestadores de implementar duas funções :
<ul>
  <li><b>nodeHasBindings :</b> retoma em um nó DOM que não tem necessariamente de ser um elemento</li>
  <li><b>getBindings :</b> devolve um objecto que representa as ligações , tal como aplicado para o contexto de dados actual</li>
</ul>

Um provedor de ligação esqueleto poderia, assim, ter a seguinte aparência :

````js
var ourBindingProvider = {
  nodeHasBindings: function( node ) {
      // returns true/false
  },

  getBindings: function( node, bindingContext ) {
      // returns a binding object
  }
};
````

Antes de chegarmos a dar conteúdo a este provedor , vamos discutir brevemente a lógica em atributos de dados -bind .

Se ao usar MVVM do Knockout nós encontrar-se insatisfeito com a ideia de lógica do aplicativo que está sendo excessivamente amarrado em sua opinião, nós podemos mudar isso. Poderíamos implementar algo um pouco como classes CSS para atribuir ligações por nome a elementos . Ryan Niemeyer ( de knockmeout.net ) sugeriu anteriormente usando - classe de dados para esta para evitar aulas de apresentação confusos com classes de dados , assim que vamos começar a nossa função nodeHasBindings apoiar este :

````js
// does an element have any bindings?
function nodeHasBindings( node ) {
    return node.getAttribute ? node.getAttribute("data-class") : false;
};
````

Em seguida, temos uma função getBindings(). Como vamos ficar com a ideia de classes CSS , por que não também considerar o apoio aulas separados por espaços para nos permitir compartilhar especificações de ligação entre os diferentes elementos ?

Vamos primeiro comentário que nossos ligações será semelhante. Nós criamos um objeto para mantê-los onde os nossos nomes de propriedades precisam corresponder as chaves que deseja usar em nossas classes de dados.

Nota: Não há uma grande quantidade de trabalho necessário para converter uma aplicação KnockoutJS de usar - ligações de dados tradicionais sobre a ligações discretas com prestadores de ligação personalizada . Nós simplesmente puxar o nosso todos os nossos dados -bind atributos , substituí-los com atributos de classe de dados e colocar nossas ligações em um objeto de ligação conforme a seguir:

````js
var viewModel = new ViewModel( todos || [] ),
    bindings = {

        newTodo: {
            value: viewModel.current,
            valueUpdate: "afterkeydown",
            enterKey: viewModel.add
        },

        taskTooltip: {
            visible: viewModel.showTooltip
        },
        checkAllContainer: {
            visible: viewModel.todos().length
        },
        checkAll: {
            checked: viewModel.allCompleted
        },

        todos: {
            foreach: viewModel.todos
        },
        todoListItem: function() {
            return {
                css: {
                    editing: this.editing
                }
            };
        },
        todoListItemWrapper: function() {
            return {
                css: {
                    done: this.done
                }
            };
        },
        todoCheckBox: function() {
            return {
                checked: this.done
            };
        },
        todoContent: function() {
            return {
                text: this.content,
                event: {
                    dblclick: this.edit
                }
            };
        },
        todoDestroy: function() {
            return {
                click: viewModel.remove
            };
        },

        todoEdit: function() {
            return {
                value: this.content,
                valueUpdate: "afterkeydown",
                enterKey: this.stopEditing,
                event: {
                    blur: this.stopEditing
                }
            };
        },

        todoCount: {
            visible: viewModel.remainingCount
        },
        remainingCount: {
            text: viewModel.remainingCount
        },
        remainingCountWord: function() {
            return {
                text: viewModel.getLabel(viewModel.remainingCount)
            };
        },
        todoClear: {
            visible: viewModel.completedCount
        },
        todoClearAll: {
            click: viewModel.removeCompleted
        },
        completedCount: {
            text: viewModel.completedCount
        },
        completedCountWord: function() {
            return {
                text: viewModel.getLabel(viewModel.completedCount)
            };
        },
        todoInstructions: {
            visible: viewModel.todos().length
        }
    };

    ....
````

No entanto, existem duas linhas que faltam no trecho acima - ainda precisamos nossa função getBindings , que fará um loop em cada uma das chaves em nossos atributos de classe de dados e construir o objeto resultante de cada um deles . Se detectarmos que o objeto de ligação é uma função, podemos chamá-lo com os nossos dados atuais usando o contexto presente . Nosso provedor de ligação personalizada completa ficaria da seguinte forma :

````js
// We can now create a bindingProvider that uses
// something different than data-bind attributes
ko.customBindingProvider = function( bindingObject ) {
    this.bindingObject = bindingObject;

    // determine if an element has any bindings
    this.nodeHasBindings = function( node ) {
        return node.getAttribute ? node.getAttribute( "data-class" ) : false;
    };
  };

// return the bindings given a node and the bindingContext
this.getBindings = function( node, bindingContext ) {

    var result = {},
        classes = node.getAttribute( "data-class" );

    if ( classes ) {
        classes = classes.split( "" );

        //evaluate each class, build a single object to return
        for ( var i = 0, j = classes.length; i < j; i++ ) {

           var bindingAccessor = this.bindingObject[classes[i]];
           if ( bindingAccessor ) {
               var binding = typeof bindingAccessor === "function" ? bindingAccessor.call(bindingContext.$data) : bindingAccessor;
               ko.utils.extend(result, binding);
           }

        }
    }

    return result;
};
};
````

Deste modo , as poucas linhas finais dos nossos ligações objecto pode ser definido como se segue :

````js
// set ko's current bindingProvider equal to our new binding provider
ko.bindingProvider.instance = new ko.customBindingProvider( bindings );

// bind a new instance of our ViewModel to the page
ko.applyBindings( viewModel );

})();
````

O que estamos fazendo aqui é efetivamente a definição de construtor de nosso manipulador de ligação que aceita um objeto ( ligações ) que usamos para pesquisar nossas ligações. Poderíamos, então, re- escrever a marcação para a nossa aplicação View usando - classes de dados da seguinte forma:

````HTML
<div id="create-todo">
                <input id="new-todo" data-class="newTodo" placeholder="What needs to be done?" />
                <span class="ui-tooltip-top" data-class="taskTooltip" style="display: none;">Press Enter to save this task</span>
            </div>
            <div id="todos">
                <div data-class="checkAllContainer" >
                    <input id="check-all" class="check" type="checkbox" data-class="checkAll" />
                    <label for="check-all">Mark all as complete</label>
                </div>
                <ul id="todo-list" data-class="todos" >
                    <li data-class="todoListItem" >
                        <div class="todo" data-class="todoListItemWrapper" >
                            <div class="display">
                                <input class="check" type="checkbox" data-class="todoCheckBox" />
                                <div class="todo-content" data-class="todoContent" style="cursor: pointer;"></div>
                                <span class="todo-destroy" data-class="todoDestroy"></span>
                            </div>
                            <div class="edit">
                                <input class="todo-input" data-class="todoEdit"/>
                            </div>
                        </div>
                    </li>
                </ul>
            </div>
````

Neil Kerkin montou um aplicativo de demonstração TodoMVC completa utilizando o acima, que pode ser acessado e jogado ao redor com aqui.

Embora possa parecer um monte de trabalho na explicação acima, agora que temos um método getBindings genéricos escrita , é muito mais trivial para simplesmente re- usá-lo e usar dados -classes em vez de estritas - ligações de dados para escrever o nosso aplicações KnockoutJS vez . O resultado líquido é de marcação espero mais limpo com nossas ligações de dados que está sendo deslocado da vista um ligações objeto em seu lugar.

## MVC Vs. MVP Vs. MVVM
