#O que é um padrão

Um padrão é uma solução resusavél que pode ser aplicada a um determinado problema no desenvolvimento de software - no nosso caso - criando código javascript, outra forma pode ser ver os padrões como templates para resolver problemas - e que são usados para situações específicas

Então, porque é importante entender padrões e se familiarizar com eles? Padrões de projeto tem três grandes beneficios:

<ol>
  <li>
  <b>Padrões são provedores de soluções:</b> eles provem solidas maneiras de resolver problemas diários com tecnicas que já foram testadas
  anteriormente criando assim uma maneira segura para a resolução do problema.
  </li>

  <li>
  <b>Padrões podem ser facilmente reusados:</b> os padrões normalmente refletem uma solução fora da caixa que pode ser adaptada para
  nossa necessidade, criando assim um código mais robusto.
  </li>

  <li>
  <b>Padrões são expressivos:</b> Quando nos olhamos para padrões geralmente vemos uma estrutra de fácil entendimento onde o código em sí
  consegue expressar a solução de maneira elegante e clara.
  </li>
</ol>

Padrões não são exatamente soluções. Isso é importante lembrar com os padrões conseguimos criar esquemas de soluções. Os Padrões
não resolvem todos os problemas do desenvolvimento ( não existe bala de prata ), agora vamos ver outras vantagens que os padrões tem.

**Reusando padrões conseguimos prevenir problemas e diminuir as dúvidas que causam grandes problemas ao processo de desenvolvimento:**
Isso significa que quando o código é construido a partir de padrões comprovados nós conseguimos ter um código que com o tempo não irá se tornar
alguem insustentável ou macarrônico, usando padrões diminuimos o tempo de desenvolvimento, padronizamos o código e não perdemos tempo reinventando a roda. **Com isso não precisamos nos focar em algum problema específico.** isso é uma maneira generalizada de resolver os problemas, os padrões de projeto
já foram testados pela comunidade e foram desenvolvidos com anos e anos de estudo por parte de toda a comunidade criando soluções melhorando os modelos
e com isso podemos dedicar mais tempo a problemas específicos da nossa aplicação.

**Certamente o uso de padrões diminui o tamanho global do nosso código:** com isso evitamos redudância e nos da um encorajamento
para olhar o código mais de perto procurando reduzir repetições ou o número de funções que executam processos semelhantes em favor
de uma única função generalizada, o tamanho global do nosso código também pode ser diminuido isso é conhecido como DRY.


**Padrões adicionam aos desenvolvedores vocabulário, com uma comunicação mais rapida**.

**Padrões são frequentimente usados e com isso são melhorados com o passar do tempo, a comunidade cria e desenvolve os padrões:**
Em alguns caso são criados novos padrões de projeto, os padrões podem ser alterados para se adequar a nossos problemas.

#Nós já estamos usando padrões diariamente

Para entendermos como o uso de padrões pode ser simples, vamos rever um problema com elemento de seleção que o jQuery se propoem a resolver.

Imagine que nos temos um script que vai varrer elementos DOM a procura da classe "foo" nos vamos incrementar um contador. qual é a
maneira mais eficiente para percorrer essa coleção de elementos? Bem temos algumas diferentes formas de resolver esse problema vamos
ver algumas:
