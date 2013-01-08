---
layout: post
category : lessons
title: "Estruturas de Dados Imutáveis - Listas"
tags : [Eventos]
---
{% include JB/setup %}

Fala pessoal…

Dando continuidade ao post anterior, vamos hoje falar sobre uma estrutura extremamente importante para linguagens funcionais: Listas, e o que elas apresentam de tão diferente e marcante das estruturas de lista que conhecemos.

Se você está chegando agora confira também o post anterior onde falei sobre Tuplas aqui.

##O Tipo Lista

Enquanto tuplas agrupam valores em uma unica entidade, listas permitem que você ligue os dados de maneira a formar uma corrente. Uma lista pode ser vazia ou composta por um elemento e uma outra lista. Um nome especial para representar essa lista vazia pode ser “nil” e para cada elemento “cons cell” de “constructed list cell”. Na imagem abaixo vemos a representação gráfica de uma lista.

Cada cons cell contém um único valor e uma referência para o próximo elemento da lista que pode ser uma nova cons cell ou uma lista vazia.

Vamos ver alguns exemplos de construção de listas:
{% highlight haskell %}
let listaVazia = [] 
//val listaVazia : a list
 
let vogais = ["a";"e";"i";"o";"u"] 
//val vogais : string list = ["a"; "e"; "i"; "o"; "u"]
 
let numeros = [1;2;3;4;5;6] 
//val numeros : int list = [1; 2; 3; 4; 5; 6]]
{% endhighlight %}

O código acima é bem simples e acredito que não requeira explicação. Um ponto importante a notar é o mecanismo de inferência de tipo atuando novamente. No primeiro exemplo é construida uma lista vazia, e seu tipo é uma lista genérica. No segundo exemplo é construida uma lista de strings, e no terceiro uma lista de inteiros. É importante notar que todos os elementos de uma lista devem possuir o mesmo tipo, diferentemente das tuplas que vimos anteriormente que podem agregar diferentes tipos.

##Operadores de Listas

Em F# são definidos alguns operadores básicos para tratarmos com listas. As listas do F# estão definidas no modulo FSharp.List . Vamos ver algumas facilidades agora.

###List Ranges

Nos permite criar listas de maneira facilitada e de maneira menos verbosa.

{% highlight haskell %}
let range = [1..10] 
//val range : int list = [1; 2; 3; 4; 5; 6; 7; 8; 9; 10]
let charRange = ['a'..'f'] 
//val charRange : char list = ['a'; 'b'; 'c'; 'd'; 'e'; 'f']
{% endhighlight %}

List Comprehensions

{% highlight haskell %}
let prepost x = [ yield x-1; yield x; yield x+1] 
let result = prepost 10 
//val prepost : int -> int list 
//val result : int list = [9; 10; 11]
{% endhighlight %}

No exemplo acima criamos uma função chamada “prepost” que recebe um valor “x” e a partir disso cria uma lista com três valores, o antecessor, o proprio, e seu sucessor. Perceba que utilizamos a palavara chave yield, que em F# é o equivalente, como você ja deve ter imaginado, do nosso querido “yield return”.

{% highlight csharp %}
let multiplesOf3 x = [for i in 1..10 -> x * i] 
let result = multiplesOf3 5 
//val multiplesOf3 : int -> int list
//val result : int list = [5; 10; 15; 20; 25; 30; 35; 40; 45; 50]
{% endhighlight %}

Já neste exemplo criamos uma função que gera dez multiplos do valor passado como parametro, e utilizamos a expressão “for .. in .. –> “. É válido dizer que isso nos permite criar lógicas interessantes para construção de listas.

###O Operador Cons e Append

O operador cons é represtando por “::” na verdade é uma simplificação da função List.Cons, que coloca cada novo elemento como a nova “cabeça” da lista. O operador cons é associativo a direita, o que significa que ele compõe valores da direita para a esquerda.

{% highlight haskell %}
let numeros2 = 1 :: 2 :: 3 :: 4 :: 5 :: 6 :: [] 
//val numeros2 : int list = [1; 2; 3; 4; 5; 6]
 
