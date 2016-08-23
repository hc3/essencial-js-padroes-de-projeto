# JavaScript MV* Patterns

Nesta seção, vamos rever três padrões de arquitetura muito importante - MVC ( Model-View- Controller) , MVP ( Model-View -Presenter ) e MVVM ( Model-View- ViewModel ) . No passado, esses padrões têm sido muito utilizadas para aplicações desktop estruturação e do lado do servidor , mas ele só tem sido nos últimos anos que vêm sendo aplicadas para JavaScript.

Como a maioria dos desenvolvedores de JavaScript usando atualmente esses padrões optar por utilizar bibliotecas como Backbone.js para implementar um / MV * -como estrutura MVC , vamos comparar como as soluções modernas , tais como diferem na sua interpretação do MVC em relação ao clássico assume estes padrões.

Vamos primeiro cobrir o básico.

## MVC

MVC é um padrão de projeto arquitetônico que incentiva a organização de uma melhor aplicação através de uma separação de interesses . Ele aplica o isolamento de dados de negócios (modelos) de interfaces de usuário ( Visualizações) , com um terceiro componente ( Controllers ) lógica tradicionalmente gestão e de entrada do usuário . O padrão foi originalmente concebido por Trygve Reenskaug durante seu tempo trabalhando em Smalltalk -80 (1979) , onde foi inicialmente chamado MVC - Editor. MVC passou a ser descrito em profundidade , em 1995, de " Design Patterns : Elementos de Software Reutilizável Orientado a Objetos " (o " GoF " livro ) , que desempenharam um papel na popularização seu uso.

## Smalltalk-80 MVC

É importante entender o que o padrão MVC original foi com o objetivo de resolver como é mutado muito fortemente desde os dias de sua origem. Voltar na década de 70 , interfaces de usuário gráficas eram poucos e distantes entre si, um conceito conhecido como Apresentação Separado começou a ser usado como um meio para fazer uma clara divisão entre objetos de domínio que modelou conceitos no mundo real (por exemplo, uma foto, uma pessoa ) e os objetos de apresentação que foram prestados a tela do usuário.

A implementação Smalltalk -80 da MVC levou este conceito mais longe e tinha um objetivo de separar a lógica da aplicação da interface do usuário . A ideia era que dissociar essas partes do aplicativo também permitirá a reutilização de modelos para outras interfaces do aplicativo. Existem alguns pontos interessantes dignos de nota sobre a arquitetura MVC do Smalltalk -80:

<ul>
  <li>Um modelo representado dados específicos do domínio e ignorava a interface de usuário (Vistas e controladores ) . Quando um modelo alterado, ele iria informar os seus observadores.</li>
  <li>A View representou o estado atual de um modelo . O padrão Observer foi usado para deixar o Vista saber sempre que o modelo foi atualizada ou modificada.</li>
  <li>Apresentação foi cuidado por a vista, mas não havia apenas um único View e Controller - um par View-Controller foi necessária para cada seção ou elemento que está sendo exibido na tela.</li>
  <li>O papel Controladores neste par estava lidando com a interação do usuário (como as teclas pressionadas e ações por exemplo, cliques ) , a tomada de decisões para a vista.</li>
</ul>

Os desenvolvedores às vezes são surpreendidos quando eles aprendem que o padrão Observer (hoje comumente implementado como a variação Publicação / Assinatura ) foi incluída como parte da arquitetura do MVC muitas décadas atrás. Em MVC do Smalltalk -80 , a Vista observa o modelo . Como mencionado no item acima , a qualquer hora as mudanças modelo, o Visualizações reagir. Um exemplo simples disso é uma aplicação suportada por dados do mercado de ações - para que o aplicativo para ser útil, qualquer alteração dos dados em nossos modelos devem resultar na Vista sendo atualizada instantaneamente.

Martin Fowler tem feito um excelente trabalho de escrever sobre as origens do MVC ao longo dos anos e se estiver interessado em mais algumas informações históricas sobre MVC do Smalltalk -80 , eu recomendo a leitura de sua obra.

## MVC For JavaScript Developers

