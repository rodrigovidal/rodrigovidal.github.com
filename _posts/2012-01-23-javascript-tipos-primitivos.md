---
layout: post
category : lessons
title: "Javascript - Tipos Primitivos e Revelações"
tags : [JavaScript]
---
{% include JB/setup %}

Olá pessoal, tudo bem?

Vamos falar hoje sobre sobre a linguagem mandatória para escrever aplicações web. O Javascript. E quem não tem medo do Javascript? Até hoje vejo que muitos desenvolvedores não deram e ainda não dão importância, para esta linguagem. A maioria se limita a criar funções simples com outras funções logo abaixo em um código que beira o impossível de manter. Ou então se limita ao simples uso de plugins jQuery desenvolvidos por terceiros, para colocar uma calendário mais bonitinho na aplicação. Bom, já passou da hora de você saber que javascript é extremamente importante, e você deveria domina-la tanto quanto a linguagem server-side de preferência. Eu realmente espero que você domine está ultima. 

Não pretendo aqui contar a historia do JavaScript, ou justificar, a razão de ela possuir alguns problemas comportamentais, o importante é que você aprenda a escrever um bom código e tenha produtividade, sem esbarrar em bugs inesperados.

Bom, precisamos mudar isso, portanto, vamos ver hoje os tipos primitivos.

Qualquer valor que você use pertence a um tipo. Em javascript existem os seguintes tipos primitivos:

**Number**  - incluir números inteiros bem como de ponto flutuante.

**String** - qualquer numero de caracteres, por exemplo "a", "ab", "ab e c".

**Boolean** - assume apenas dois valores, true, ou false.

**Undefined** - quando você tenta acessar uma variável que não existe, você recebe o valor especial undefined. O mesmo acontece para uma variável que ainda não foi inicializada. O javascript na verdade a inicializa para undefined por debaixo dos panos.

**Null** - este é outro tipo especial que somente possui um valor, o valor null. Que significa, ausência de valor, vazio, ou nada. A diferença com undefined é que se uma variável tem o valor null, ela está definida, e só acontece quando é definida para null.

Qualquer valor que não pertença a nenhum dos 5 tipos primitivos listados acima é do tipo Object. 
Então basta lembrarmos que em Javascript o tipo dos dados pode ser:

Primitivo (os 5 listados acima)
Não-Primitivo (object)

###O operador typeof

Se você quer saber o tipo de uma variável ou de um valor, você pode utilizar o operador typeof. Este operador retorna uma string que representa o tipo do dado. Por exemplo o operador pode retornar "number", "string", "boolean", "undefined", "object". Vejamos alguns exemplos:

###Números

{% highlight javascript %}
//Inteiro
var n = 1234;
typeof n;
>> "number"
 
//Ponto Flutuante
var n = 1.23;
typeof n;
>> "number"
 
//Octal
var n = 0377;
typeof n;
>> "number"
 
//Hexa
var n = 0x00;
typeof n;
>> "number"
 
//Exponent
var n = 1e1;
typeof n;
>> "number"
{% endhighlight %}

Existem um valor especial em JavaScript chamado Infinity, veja bem, é um valor especial e não um tipo. Que representa como o próprio nome diz um valor Infinito, ou um valor que o JavaScript não consegue lidar.

{% highlight javascript %}
typeof Infinity;
>> "number"
{% endhighlight %}

Mas como obter este valor especial Infinity, bom, podemos.

{% highlight javascript %}
1e309;
>> Infinity
 
6/0;
>> Infinity
{% endhighlight %}
Bom, Infinity representa uma numero extremamente grande, e como representar um numero extremamente pequeno?

{% highlight javascript %}
-1/0
>> -Infinity
{% endhighlight %}
Bonito não?

Existe ainda um outro tipo especial, quando estamos lidando com números! O valor NaN! Quem nunca o viu, que atire a primeira pedra 

A primeira coisa que eu gosto de deixar claro sobre NaN é que ele é um numero! O que? Ahn? Isso mesmo! Veja..

{% highlight javascript %}
typeof NaN;
>> "number"
{% endhighlight %}

Oh my God! Provavelmente foi isso que você exclamou ao ver este exemplo! Você obtém o valor NaN quando tenta realizar uma operação que seria realizada sobre  inteiros, mas esta operação falha. Ai você obtém como retorno este tipo especial. Simples não?

Vejamos alguns exemplos:

{% highlight javascript %}
var a = 10 * "JavaScript";
a
>> NaN
 
1 + 2 + 3 + 4 + NaN;
>> NaN
{% endhighlight %}
Perfeito!

###Strings

Como vocês sabem strings são uma sequencia de caracteres. Em Javascript qualquer valor colocado entre aspas simples OU duplas é uma considerado uma string. Vejamos alguns exemplos:  

{% highlight javascript %}
typeof "1";
>> "string"
 
typeof "texto";
>> "string"
 
typeof 'texto';
>> "string"
 
typeof "";
>> "string"
{% endhighlight %}

###Conversões com String

Quando você usa uma string como uma operando em uma operação aritmética (com exceção da adição, veremos o porquê), a string é convertida em um numero por debaixo dos panos.

{% highlight javascript %}
var s = 3 * '1';
>> 3
typeof s
>> "integer"
 
var t = 3 + '1'
>> "31"
typeof t
>> "string"
{% endhighlight %}

Como podemos ver caso o operador seja adição (+) o numero 3 é convertido para string por implicitamente e o resultado é uma string "31". Para qualquer outro operador a string seria convertida em um numero como no primeiro exemplo acima.

Caso a conversão implícita falhe o resultado, como não poderia deixar de ser, será NaN.

{% highlight javascript %}
"texto" * 3;
>> NaN
{% endhighlight %}
###Boolean

Bom como vimos existem apenas dois valores que percentem ao tipo Boolean, true e false;

{% highlight javascript %}
var b = true;
typeof b;
>> "boolean"
 
var b = false;
typeof b;
>> "boolean"
{% endhighlight %}

Tudo convertido para boolean é true, com exceção dos seguintes 6 valores:

""
null
undefined
0
NaN
false

###Undefined

Você recebe undefined como valor quando você tenta usar uma variável que não existe ou que ainda não foi inicializada

{% highlight javascript %}
foo
>> foo is not defined
 
typeof foo
>> "undefined"
 
var x;
typeof x;
>> "undefined"
 
1 + undefined;
>> NaN
 
1 * undefined;
>> NaN
 
!!undefined
>> false
 
"" + undefined
>> "undefined"
{% endhighlight %}

###Null
O valor null não é atribuído pelo Javascript pode debaixo dos panos, ele somente pode ter vindo de uma inicialização do seu código
{% highlight javascript %}
var variável = null;
>> null
typeof variável
>> "object"
 
var i = 1 + null;
>> 1
 
1*null;
>> 0
{% endhighlight %}

Vemos que o valor null é considerado um objeto, o que pode parecer um pouco estranho, ter um objeto que é a representação de nada. Mas veremos mais sobre em futuros posts.

Bom por hoje era isso pessoal, espero ter desmistificado um pouco sobre os tipos primitivos no Javascript.

Abraço!