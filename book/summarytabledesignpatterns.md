# Categorização de Padrões de Projeto

Em minhas primeiras experiências de aprendizado sobre padrões de projeto, eu encontrei a tabela a seguir um lembrete muito útil do que uma série de padrões tem a oferecer - ela cobre os 23 padrões de projeto mencionados pela GoF. A tabela original foi resumida por Elyse Nielsen em 2004 e eu a modifiquei onde foi necessário de acordo com nossa discussão nessa seção do livro.

Eu recomendo usar esta tabela como referência, mas lembre-se que existem padrões adicionais que não foram mencionados aqui mas vão ser discutidos depois no livro.

## Uma breve nota sobre as classes

Tenha em mente que haverá padrões nesta tabela que fazem referência ao conceito de "classes". JavaScript é uma linguagem sem classes, entretanto classes podem ser simuladas usando funções.

A abordagem mais comum para conseguir isto é definindo uma função JavaScript onde nós então criamos um objeto usando a palavra-chave `new`. `this` pode ser usado para ajudar a definir novas propriedades e métodos para o objeto como no exemplo seguinte:

```javascript
// Uma "classe" carro
function Car ( model ) {

	this.model = model;
	this.color = "silver";
	this.year  = "2012";

	this.getInfo = function () {
		return this.model + " " + this.year;
	};
}
```

Nós podemos então instanciar o objeto usando o construtor Car que definimos acima:

```javascript
var myCar = new Car("ford");

myCar.year = "2010";

console.log( myCar.getInfo() );

```

Para mais formas de definir "classes" usando JavaScript, veja [este](http://www.phpied.com/3-ways-to-define-a-javascript-class/) útil post de Stoyan Stefanov.

Vamos agora analisar a tabela.

### Criacional

|Criacional|Baseada no conceito de criar um objeto.|
| :-------------: |:-------------:|
| **Classe** |
| Método Factory | Faz uma instância de diversas classes derivadas baseadas em dados ou eventos interligados. |
| **Objeto** |
| Factory Abstrata | Cria uma instância de diversas famílias de classes sem detalhar classes concretas. |
| Builder | Separa a construção de objetos de sua representação, sempre criando o mesmo tipo de objeto. |
| Prototype | Uma inicialização completa usada para copiar ou clonar. |
| Singleton | Uma classe com somente uma instância com pontos de acesso global. |


### Estrutural

|Estrutural|Baseada na ideia de construir blocos de objetos.|
| :-------------: |:-------------:|
| **Classe** |  |
| Adapter | Liga interfaces de diferentes classes, portanto as classes podem trabalhar juntas apesar das incompatibilidades das interfaces |
|**Objeto**||
|Adapter|Liga interfaces de diferentes classes, portanto as classes podem trabalhar juntas apesar das incompatibilidades das interfaces|
|Bridge|Separa uma interface de objeto a partir de sua implementação, assim os dois podem variar independentemente|
|Composite|Uma estrutura dos objetos simples e compostos que torna o objeto total mais que a soma de suas partes.|
|Decorator|Adiciona dinâmicamente processamento alternativo para objetos.|
|Facade|Uma única classe que esconde a complexidade de todo subsistema.|
|Flyweight|Um exemplo refinado usado para o compartilhamento eficiente de informações que estão contidas em outro lugar.|
|Proxy|Um objeto de espaço reservado que representa o verdadeiro objeto.|


### Comportamental

|Comportamental|Baseado na forma como os objetos desempenham e trabalham em conjunto.|
| :-------------: |:-------------:|
|**Classe**||
|Interpreter|Uma forma de incluir elementos da linguagem em um aplicativo para coincidir com a gramática da língua pretendida.|
|Template Método|Cria o *shell* de um algoritmo em um método, em seguida, adia as etapas exatas para uma subclasse. |
|**Objeto**||
|Chain of Responsibility|Uma forma de passar uma requisição entre uma cadeia de objetos para encontrar o objeto que pode manipular a requisição.|
|Command|Encapsula uma requisição de comando como um objeto para permitir, login ou fila de pedidos, e fornece tratamento de erros para os pedidos não tratados.|
|Iterator|Acessa sequencialmente os elementos de uma coleção sem conhecer o funcionamento interno da coleção.|
|Mediator|Define simples comunicações entre classes para prevenir um grupo de classes de se referir explicitamente uma a outra.|
|Memento|Captura o estado interno de um objeto para poder restaurá-lo mais tarde.|
|Observer|Uma forma de notificar a mudança para um número de classes para assegurar a consistência entre as classes.|
|State|Altera o comportamento do objeto quando seu estado muda.|
|Strategy|Encapsula um algoritmo dentro de uma classe separano a seleção a partir da implementação.|
|Visitor|Adiciona uma nova operação para uma classe sem mudar a classe.|