Nós analisamos os anos 70, mas agora vamos voltar para o aqui e agora. Nos tempos modernos , o padrão MVC foi aplicado a uma grande variedade de linguagens de programação , incluindo de maior relevância para nós: JavaScript. JavaScript tem agora uma série de quadros ostentando suporte para MVC (ou variações sobre isso , o que nos referimos como a família MV x) , permitindo que os desenvolvedores adicionem facilmente estrutura de suas aplicações sem grande esforço.

Essas estruturas incluem os gostos de Backbone , Ember.js e AngularJS . Dada a importância de evitar a "spaghetti" de código, um termo que descreve o código que é muito difícil de ler ou manter devido à sua falta de estrutura , é imperativo que o desenvolvedor JavaScript moderna entender o que esse padrão fornece . Isto permite-nos apreciar de forma eficaz o que essas estruturas nos permitem fazer diferente.

Sabemos que MVC é composto por três componentes principais :

## Models

Modelos gerenciar os dados para um aplicativo . Eles estão preocupados com camadas nem a interface do usuário , nem apresentação ao contrário, representam formas únicas de dados que um aplicativo pode exigir . Quando um modelo é alterado ( por exemplo , quando ele é atualizado ), ele irá normalmente notificar os seus observadores ( vistas e.g. , um conceito que vai cobrir em breve ) que ocorreu uma mudança para que eles possam reagir em conformidade.

Para compreender os modelos ainda mais , vamos imaginar que temos uma galeria de fotos JavaScript aplicação. Em uma galeria de fotos , o conceito de uma foto mereceria o seu próprio modelo , pois representa um único tipo de dados específicos do domínio . Tal modelo pode conter atributos relacionados, tais como uma legenda, fonte de imagem e meta- dados adicionais. Uma foto específica seria armazenado em uma instância de um modelo e um modelo também pode ser reutilizável. Abaixo podemos ver um exemplo de um modelo muito simplista implementada usando Backbone.

````js
var Photo = Backbone.Model.extend({

    // Default attributes for the photo
    defaults: {
      src: "placeholder.jpg",
      caption: "A default image",
      viewed: false
    },

    // Ensure that each photo created has an `src`.
    initialize: function() {
       this.set( { "src": this.defaults.src} );
    }

});
````

As capacidades embutidas de modelos variam entre quadros , no entanto, é bastante comum para eles para suportar a validação de atributos , onde os atributos representam as propriedades do modelo , como um identificador do modelo . Ao usar modelos em aplicações do mundo real que geralmente também desejo persistência modelo . Persistência nos permite editar e modelos de atualização com o conhecimento que seu estado mais recente será salvo em qualquer : memória, em localStorage de dados de armazenamento de um usuário ou sincronizado com um banco de dados.

Além disso , um modelo também pode ter múltiplos pontos de vista observando. Se por exemplo , o nosso modelo foto continha meta- dados, tais como a sua localização ( latitude e longitude ) , amigos que estavam presentes na foto (a lista de identificadores ) e uma lista de marcas , um desenvolvedor pode decidir dar uma visão única para exibir cada uma destas três facetas.

Não é incomum para as modernas MVC / MT * quadros para fornecer um meio de modelos de grupo em conjunto ( por exemplo na espinha dorsal , estes grupos são referidos como " colecções ") . Gerenciando modelos em grupos nos permite escrever lógica de aplicação com base em notificações do grupo deve qualquer modelo que contém ser alterado. Isso evita a necessidade de observar manualmente instâncias de modelo individuais.

Um agrupamento de exemplo de modelos em uma espinha dorsal de recolha simplificado pode ser visto abaixo.

````js
var PhotoGallery = Backbone.Collection.extend({

    // Reference to this collection's model.
    model: Photo,

    // Filter down the list of all photos
    // that have been viewed
    viewed: function() {
        return this.filter(function( photo ){
           return photo.get( "viewed" );
        });
    },

    // Filter down the list to only photos that
    // have not yet been viewed
    unviewed: function() {
      return this.without.apply( this, this.viewed() );
    }
});
````

textos mais antigos sobre MVC podem também conter referência a uma noção de modelos de gestão de aplicação state.In JavaScript estado aplicações tem uma conotação diferente , normalmente referindo-se ao atual "estado" ou seja vista ou sub -view (com dados específicos) sobre a tela dos usuários em um ponto fixo. Estado é um tema que é discutido regularmente quando se olha para aplicações de página única, onde o conceito de estado precisa de ser simuladas.

