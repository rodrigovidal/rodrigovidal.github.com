---
layout: post
category : lessons
title: "F# Para Programadores C# - Parte 2"
tags : [F#, C#]
---
{% include JB/setup %}

Olá pessoal, vamos dar continuidade à série!

A parte 1 você encontra aqui! É indicado que você leia o post anterior antes de continuar.

Neste ponto eu gostaria de recomendar um excelente livro de F# para quem está começando –> Programming F# do Chris Smith.

##Aplicações de Funções vs. Invocação de Métodos

Em linguagens funcionais como F#, é comum usar o termo aplicação, para dizer que uma função será executada com um conjunto de argumentos. Enquanto no C# invocamos um método a partir de um objeto.

Para invocar um método de um objeto no C# devemos fazer:

{% highlight csharp %}
obj.Foo(x);
{% endhighlight %}
onde obj representa uma objeto qualquer. Enquanto em F# as funções podem ser aplicadas diretamente.

{% highlight fsharp %}
foo x
{% endhighlight %}

###Funções de Dois Argumentos

**C#**
{% highlight csharp %}
obj.Foo(a,b);
{% endhighlight %}

**F#**

{% highlight fsharp %}
foo a b
{% endhighlight %}
Repare que F# dispensa o uso de parênteses, de virgulas e do ponto e virgula no final, mantendo uma sintaxe mais sucinta.

O Resultado de uma função como argumento

**C#**

 {% highlight csharp %}
Foo(Fee(x));
Foo(a, Fee(b));
{% endhighlight %}

**F#**

 {% highlight fsharp %}
foo (fee x)
foo a (fee b)
{% endhighlight %}
Assim como em Haskell em F# os parênteses servem para alterar a prioridade de execução.

###Escrevendo suas próprias funções

Você pode agora criar uma arquivo hello.fsx, sendo FSX de FSharp Script. E adicione o seguinte código:

{% highlight fsharp %}
let add a b = a + b
let inc a = add a 1
let double x = x + x
let quadruple x = double (double x)
{% endhighlight %}

Caso você esteja usando o VS basta usando ALT + Enter para que o script rode no console interativo. Caso esteja usando o REPL na linha de comando basta colocar C:\Program Files (x86)\Microsoft SDKs\F#\3.0\Framework\v4.0 no path das suas variáveis de ambiente do Windows e digitar fsi .

**C#**

{% highlight csharp %}
Func<int, int, int> add = (a, b) => a + b;
Func<int, int> inc = a => add(a, 1);
Func<int, int> @double = x => x + x;
Func<int, int> quadruple = x => @double(@double(x));
{% endhighlight %}

Bem parecido, no entanto podemos perceber que o F# trata funções como membros de primeira classe. Pois ele consegue inferir com facilidade o tipo da função sem precisarmos definir o tipo. Enquanto isso o C# trata a função como um objeto, Func, e o mecanismo de inferência não funciona tão bem para resolver isso no C#. 

Por hoje era isso!

Abraço,
Rodrigo Vidal