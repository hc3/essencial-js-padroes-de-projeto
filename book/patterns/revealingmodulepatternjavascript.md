# The Revealing Module Pattern

Agora que estamos um pouco mais familiarizados com o module pattern, vamos ver uma versão imprivisada - Christian Heilmann's Reaviling Module Pattern.

O Revealing module pattern surgiu quando Heilmann estava um frustrado com o fato de ter que repetir o nome para o objeto principal
quando ele queria chamar um método publico. Ele também não gostava da exigência do padrão de ter que mudar para objeto literal para
coisas que ele desejava tornar público.

O resultado disso foi uma atualização do padrão onde ele de forma simples definiu todas as funções e variáveis no escopo privado e então
retornou um objeto anônimo que recebe as funções e variáveis que devem ser acessíveis.

vamos ver um exemplo:

````js
var myRevealingModule = (function () {

        var privateVar = "Ben Cherry",
            publicVar = "Hey there!";

        function privateFunction() {
            console.log( "Name:" + privateVar );
        }

        function publicSetName( strName ) {
            privateVar = strName;
        }

        function publicGetName() {
            privateFunction();
        }


	// Revela apenas o que deve ser público
        // funções privadas e propriedades

        return {
            setName: publicSetName,
            greeting: publicVar,
            getName: publicGetName
        };

    })();

myRevealingModule.setName( "Paul Kinlan" );
````

O Padrão pode ser usado para revelar funções ou propriedades privadas :

````js
var myRevealingModule = (function () {

        var privateCounter = 0;

        function privateFunction() {
            privateCounter++;
        }

        function publicFunction() {
            publicIncrement();
        }

        function publicIncrement() {
            privateFunction();
        }

        function publicGetCount(){
          return privateCounter;
        }

        // Revela apenas o que deve ser público
        // funções privadas e propriedades

       return {
            start: publicFunction,
            increment: publicIncrement,
            count: publicGetCount
        };

    })();

myRevealingModule.start();
````

<b>Vantagens</b>

Esse padrão permite que a sintaxe seja mais consistente. Isso cria um código mais limpo e reusável.

<b>Desvantagens</b>

A desvantagem para esse padrão é que se a função privada fizer referência para uma função pública, a função pública não pode ser
sobrecarregada se isso for necessário. isso acontece porque a função privada vai continuar fazendo referência para a implemntação privada e o padrão não deve ser aplicado para membros publicos apenas para funções.

Objetos com membros públicos que se referem a variáveis privadas também estão sujeitas as notas da regra acima.

O resultado é isso, modules criados com reaviling module pattern podem ser frágeis, se comparados aos módulos criados com o padrão module original então é sempre bom ver e rever o porquê de usar uma patterns module ou revealing module.

<b> Exemplo de aplicação prática </b> - 03/09/2016 - 09:39AM

Fiz uso desse padrão no seguinte problema, eu tinha essas duas funções que irião se repetir em todos os meus controllers:
````js
const defaultResponse = (data, statusCode = 200) => ({
  data,
  statusCode
});

const errorResponse = (message, statusCode = 400) => ({
  error:message
},statusCode);
````
eu removi elas dos controllers criei um novo .js aplicando o padrão reaviling module pattern da seguinte maneira:
````js
const callback = (() => {

  const defaultResponse = (data, statusCode = 200) => ({
    data,
    statusCode
  });

  const errorResponse = (message, statusCode = 400) => ({
    error:message
  },statusCode);

  return {
    defaultResponse,
    errorResponse
  }

})()

export default callback;
````
ficou muito melhor assim, porque agora eu só preciso fazer import desse objeto e chamar o método que preciso dele reduzindo assim a repetição de código.