Então, para resumir , os modelos estão principalmente preocupados com dados corporativos.

## Views

As vistas são uma representação visual dos modelos que apresentam uma exibição filtrada de seu estado atual. Enquanto vistas Smalltalk são cerca de pintura e manutenção de um bitmap, vistas JavaScript são sobre a construção e manutenção de um elemento DOM.

Uma visão tipicamente observa um modelo e é notificado quando o modelo muda , permitindo a vista para se atualizar em conformidade. Projeto literatura padrão comumente refere-se a pontos de vista como "burro" , dado que os seus conhecimentos de modelos e controladores em uma aplicação é limitada.

Os usuários são capazes de interagir com vista e isso inclui a capacidade de ler e editar (isto é obter ou definir os valores de atributo in) modelos. Como a vista é a camada de apresentação , que geralmente apresentam a capacidade de editar e atualizar de forma user-friendly . Por exemplo, na antiga aplicação galeria de fotos discutimos anteriormente , edição modelo poderia ser facilitada através de uma visão "editar ", onde um usuário que selecionou uma foto específica poderia editar a sua meta- dados.

A tarefa real de atualizar o modelo cai para controladores (que nós estaremos cobrindo em breve).

Vamos explorar pontos de vista um pouco mais usando a baunilha JavaScript implementação de exemplo . Abaixo podemos ver uma função que cria uma única visualização de fotografias , consumindo uma instância de modelo e uma instância do controlador.

Nós definimos um utilitário render () dentro do nosso ponto de vista que é responsável por renderizar o conteúdo do Modelo fotográfico usando um motor de templates JavaScript ( Sublinhado templates ) e atualizar o conteúdo do nosso ponto de vista , referenciado por photoEl .

O photomodel em seguida, adiciona a callback render () como um dos seus assinantes para que, através do padrão Observer que pode desencadear a fim de atualizar quando o modelo for alterado.

Pode-se perguntar onde a interação do usuário entra em jogo aqui. Quando os usuários clicam em qualquer elemento dentro da vista , não é de responsabilidade do fim de saber o que fazer a seguir. Ele se baseia em um controlador para tomar esta decisão para ele. Na nossa implementação de exemplo , isso é conseguido através da adição de um ouvinte de evento para photoEl que irá delegar lidar com o comportamento do botão de volta para o controlador , passando as informações do modelo junto com ele em caso de necessidade.

O benefício desta arquitetura é que cada componente desempenha o seu próprio papel separada em fazer a função do aplicativo , conforme necessário.

````js
var buildPhotoView = function ( photoModel, photoController ) {

  var base = document.createElement( "div" ),
      photoEl = document.createElement( "div" );

  base.appendChild(photoEl);

  var render = function () {
          // We use a templating library such as Underscore
          // templating which generates the HTML for our
          // photo entry
          photoEl.innerHTML = _.template( "#photoTemplate", {
              src: photoModel.getSrc()
          });
      };

  photoModel.addSubscriber( render );

  photoEl.addEventListener( "click", function () {
    photoController.handleEvent( "click", photoModel );
  });

  var show = function () {
    photoEl.style.display = "";
  };

  var hide = function () {
    photoEl.style.display = "none";
  };

  return {
    showView: show,
    hideView: hide
  };

};
````

## Templating

No contexto das estruturas JavaScript que suportam MVC / MT * , vale a pena discutir brevemente JavaScript templates e sua relação com vistas à medida que tocou brevemente sobre ela na última seção.

Tem sido considerado por muito tempo ( e comprovada ) uma má prática da performance para criar manualmente grandes blocos de marcação HTML na memória através de concatenação . Os desenvolvedores que fazem presa assim que caíram para a iteração inperformantly através de seus dados , envolvendo-o em divs aninhados e usando técnicas ultrapassadas , como document.write para injetar o "modelo" para o DOM. Como isso normalmente significa manter em linha de marcação baseado num guião com a nossa marcação padrão, pode rapidamente tornar-se tanto difícil de ler e mais importante, manter tais desastres , especialmente quando a construção de aplicações não- trivialmente porte.