let impares = [1;3;5;7;9] 
let pares = [0;2;4;6;8]
let todos = pares @ impares 
//val todos : int list = [0; 2; 4; 6; 8; 1; 3; 5; 7; 9]
{% endhighlight %}
O operador append, representado por “@”  é uma simplificação para a função List.append que transforma duas listas em uma.

###Algumas Funções do Módulo List

Duas funções importantissimas são a função List.head que retorna a “cabeça” da lista..

{% highlight haskell %}
let head = List.head [1..10] 
//val head : int = 1
{% endhighlight %}
.. e a função List.tail que retorna a cauda da lista, ou seja, todos os elementos a partir da cabeça.

{% highlight haskell %}
let tail = List.tail [1..10] 
//val tail : int list = [2; 3; 4; 5; 6; 7; 8; 9; 10]
{% endhighlight %}
Estas funções são essenciais quando estamos lidando com recursão e por isso resolvi apresentá-las aqui. Ja falamos bastante sobre como trabalhar com listas com F#, no entanto, o mais interessante ainda estar por vir. Você como um leitor atento deve ter reparado que estas listas não são as mesmas listas que acompanham o .NET Framework, ou as listas, que você está acostumado a utilizar com C# no namespace System.Collections.Generic, e o tipo List<>.

Mas então o que essas listas tem de diferente?

###Listas Imutáveis

Uma lista ser imutável significa que nós podemos construir uma lista, nós não podemos pegar uma lista existente e modificá-la; nós não podemos adicionar ou remover um elemento. Funções que precisam adicionar novos elementos ou removê-los sempre retornarão uma nova lista sem modificar a original, porque modificar uma lista é simplesmente impossivel. Isso está de acordo com o conceito de nós eliminarmos efeitos colaterais e indesejados em funções que manipulam listas.

Seguindo o post sobre tuplas vou mostrar como seria de maneira simples uma Lista imutável em C#

{% highlight csharp %}
public class FuncList<t>
{
    public bool IsEmpty { get; private set; }
    public T Head { get; private set; }
    public FuncList<t> Tail { get; private set; }
 
    public FuncList()
    {
        IsEmpty = true;
    }
 
    public FuncList(T head, FuncList<t> tail)
    {
        IsEmpty = false;
        Head = head;
        Tail = tail;
    }
}
 
public static class FuncList
{
    public static FuncList<t> Empty<t>()
    {
        return new FuncList<t>();
    }
 
    public static FuncList<t> Cons<t>(T head, FuncList<t> tail)
    {
        return new FuncList<t>(head, tail);
    }
}
{% endhighlight %}

A classe FuncList é generica, então ela pode armazenar valores de qualquer tipo. Ela possui uma propriedade IsEmpty que é True quando nós criamos uma lista vazia pelo construtor sem parâmetro. O segundo construtor recebe dois argumentos, o primeiro é a cabeça da lista e o segundo é a lista que segue como cauda para o novo valor. 
Ou seja o primeiro construtor equivale a lista vazia do F# [] e o segundo ao operador Cons "::"  do F#.

Agora como criar uma lista a partir desse novo tipo? Vemos abaixo:

{% highlight csharp %}
var numbers = FuncList.Cons(1, FuncList.Cons(2, FuncList.Cons(3, FuncList.Empty<int>())));
//Equivalente em F#
//let numbers = 1 :: 2 :: 3 :: []
{% endhighlight %}

Agora vamos criar um método que calcule a soma de todos os elementos de uma lista formada de inteiros

{% highlight csharp %}

public static int SumList(FuncList<int> numbers)
{
    return numbers.IsEmpty ? 0 : numbers.Head + SumList(numbers.Tail);
}
int sum = FuncList.SumList(numbers);

{% endhighlight %}

A função acima faz uso de recursão todos os elementos da lista. Caso a lista esteja vazia, retorna o valor zero, caso contrário retorna a cabeça + o resultado da função novamente sobre a cauda da lista, sucessivamente até encontrar como cauda apenas a lista vazia.

Bom pessoal, por hoje era isso!