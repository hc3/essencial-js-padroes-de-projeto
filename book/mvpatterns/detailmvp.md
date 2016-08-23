# MVP

Model- View-Presenter (MVP ) é um derivado do padrão de projeto MVC , que se concentra em melhorar a lógica de apresentação . Originou-se em uma empresa chamada Taligent no início de 1990 , enquanto eles estavam trabalhando em um modelo para um ambiente C ++ CommonPoint . Embora ambos MVC e MVP alvo a separação de interesses entre vários componentes , existem algumas diferenças fundamentais entre eles.

Para os fins deste resumo , vamos concentrar-se na versão do MVP mais adequado para arquiteturas baseadas na web.

## Models, Views & Presenters

A P em MVP significa apresentador. É um componente que contém a lógica de negócios de interface do usuário para a vista. Ao contrário de MVC, invocações da visualização são delegadas para o apresentador , que está dissociada da visão e, em vez falar com ele através de uma interface. Isto permite a todos os tipos de coisas úteis , tais como ser capaz de zombar vistas em testes de unidade .

A implementação mais comum de MVP é aquele que usa uma exibição passiva (uma visão que é para todos os efeitos "burras" ) , que contém pouca ou nenhuma lógica . Se MVC e MVP são diferentes é porque o C e P fazem coisas diferentes . No MVP , o P observa modelos e visualizações atualizações quando os modelos mudam. A P efetivamente liga modelos de pontos de vista, uma responsabilidade que era anteriormente detida pelos controladores em MVC.

Solicitado por uma visão , apresentadores de executar qualquer trabalho a fazer com as solicitações do usuário e passar dados de volta para eles . A este respeito , eles recuperar dados , manipulá-lo e determinar como os dados devem ser apresentados na vista . Em algumas implementações , o apresentador também interage com uma camada de serviço para persistir dados ( modelos). Modelos podem desencadear eventos , mas é o papel apresentadores para inscrever-se a eles para que ele possa atualizar a vista. Nesta arquitetura passiva , não temos nenhum conceito de dados diretos de ligação. Visualizações expor setters que os apresentadores podem usar para definir dados.

O benefício desta mudança do MVC é que ela aumenta a capacidade de teste da nossa aplicação e fornece uma separação mais clara entre a visão eo modelo . Esta não é, contudo , sem os seus custos como a falta de dados de suporte de ligação no padrão muitas vezes pode significar ter que cuidar dessa tarefa separadamente.

Apesar de uma implementação comum de uma ideia passiva é para a vista para implementar uma interface , existem variações sobre ele , incluindo o uso de eventos que pode dissociar a vista do apresentador um pouco mais. Como não temos a construção de interface em JavaScript , estamos usando mais um protocolo de uma interface explícita aqui. É tecnicamente ainda uma API e é provavelmente justo para nos referir a ele como uma interface a partir dessa perspectiva .

Há também uma variação Controlador de Supervisão de MVP , que é mais perto dos padrões MVC e MVVM , pois proporciona a partir do modelo diretamente a partir da tela de ligação de dados . Valor-chave observando ( KVO ) plugins (como Backbone.ModelBinding plug-in de Derick Bailey ) tendem a trazer Backbone fora da vista do passivo e mais para as Supervisão controlador ou MVVM variações.

## MVP or MVC?

MVP é geralmente usado na maioria das vezes em aplicações de nível empresarial , onde é necessário reutilizar tanto a lógica de apresentação possível. Aplicações com vistas muito complexo e uma grande quantidade de interação com o usuário pode achar que MVC não se encaixa o projeto de lei aqui como resolver este problema pode significar bastante dependentes em vários controladores . No MVP , tudo isso lógica complexa pode ser encapsulado em um apresentador , que pode simplificar a manutenção muito.

Como vistas MVP são definidos através de uma interface ea interface é tecnicamente o único ponto de contato entre o sistema ea vista (diferente de um apresentador ) , este padrão também permite que os desenvolvedores escrevam lógica de apresentação sem a necessidade de esperar por designers para produzir layouts e gráficos para a aplicação.

Dependendo da implementação , MVP pode ser mais fácil automaticamente teste de unidade de MVC. A razão citada para isto é que o apresentador pode ser usado como um mock completa da interface de utilizador e por isso pode ser testado unidade independente de outros componentes . Na minha experiência, isso realmente depende dos idiomas que estão implementando MVP em (há uma grande diferença entre optando por MVP para um projeto de JavaScript mais de um para dizer, ASP.net ) .