soluções JavaScript templates (como Handlebars.js e bigode ) são frequentemente utilizados para definir modelos para vistas como marcação (seja armazenado externamente ou dentro de tags de script com um tipo personalizado - por exemplo text / modelo) contendo as variáveis ​​do modelo . Variáveis ​​podem ser delimitada usando uma sintaxe variável (por exemplo, {{ name} }) e os quadros são tipicamente inteligente o suficiente para aceitar dados em um formulário de JSON (que instâncias de modelo podem ser convertidos em ) de tal forma que nós só precisa se preocupar com a manutenção de modelos limpos e modelos limpos. A maior parte do trabalho pesado para fazer com a população é cuidado pela própria estrutura . Isto tem um grande número de benefícios , especialmente quando optando por armazenar modelos externamente como isso pode dar lugar a modelos sendo carregado dinamicamente em uma base como necessária quando se trata de construir aplicações maiores.

Abaixo podemos ver dois exemplos de modelos HTML . Um implementado usando a estrutura Handlebars.js popular e modelos de outro usando o sublinhado.

<b>Handlebars.js</b>
````HTML
<li class="photo">
  <h2>{{caption}}</h2>
  <img class="source" src="{{src}}"/>
  <div class="meta-data">
    {{metadata}}
  </div>
</li>
````
<b>Underscore.js Microtemplates</b>
````HTML
<li class="photo">
  <h2><%= caption %></h2>
  <img class="source" src="<%= src %>"/>
  <div class="meta-data">
    <%= metadata %>
  </div>
</li>
````

Note-se que os modelos não são eles próprios pontos de vista. Os desenvolvedores vindos de uma arquitetura Struts Modelo 2 pode sentir como um modelo * é * uma vista, mas não é . A vista é um objeto que observa um modelo e mantém a representação visual up- to-date . Um modelo * pode * ser uma maneira declarativa para especificar parte ou mesmo a totalidade de um objeto de exibição para que ele pode ser gerado a partir da especificação do modelo.

Também é interessante notar que em desenvolvimento web clássico, navegando entre as visões independentes necessário o uso de uma atualização de página . In-page Individual aplicações JavaScript no entanto , uma vez que dados são buscados a partir de um servidor via Ajax , ele pode simplesmente ser processado de forma dinâmica em uma nova visão dentro da mesma página , sem ser necessária qualquer actualização.

O papel da navegação cai , assim, a um " router" , que auxilia no gerenciamento de estado do aplicativo (por exemplo, permitindo que os usuários para marcar uma visão particular de terem navegado ) . Como os roteadores são, no entanto , nem uma parte do MVC nem presente em cada framework MVC -like , eu não vou estar indo para eles em maior detalhe nesta seção.

Para resumir, vistas são uma representação visual dos nossos dados da aplicação.

## Controllers

Os controladores são um intermediário entre modelos e pontos de vista que são classicamente responsável por atualizar o modelo quando o usuário manipula a vista.

Em nossa galeria de fotos do aplicativo, um controlador seria responsável pelo tratamento muda o usuário feitas para a visualização de edição para uma foto particular, a atualização de um modelo específico de fotografia quando um usuário tiver terminado a edição.

Remember that the controllers fulfill one role in MVC: the facilitation of the Strategy pattern for the view. In the Strategy pattern regard, the view delegates to the controller at the view's discretion. So, that's how the strategy pattern works. The view could delegate handling user events to the controller when the view sees fit. The view *could* delegate handling model change events to the controller if the view sees fit, but this is not the traditional role of the controller.

Em termos de onde a maioria dos frameworks JavaScript MVC tira do que é convencionalmente considerado " MVC ", no entanto , é com os controladores . As razões para isso variam, mas na minha opinião honesta , é que os autores quadro inicialmente olhar para a interpretação do lado do servidor da MVC, perceber que não se traduz 1: 1 no lado do cliente e re- interpretar o C em MVC para significar algo que sentem faz mais sentido. O problema com isso, porém, é que é subjetivo, aumenta a complexidade , tanto o entendimento do padrão MVC clássico e, claro, o papel de controladores em estruturas modernas.

Como exemplo , vamos rever brevemente a arquitetura da estrutura arquitetônica populares Backbone.js . Backbone contém modelos e visualizações ( algo semelhante ao que revista mais cedo ) , no entanto, ele não tem realmente verdadeiros controladores. Seus pontos de vista e routers agir um pouco semelhante a um controlador, mas também não são realmente controladores por conta própria.

A este respeito , contrariamente ao que pode ser mencionado na documentação oficial ou nas postagens do blog , Backbone é nem um quadro verdadeiramente MVC / MVP nem MVVM . É de fato melhor para considerá-lo um membro da família MV * que aborda a arquitetura em sua própria maneira. Há, naturalmente, nada de errado com isso, mas é importante distinguir entre MVC clássico e MV * devemos começar contando com o conselho de literatura clássica sobre a antiga para ajudar com o último.

## Controllers in another library (Spine.js) vs Backbone.js

<b>Spine.js</b>

Sabemos agora que os controladores são, tradicionalmente responsável por atualizar o modelo quando o usuário atualiza a vista. É interessante notar que o mais popular framework JavaScript MVC / MV * no momento da escrita ( Backbone ) não tem o seu próprio conceito explícito de controladores.

Assim, pode ser útil para nós verificar o controlador de outro framework MVC para apreciar a diferença de implementações e ainda demonstrar como quadros nontraditionally abordar o papel do controlador. Para isso, vamos dar uma olhada em um controlador de amostra a partir Spine.js :

Neste exemplo, vamos ter um controlador chamado PhotosController que será encarregado de fotos individuais na aplicação. Ele irá garantir que quando as atualizações de vista (por exemplo, um usuário editou a foto meta- dados ) o modelo correspondente faz também.

Nota: Nós não seremos investigando pesadamente em Spine.js em tudo, mas só vai ter uma visão de dez pés do que seus controladores pode fazer:

````js
// Controllers in Spine are created by inheriting from Spine.Controller

var PhotosController = Spine.Controller.sub({

  init: function () {
    this.item.bind( "update", this.proxy( this.render ));
    this.item.bind( "destroy", this.proxy( this.remove ));
  },

  render: function () {
    // Handle templating
    this.replace( $( "#photoTemplate" ).tmpl( this.item ) );
    return this;
  },

  remove: function () {
    this.el.remove();
    this.release();
  }
});
````

Em Spine , os controladores são considerados a cola para um aplicativo, adicionando e responder a eventos DOM , tornando modelos e garantir que os pontos de vista e modelos são mantidos em sincronia ( o que faz sentido no contexto do que sabemos ser um controlador).

O que estamos fazendo no exemplo acima é a criação de ouvintes na atualização e destruir eventos usando render () e remove ( ) . Quando uma entrada de foto é atualizado , nós re- tornar a fim de reflectir as alterações para os meta-dados . Da mesma forma, se a foto é excluído da galeria , nós removê-lo do ponto de vista. Na função render () , estamos usando Sublinhado micro- templates (via _.template ()) para processar um modelo de JavaScript com o     photoTemplate ID . Isso simplesmente retorna um string HTML compilado usado para preencher o conteúdo de photoEl .

O que isto nos proporciona é uma forma muito leve, simples de gerenciar mudanças entre o modelo e a vista.

<b>Backbone.js</b>

Mais tarde nesta seção vamos rever as diferenças entre Backbone e MVC tradicional, mas por agora vamos nos concentrar nos controladores .

Em Backbone , um compartilha da responsabilidade de um controlador com tanto o Backbone.View e Backbone.Router . Algum tempo atrás Backbone fiz uma vez vem com seu próprio Backbone.Controller , mas como a nomeação para este componente não faz sentido para o contexto em que ele estava sendo usado , foi mais tarde renomeado para Router.

Roteadores lidar com um pouco mais da responsabilidade do controlador , como é possível vincular os eventos lá para modelos e temos o nosso vista responder a Dom eventos e renderização. Como Tim Branyen ( outro contribuinte Backbone baseado em Bocoup ) também tem indicado anteriormente , é possível fugir com não necessitando Backbone.Router em tudo para isso, então uma maneira de pensar sobre isso usando o paradigma Router é provavelmente:

````js
var PhotoRouter = Backbone.Router.extend({
  routes: { "photos/:id": "route" },

  route: function( id ) {
    var item = photoCollection.get( id );
    var view = new PhotoView( { model: item } );

    $('.content').html( view.render().el );
  }
});
````

