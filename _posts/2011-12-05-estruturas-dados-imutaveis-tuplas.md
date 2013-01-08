---
layout: post
category : lessons
title: "Estruturas de Dados Imutáveis - Tuplas"
tags : [Programação Funcional]
---
{% include JB/setup %}

Fala pessoal..

Hoje vamos falar sobre estruturas de dados imutáveis e o que isso quer dizer, e aproveitarei para mostrar como funcionam tuplas em F# e como seria uma implementação para Tuplas no C#.

Uma estrutura de dados imutável é uma estrutura em que o valor não muda após sua criação. Quando você declara uma estrutura que contém campos por exemplo, estes também devem ser imutáveis.

##O Tipo Tupla

A estrutura mais simples em F# e o tipo Tupla. Tupla é uma estrutura simples capaz de agrupar elementos de diferentes tipos. O exemplo a seguir mostra como criar uma tupla que possui dois elementos agrupados:

{% highlight haskell %}
let tp = (“O número é”, 42)
{% endhighlight %}

Como podemos ver criar uma tupla em F# é extremamente simples, nós escrevemos uma lista separada por virgulas dentro de parênteses. Mas vamos olhar com mais detalhes. O mecanismo de inferência de tipo da linguagem foi utilizado aqui, então você não precisa explicitamente dizer o tipo de sua tupla. O compilador infere que o primeiro elemento da sua tupla é uma string e o segundo é um inteiro, então podemos ler como: uma tupla que contem uma string como seu primeiro valor e um inteiro como seu segundo valor. Como podemos notar o valor de tp é do tipo string * int. Esta é a forma como é representado esta tupla, em F#.

##Operações com Tuplas

Vamos começar a usar algumas operações com tuplas.

{% highlight haskell %}
let mostrar tupla = 
	printfn "O primeiro elemento é %s e o segundo é %d" (fst tupla) (snd tupla)
 
do mostrar ("X", 7)
{% endhighlight %}

Neste exemplo criamos uma função chamada mostrar que recebe como parâmetro uma tupla. Nesta definição usamos a função printfn (que eu acredito que não precise de comentários) e passamos como argumento o primeiro e o segundo elemento da tupla. fst e snd são definidos como operadores em FSharp.Core.Operators e retornam o primeiro e o segundo elemento da tupla respectivamente (é uma referencia para First e Second).

Se analisarmos a função mostrar ela tem a seguinte definição (string * int –> unit). Ou seja ela recebe uma tupla de string e inteiro e retorna unit. Unit é um tipo do F# que indica que a função não retorna valor. Vemos que novamente o mecanismo de inferência de tipo entrou em cena, no entanto como pôde saber qual o tipo da tupla com que esta função sabe trabalhar? A resposta esta na string passada para a função printfn, o %s indica string e o %d indica um inteiro. A partir dai o compilador consegue inferir que estes são os tipos esperados pela função e a definição da tupla.

Tupla é uma estrutura imutável, ou seja, não podemos alterar o valor de um elemento da tupla, uma vez que a tupla tenha sido criada. Para alterarmos esse valor é necessário criar uma nova tupla. Vamos ver abaixo uma função que realiza essa operação.

{% highlight haskell %}
let tuplaComNovoValor novoValor tupla = 
	let (original1, original2) = tupla 
	(novoValor, original2)
{% endhighlight %}

Definimos a função tuplaComNovoValor que cria uma nova tupla alterando o valor do primeiro item. Isso é necessário pois como dissemos anteriormente, a estrutura tupla é imutável, para modifica-la somente criando outra com seus novos valores.

##Implementando um tipo Tupla em C#

Como sabemos no .NET 4 foi inserido um tipo Tuple<> genérico, que nos permite trabalhar de forma semelhante ao que fizemos com F#. No entanto meu objetivo aqui é mostrar de maneira mais simples como esse tipo foi/poderia ter sido implementado.

{% highlight csharp %}
public sealed class Tuple<T1, T2>
{
	private readonly T1 item1;
	private readonly T2 item2;
 
	public T1 Item1
	{
		get { return item1; }
	}
 
	public T2 Item2
	{
		get { return item2; }
	}
 
	public Tuple(T1 item1, T2 item2)
	{
		this.item1 = item1;
		this.item2 = item2;
	}
 
}
{% endhighlight %}

O que há de mais notável neste tipo é que ele é imutável. De maneira simples, nós marcamos todos os campos com o modificador readonly e provemos apenas propriedade do tipo Get. Campos somente leitura só podem ter valores atribuídos no construtor, o que significa que uma vez criada, seu estado interno não pode ser alterado, ou mutado, assim como seus valores armazenados internamente também são imutáveis.

##Implementando a mesma Tupla em F#

{% highlight fsharp %}
    type Tuple<'T1,'T2> =  
        new : 'T1 * 'T2 -> Tuple<'T1,'T2>
        member Item1 : 'T1 with get
        member Item2 : 'T2 with get
{% endhighlight %}

É claro que muito da implementação foi suprimida, pois não é do interesse deste post. Mas é o suficiente para expor o conceito apresentado.

Como podemos ver é muito simples trabalhar com Tuplas no F#. Em C# este trabalho se torna mais verboso, pois o mecanismo de inferência de tipo ainda não é tão poderoso.

Por hoje era isso pessoal!

Abraço!