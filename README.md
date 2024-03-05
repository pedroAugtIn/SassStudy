#Minhas anotações sobre o estudo de Sass

Sass é uma linguagem de folhas de estilo que estende o CSS com funcionalidades adicionais. Ele oferece uma sintaxe mais poderosa e expressiva para escrever estilos CSS, permitindo que você escreva código css de forma mais eficiente e organizada.
Observações importantes:
Sass é um compilador de CSS, da mesma forma que typescript é para javascript.
É necessário instalá-lo:
npm install -g sass
Os arquivos sass são .scss
Após escrever um código sass, é necessário compila-lo em CSS. Podemos fazer isso utilizando o comando ‘sass’ no terminal. No diretório de nosso arquivo ‘.scss’ temos que executar o seguinte comando para compilar:
sass input.scss output.css
Substituindo ‘input.css’ pelo nome do nosso arquivo sass e ‘output.css’ pelo nome do arquivo css de saída (escolhemos o nome do arquivo css aqui, não precisa ser criado nenhuma css anteriormente). Isso criará um arquivo css compilado a partir do código sass. Ex:
 
Após compilar, basta linkar em nosso html nosso arquivo css.
Sempre que alterarmos nosso arquivo sass é necessário recompilarmos para aplicar as alterações em nosso html.

Uma das diferenças entra sass e o convencional css é a possibilidade de definir variáveis.
Elas são declaradas e armazenam dados, similar ao javascript.
Em javascript variáveis são declaradas com let e const. Em sass variáveis iniciam com $ seguida do nome da variável.
Por exemplo:
$main-fonts: Arial, sans-serif;
$headings-color: green;

E para usar as variáveis:
h1 {
  font-family: $main-fonts;
  color: $headings-color;
}

Um dos benefícios da utilização de variáveis é a praticidade para alterar diversos momentos de uma única vez, por exemplo, se tenho vários elementos com o mesmo padrão de cor, se fosse existir alguma alteração no projeto teria que alterar uma à uma em suas propriedade css. Com a utilização da variável basta alterar a regra da variável que todos os elementos que a utilizam são alterados automaticamente.

Sass também permite agruparmos regras de CSS, o que é uma ótima forma de organizarmos nosso style sheet.
Ex:
Normalmente selecionamos os elementos individualmente:
article {
  height: 200px;
}

article p {
  color: white;
}

article ul {
  color: blue;
}

Através do Sass podemos simplesmente fazer:
article {
  height: 200px;

  p {
    color: white;
  }

  ul {
    color: blue;
  }
}

Em Sass um “mixin” é um grupo de declarações CSS que podem ser reutilizadas.
Mixins são como funções para css. Vejamos uma:
@mixin box-shadow($x, $y, $blur, $c){ 
  -webkit-box-shadow: $x $y $blur $c;
  -moz-box-shadow: $x $y $blur $c;
  -ms-box-shadow: $x $y $blur $c;
  box-shadow: $x $y $blur $c;
}

A definição começa com @mixin seguida de um nome customizado.
Os parâmetros, conforme utilizados no exemplo acima, são opcionais.
Agora, toda vez que uma box-shadow for necessária, uma simples linha de código aplicando nosso mixin substituí todo o código. Um mixin é aplicado através de @include.
Ex:
div {
  @include box-shadow(0px, 0px, 4px, #fff);
}

Sass também inclui o diretório @if, semelhante ao utilizado em javascript.
Ex:
@mixin make-bold($bool) {
  @if $bool == true {
    font-weight: bold;
  }
}

E como no javascript, o @else if e @else também são aplicáveis:
@mixin text-effect($val) {
  @if $val == danger {
    color: red;
  }
  @else if $val == alert {
    color: yellow;
  }
  @else if $val == success {
    color: green;
  }
  @else {
    color: black;
  }
}

O diretório @for adiciona estilos em loop, muito semelhante ao “for” do javascript.
@for é utilizado de duas maineiras: Do começo através do fim, contando o fim. Ou do começo ao fim, excluindo o fim.
Exemplo incluindo o fim:
@for $i from 1 through 12 {
  .col-#{$i} { width: 100%/12 * $i; }
}

O #{$i} é a sintaxe para combinar a variável (i) com texto para fazer uma string. 


Em sass, @each itera sobre cada item de nossa lista. Em cada iteração, em cada iteração, à variável é assinalada o valor respectivo da lista ou map.
@each $color in blue, red, green {
  .#{$color}-text {color: $color;}}

Para o map a sintax é um pouco diferente:
$colors: (color1: blue, color2: red, color3: green);

@each $key, $color in $colors {
  .#{$color}-text {color: $color;}
}

Destaca-se que a variável $key é necessária para referenciar a key de nosso map. Ambos os exemplos acima, quando compilados, se transformando no seguinte css:
.blue-text {
  color: blue;
}

.red-text {
  color: red;
}

.green-text {
  color: green;
}


@while também é semelhante ao loop while de javascript, criando regras de css até que uma condição seja cumprida.
Vejamos exemplo:
$x: 1;
@while $x < 13 {
  .col-#{$x} { width: 100%/12 * $x;}
  $x: $x + 1;
}

Arquivos separados que armazenam segmentos de código CSS, em sass, são chamados de “partials”. Nomes para “partials” iniciam com (_), o que diz ao sass que trata-se de um pequeno segmento de css que não deve ser convertido a um css file.

Para trazer o código partial para outro arquivo sass, podemos utilizar o comando @import. Por exemplo, se todos os nossos “mixins” estão salvos em uma partial chamada “_mixin.scss”, e são necessárias no arquivo “main.scss”, podemos utilizá-las assim:
@import 'mixins'

Sass possui uma atribuição chamada extend, que facilita estendermos os atributos de css de um elemento a outro. Vejamos:
Digamos que temos um elemento de class=”panel” e o seguinte css:
.panel{
  background-color: red;
  height: 70px;
  border: 2px solid green;
}

Agora digamos que precisamos de outro elemento, só que com class=”big-panel”, com as mesmas atribuições de css de nosso panel, mas com atributos adicionais de width e font-size. Para isso podemos simplesmente fazer:
.big-panel{
  @extend .panel;
  width: 150px;
  font-size: 2em;
}

