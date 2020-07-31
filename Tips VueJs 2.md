# VueJs 2

## Detalhes e dicas:

- A estrutura é similar a do Polymer. Basta criar uma nova instancia do Vue object e usar as propriedades necessárias.

- Principais props: **el**, **data**, **methods, computed**, **watch.**

- É possível acessar qualquer propriedade de **data** e qualquer método de **methods** usando **this**. O Vue consegue ancorar essas propriedades acessíveis deixando disponíveis ao **this** (ele faz um tipo de proxy).

- Vue renderiza o html do template (aquele que estiver conectado a instância do Vue no html) internamente, faz o processamento e coloca no DOM. Devido a isso ao inspecionar um elemento HTML que o Vue esteja manipulando, não será possível ver referencias dessa “magica” acontecendo.

- Todos os dados em **data** ou **methods** podem ser acessados no HTML utilizando **string interpolation** e sem a necessidade do **this**.

- Não é possível utilizar **string interpolation** diretamente em NENHUM atributo html nativo. O parse não será feito. Para fazer isso é necessário utilizar a diretiva **v-bind:arg**, por exemplo: `<a v-bind:href="link" ></a>`

- Toda propriedade é re-renderizada quando o valor da propriedade é alterado. Para desabilitar esse comportamento basta usar a diretiva **v-once**. Dessa forma o elemento que tiver essa diretiva não terá a propriedade no **string interpolation** alterada com mudanças futuras de valor, se mantendo com o primeiro valor atribuído a ela.

- **Vue sempre processa textos em propriedades como textos** em seu comportamento default. Dito isso, se houver um propriedade em **data** que represente um template html por exemplo, será necessário usar a diretiva **v-html=”prop”** para que ele seja capaz de fazer o sanitaze desse HTML e renderiza-ló como de fato um HTML (e não texto). Vale ressaltar que isso deve ser utilizado com cuidado e apenas se vc conhecer o conteúdo do template e que ele não sofrerá mudanças feitas pelo usuário por exemplo.

- Para lidar com eventos bastas usar a diretiva **v-on:arg** passando em **arg** qualquer propriedade de evento que a tag em questão possua.

- Vue possui **modificadores** de eventos que permitem modificar algum evento. Para usar basta fazer **v-on:arg.modifier** (por exemplo v-on:mousemove.stop). É possível também encadear modificadores como por exemplo **v-on:keyup.enter.space** (nesse caso eventos que escutam quando as teclas enter ou space são pressionadas. 
  - Link para os [modificadores de evento](http://vuejs.org/v2/guide/events.html#Event-Modifiers)
  - Link para os [modificadores de key events](https://vuejs.org/v2/guide/events.html#Key-Modifiers)

- Dentro de qualquer lugar acessível a instancia do Vue é possível escrever javascript puro (desde que não sejam coisas como if, for, etc..). Ex: `<button v-on:click="counter++">Click me</button>`

- Entendendo o ponto acima, podemos então usar inclusive operação ternaria nos **string interpolation**

- Two-way data binding é possível através da diretiva **v-model**

- Para diminuir a dificuldade de manter uma prop atualizada com o mesmo valor em vários lugares diferentes da aplicação, nós podemos fazer uso da propriedade **computed**.

- Tudo que é guardado na nossa propriedade **computed** pode ser usado (e funciona igual) como uma propriedade de **data**. **Computed** é a melhor escolha quando precisamos guardar o resultado (cachear o valor).

- A propriedade **watch** funciona de uma maneira inversa da maioria. No objeto dela as propriedades **devem** **ser** as propriedades (qualquer propriedade, não apenas as que estão em **data**) que nós queremos **escutar as mudanças.** É sempre recomendando usar **computed** por ser mais otimizado e só usar **watch** em ultimo caso.

- Existem shorthands para as direticas, como **v-on:click** = **@click** ou **v-bind:href** = **:href.**

- Para condicionais temos as diretivas **v-if=”var”,** **v-else e v-else-if** que funcionam parecido com angularJs. O else/else-if deve vir na sequencia da tag com o if e será mostrado caso não caia na condição do if.
  - **v-else** só funciona se:
    - Estiver exatamente em baixo do elemento com **v-if** e for do mesmo tipo de tag (se a tag que tem v-if for uma section a tag com v-else **também precisa ser** section).

- Também podemos fazer a mesma coisa do if/else usando **v-show**. É recomendado sempre irmos pelo caminho do if/else pois os mesmos são mais performáticos. Usar o show apenas em ocasiões necessárias.

- Para tratar listas no HTML podemos usar **v-for**=”ingrediente in ingredients” (exemplo). Para pegarmos o valor basta usarmos em conjunto **string interpolation** dentro da tag. É possível alcançar qualquer valor do objeto ingrediente (se ele tiver subníveis).
  - Para termos index: =”(value, index) in ingredientes”
  - É possível fazer **v-for** encadeados. No encadeados podemos ter acesso a (value, key, index).
  - É possível iterar sobe números passados para o **v-for:** v-for=”n in 10”
  - Recomendado usar **:key=”ingridient”** no v-for para que o Vue por baixo dos panos não só guarde a posição do elemento mas o elemento em si (sem o key ele não faz isso por default). Isso faz diferença se em algum momento precisarmos reordenar a lista por exemplo. Ele não vai apenas mudar as posições na memoria e vai também carregar os itens junto, além das posições.