No final do dia, as preocupações subjacentes que podem ter com MVC provavelmente será uma realidade para MVP , dado que as diferenças entre eles são principalmente semântica. Enquanto estamos limpa separar preocupações em modelos , visualizações e controladores ( ou apresentadores ) que deve ser a realização da maioria dos mesmos benefícios , independentemente da variação que optar.

## MVC, MVP and Backbone.js

Há muito poucos , se alguns frameworks JavaScript arquitectónicas que pretendem implementar os padrões MVC ou MVP em sua forma clássica como muitos desenvolvedores de JavaScript não ver MVC e MVP como sendo mutuamente exclusivas ( que são realmente mais provável ver MVP rigorosamente aplicadas quando olhando para frameworks web , como ASP.net ou GWT ) . Isto é porque é possível ter lógica apresentador / view adicionais em nossa aplicação e ainda considerá-lo um sabor de MVC.

contribuinte Backbone Irene Ros ( de Bocoup sediada em Boston ) assina esta maneira de pensar , como quando ela separa vistas para fora em seus próprios componentes distintos , ela precisa de algo para realmente montá-los para ela. Isso poderia ser uma rota controlador (como um Backbone.Router , coberto mais tarde no livro ) ou um retorno de chamada em resposta aos dados sendo procurados .

Dito isto, alguns desenvolvedores , contudo, sente que Backbone.js melhor se encaixa na descrição de MVP do que MVC. Sua visão é de que :

<ul>
  <li>O apresentador em MVP descreve melhor o Backbone.View (a camada entre a vista de modelos e os dados associados a ele ) do que um controlador faz</li>
  <li>O modelo ajusta Backbone.Model ( que não é significativamente diferente para os modelos no MVC em tudo )</li>
  <li>Os pontos de vista melhor representam modelos (por exemplo, modelos de marcação Guiador / bigode )</li>
</ul>

A resposta a esta pode ser que a visão pode também ser apenas uma View ( como por MVC ), porque Backbone é flexível o suficiente para deixá-lo ser usado para várias finalidades . O V no MVC eo P em MVP tanto pode ser realizado por Backbone.View porque eles são capazes de atingir dois propósitos : renderizar componentes atômicos e montar os componentes prestados por outros pontos de vista.

Nós vimos também que, no Backbone a responsabilidade de um controlador é compartilhado tanto com o Backbone.View e Backbone.Router e no exemplo a seguir nós podemos realmente ver que os aspectos de que são certamente verdade.

Nosso backbone PhotoView usa o padrão Observer para "assinar " para alterações ao modelo de um modo de exibição no this.model.bind linha ( " mudança" , ...). Ele também lida com modelagem no método render () , mas ao contrário de algumas outras implementações , a interação do usuário também é tratado no Vista ( ver eventos ) .

````js
var PhotoView = Backbone.View.extend({

    //... is a list tag.
    tagName: "li",

    // Pass the contents of the photo template through a templating
    // function, cache it for a single photo
    template: _.template( $("#photo-template").html() ),

    // The DOM events specific to an item.
    events: {
      "click img": "toggleViewed"
    },

    // The PhotoView listens for changes to
    // its model, re-rendering. Since there's
    // a one-to-one correspondence between a
    // **Photo** and a **PhotoView** in this
    // app, we set a direct reference on the model for convenience.

    initialize: function() {
      this.model.on( "change", this.render, this );
      this.model.on( "destroy", this.remove, this );
    },

    // Re-render the photo entry
    render: function() {
      $( this.el ).html( this.template(this.model.toJSON() ));
      return this;
    },

    // Toggle the `"viewed"` state of the model.
    toggleViewed: function() {
      this.model.viewed();
    }

});
````

Outra opinião ( bastante diferente ) é que Backbone mais se assemelha Smalltalk -80 MVC , que passamos anteriormente.

Tão regular Backbone blogger Derick Bailey colocou anteriormente , é em última análise, melhor não forçar Backbone para caber quaisquer padrões de design específicos. padrões de projeto devem ser considerados guias flexíveis para como os aplicativos podem ser estruturados e , a este respeito , Backbone se encaixa nem MVC nem MVP . Em vez disso , ele toma emprestado alguns dos melhores conceitos de vários padrões arquitetônicos e cria uma estrutura flexível que só funciona bem.

No entanto, é digno de se entender onde e por que esses conceitos se originou , por isso espero que as minhas declarações de MVC e MVP ter sido de ajuda . Chame-lhe o caminho Backbone , MV * ou o que ajuda a referenciar o seu sabor de arquitetura de aplicação . A maioria dos frameworks JavaScript estruturais irá adoptar a sua própria visão sobre padrões clássicos , intencionalmente ou por acidente, mas o importante é que eles ajudam -nos a desenvolver aplicações que são organizados , limpos e podem ser facilmente mantidos .