Para resumir, o takeaway desta seção é que os controladores de gerenciar a lógica ea coordenação entre modelos e visualizações em um aplicativo.

## O que MVC nos dar ?

Esta separação de interesses em MVC facilita a modularização mais simples da funcionalidade do aplicativo e permite :

<ul>
  <li>Mais fácil manutenção geral . Quando as atualizações precisam ser feitas para o aplicativo é muito claro se as mudanças são centradas em dados , ou seja, alterações em modelos e, possivelmente, controladores ou meramente visual , ou seja, alterações nas visualizações .</li>
  <li>A dissociação entre modelos e pontos de vista significa que é significativamente mais simples e direta para escrever testes de unidade para lógica de negócios</li>
  <li>Duplicação de modelo de baixo nível e código do controlador (isto é o que pode ter sido usando em vez disso) é eliminado através da aplicação</li>
  <li>Dependendo do tamanho do aplicativo e separação de funções , esta modularidade permite que os desenvolvedores responsáveis ​​pela lógica do núcleo e desenvolvedores que trabalham nas interfaces de usuário para trabalhar simultaneamente</li>
</ul>

## Smalltalk-80 MVC In JavaScript

Embora a maioria dos modernos frameworks JavaScript tentar evoluir o paradigma MVC para melhor atender as diferentes necessidades de desenvolvimento de aplicações web , há um quadro que tenta aderir à forma pura do padrão encontrado em Smalltalk -80. Maria.js ( https://github.com/petermichaux/maria ) por Peter Michaux oferece uma implementação que é fiel às origens CVM - Os modelos são modelos , vistas são vistas e controladores são nada, mas os controladores . Enquanto alguns desenvolvedores podem se sentir um quadro MV * deve abordar mais preocupações , este é uma referência útil para estar ciente de no caso de você gostaria de ter uma implementação JavaScript do MVC originais.

## Aprofundando

Neste ponto no livro, devemos ter uma compreensão básica do que o padrão MVC fornece , mas ainda há algumas informações fascinantes sobre o assunto digno de nota.

O GoF não se referem a MVC como um padrão de design, mas sim considerá-lo um conjunto de classes para construir uma interface de usuário . Na sua opinião , é realmente uma variação de três padrões de design clássico : o Observer, Estratégia e padrões Composite . Dependendo de como MVC foi implementado em um quadro , ele também pode usar os padrões de fábrica e Modelo . O livro GoF menciona esses padrões como extras úteis quando se trabalha com MVC.

Como já discutimos , modelos representam os dados do aplicativo , mas as opiniões são o que o usuário é apresentado na tela . Como tal, MVC baseia-se no padrão Observer para alguns de sua comunicação núcleo (algo que, surpreendentemente, não é coberto em muitos artigos sobre o padrão MVC ) . Quando um modelo for alterado, ele notifica seus observadores (Vistas) que algo foi atualizado - este é talvez o relacionamento mais importante no MVC. A natureza observador desta relação é também o que facilita múltiplas visões estar ligado ao mesmo modelo .

Para os desenvolvedores interessados ​​em conhecer mais sobre a natureza dissociada da MVC ( mais uma vez, dependendo da implementação ) , um dos objetivos do padrão é para ajudar a definir relacionamentos um-para -muitos entre um tópico ( objeto de dados ) e os seus observadores. Quando um tópico muda , seus observadores são actualizados. Pontos de vista e controladores têm uma relação um pouco diferente. Controladores de facilitar vistas para responder a entrada do usuário diferentes e são um exemplo do padrão Strategy .

## Resumo

Tendo examinado o padrão MVC clássica , devemos agora entender como ele nos permite separar de forma limpa preocupações em um aplicativo. Também deve agora apreciar como frameworks JavaScript MVC podem diferir em sua interpretação do padrão MVC , que embora bastante aberto a variação, ainda compartilha alguns dos conceitos fundamentais do padrão original tem para oferecer.

Ao rever um novo framework JavaScript MVC / MT * , lembre-se - pode ser útil dar um passo para trás e avaliar como é optou por abordagem de arquitetura ( especificamente, como ele suporta modelos de execução, vistas , controladores ou outras alternativas ) , pois isso pode melhor nos ajudar Grokar como a estrutura espera ser utilizado .
