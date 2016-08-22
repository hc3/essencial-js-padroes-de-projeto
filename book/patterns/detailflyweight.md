# Flyweight

O padrão Flyweight é uma solução estrutural clássica para otimizar o código que é repetitivo , lento e compartilha de forma ineficiente de dados. Destina-se a minimizar o uso de memória em um aplicativo através da partilha de dados, tanto quanto possível com objetos relacionados ( configuração do aplicativo por exemplo , estado e assim por diante ) .

O padrão foi concebido por Paul Calder e Mark Linton em 1990 e foi nomeado após a classe de peso do boxe , que inclui lutadores com peso inferior a 112lb . O próprio Flyweight nome é derivado dessa classificação peso, pois refere-se ao peso pequeno ( espaço de memória ) o padrão visa nos ajudar a alcançar.

Na prática, Flyweight compartilhamento de dados pode envolver levando vários objetos semelhantes ou construções de dados usados ​​por um número de objetos e colocando esses dados em um único objeto externo. Nós podemos passar por este objecto para aqueles dependendo estes dados , em vez de armazenar dados de forma idêntica em cada uma.

## Usando Flyweights

Existem duas maneiras em que a FLYWEIGHT podem ser aplicadas . A primeira é a camada de dados, onde lidamos com o conceito de partilha de dados entre grandes quantidades de objetos semelhantes armazenados na memória.

A segunda é a de camada DOM em que a Mosca pode ser usado como um evento de gestor central para evitar anexar manipuladores de eventos para cada elemento filho num recipiente pai que deseja ter um comportamento semelhante.

Como a camada de dados é onde o FLYWEIGHT é a mais utilizada tradicionalmente , vamos dar uma olhada neste primeiro.
