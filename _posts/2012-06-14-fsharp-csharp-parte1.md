---
layout: post
category : lessons
title: "F# Para Programadores C# - Parte 1"
tags : [F#, C#]
---
{% include JB/setup %}

Olá pessoal,

está semana estava conversando com meu querido amigo @elemarjr, e no meio da conversa, agora não lembro quem falou, surgiu o assunto de eu começar uma série nos moldes que ele está seguindo, passando Haskell para programadores C#. (Obs.: Esses posts serão ports da série do Elemar para F#, com seu devido consentimento).

Porque eu faria isso? Bom, Haskell até hoje foi a linguagem mais incrível que eu ja tive o prazer de estudar. Infelizmente, a tentativa da Microsoft de portar de Haskell para .net fracassou. No entanto, a Microsoft obteve sucesso ao lançar o F#, uma linguagem funcional que roda sobre o .NET Framework. Essa semana o @reegiss perguntou para o @elemarjr via twitter se havia uma maneira de integrar Haskell com .net. Como eu disse houveram algumas tentativas, mas nenhuma vingou como projeto. Então se você quer poder usar uma linguagem funcional com o .NET, eu recomendo F# ou Clojure.

F# tem uma abordagem mais pragmática que Haskell e integra de maneira suave com o C# (É possivel invocar código F# de código C# e vice-versa).

##Para começar…

Você deve acessar o F# Developer Center no MSDN e baixar a versão que atende o seu ambiente. É bom lembrar que F# roda muito bem sobre o Mono, em ambientes Unix e OSX.

Há também o site http://www.tryfsharp.org/ onde você consegue executar código F# em um REPL que roda no browser! Olha ai hein!

###Esquentando os motores…

Vamos escrever uma função que realiza o somatório de 1 até 100 em C#?

{% highlight fsharp %}
var soma = 0;
for (var i = 1; i <= 100; i++)
    soma += i;
{% endhighlight %}

Mais imperativo impossivel não? Vamos escrever isso de maneira mais declariva no C#, graças à injeção funcional que o C# levou na versão 3.0 

 {% highlight csharp %}
Enumerable.Range(1, 100).Sum();
{% endhighlight %}

Em F# é assim:

{% highlight fsharp %}
List.sum [1..100]
{% endhighlight %}

Fácil!

Como o @elemarjr disse, LINQ foi totalmente inspirado no paradigma funcional, ele diz em Haskell pois, o Erik Meijer, também conhecido como o pai do LINQ, é muito influeciado por Haskell. No entanto algumas abstrações ja estavam presentes mesmo antes disso em LISP.

Vamos ver como fazemos algumas operações com LINQ em F#.

###head/first

**C#**

 {% highlight csharp %}
new[] {1, 2, 3, 4, 5}.First();
{% endhighlight %}

**F#**

 {% highlight fsharp %}
List.head [1..5]
{% endhighlight %}

###tail/Skip(1)

**C#**

 {% highlight csharp %}
new[] {1, 2, 3, 4, 5}.Skip(1);
{% endhighlight %}

**F#**

 {% highlight fsharp %}
List.tail [1..5]
{% endhighlight %}

O que podemos notar é que:

C#, por ser uma linguagem orientada a objetos, coloca o “objeto” em destaque. Repare, a instrução começa com o objeto. Haskell, por outro lado, coloca a função em primeiro lugar. Por Elemar Jr.

Eu diria, de outra forma. Em linguagens OO, o coração, ou a menor unidade de abstração é o objeto, enquanto em linguagens funcionais a menor unidade de abstração é a função. Ou seja, a granularidade é mais alta.

###skip n / Skip(n)

**C#**

 {% highlight csharp %}
new[] {1, 2, 3, 4, 5}.Skip(3);
{% endhighlight %}

**F#**

 {% highlight fsharp %}
Seq.skip 3 [1..5]
{% endhighlight %}

###length / Count()

**C#**

 {% highlight csharp %}
new[] {1, 2, 3, 4, 5}.Count();
{% endhighlight %}

**F#**

 {% highlight fsharp %}
List.length [1..5]
{% endhighlight %}

###sum / Sum()

**C#**

 {% highlight csharp %}
new[] {1, 2, 3, 4, 5}.Sum();
{% endhighlight %}

**F#**

 {% highlight fsharp %}
List.sum [1..5]
{% endhighlight %}

###reduce / Aggregate(a, b => a * b)

**C#**

{% highlight csharp %}
new[] {1, 2, 3, 4, 5}.Aggregate((a, b) => a*b);
{% endhighlight %}

**F#**

 {% highlight fsharp %}
List.reduce (fun a b -> a * b) [1..5]
{% endhighlight %}

###@ / Union

**C#**

 {% highlight csharp %}
new[] {1, 2, 3}.Union(new[] {4, 5});
{% endhighlight %}

**F#**

 {% highlight fsharp %}
[1..3] @ [4;5]
{% endhighlight %}

###rev / Reverse()

**C#**

 {% highlight csharp %}
{1, 2, 3, 4, 5}.Reverse();
{% endhighlight %}

**F#**

 {% highlight fsharp %}
List.rev [1..5]
{% endhighlight %}

Para efeito de conhecimento apenas, quando o LINQ foi implementado no C#, essas funções já existiam no F#. Nessa época F# era uma projeto da Microsoft Research.

Abraço,

Rodrigo Vidal

