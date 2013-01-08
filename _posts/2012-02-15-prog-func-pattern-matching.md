---
layout: post
category : lessons
title: "Programação Funcional - Pattern Matching"
tags : [Programação Funcional]
---
{% include JB/setup %}

Olá pessoal! Vamos hoje falar sobre pattern matching que é um recurso extremamente utilizado em programação funcional. Veremos alguns exemplos escritos em F#. Este post surgiu devido a uma conversa com o amigo Bernardo Rosmaninho que demonstrou interesse no assunto, e como prometido, aqui está o post 

Segundo a Wikipédia

> In computer science, pattern matching is the act of checking some sequence of tokens for the presence of the 
> constituents of some pattern. In contrast to pattern recognition, the match usually has to be exact. The patterns 
> generally have the form of either sequences or tree structures. Uses of pattern matching include outputting the 
> locations (if any) of a pattern within a token sequence, to output some component of the matched pattern, and to 
> substitute the matching pattern with some other token sequence (i.e., search and replace).

Dado isso, eu diria que pattern matching é uma série de regras que serão executadas se o padrão informado encaixar com a entrada, permitindo também sua decomposição. Esta expressão então retorna o resultado da regra acionada. Como consequência, todas as regras de um pattern match devem retornar o mesmo tipo.

Pattern matching é de alguma maneira similar ao switch do C# e do C++, mas muito mais poderoso, como veremos agora.

##Você está com sorte!

{% highlight fsharp %}
let lucky = function
    | 7 -> "Você está com sorte!"
    | _ -> "Desculpe mas você está sem sorte!"
{% endhighlight %}

Como vemos este código está escrito em F# e define uma simples função, chamada "lucky". Está função possui duas regras, mas primeiro analizemos o tipo desta função. Executando o código acima no F# Interactive (fsi.exe) obtemos que esta função é do tipo (lucky : int -> string). Ou seja, ela recebe um valor do tipo int e retorna uma string. Note que não foi necessário definir um parâmetro para a função, e mesmo assim ela parece receber um. Este é um comportamento da keyword function utilizada, que faz com que o ultimo parâmetro seja o valor a ser comparado com a regra.

Bom então como dito anteriormente esta função possui duas regras. A primeira determina que caso o valor recebido como parâmetro do tipo inteiro seja o numero 7, então esta função retornara a string "Você está com sorte!". A segunda regra utiliza de um "underscore" para dizer que, qualquer outro valor que não satisfaça a regra anterior deve retornar "Desculpe mas você está sem sorte!". Cada regra se inicia com o pipe a esquerda. É importante notar que em F# a identação do código delimita o escopo, e o pipe deve estar identado corretamente para que seu código compile. Perceba também que, a regra é separada de seu resultado pelo operador "->".

Está função é bem simples e não apresenta nenhuma regra complexa, preparei mais algumas funções para nos familiarizarmos mais com o conceito de pattern matching. Veja se é capaz de entender o que a função abaixo faz:

{% highlight fsharp %}
let sayMe = function
    | 1 -> "One"
    | 2 -> "Two"
    | 3 -> "Three"
    | 4 -> "Four"
    | 5 -> "Five"
    | 6 -> "Six"
    | 7 -> "Seven"
    | _ -> "Not between 1 and 7"
{% endhighlight %}

Muito simples não é mesmo?

##Pattern Matching em uma Função Recursiva

Uma implementação simples de uma função recursiva usando pattern matching é o didático fatorial. (Eu ainda não tratei de função recursivas aqui no blog, mas o post está a caminho).

Note:
{% highlight fsharp %}
let rec factorial = function
    | 0 -> 1
    | n -> n * factorial (n-1)
{% endhighlight %}
Em F# para determinar que uma função é recursiva é necessário utilizar a keyword "rec" explicitamente! Executando esta função no FSI, obtemos o tipo (factorial : int -> int). Ou seja recebe um inteiro e retorna um inteiro.

A primeira regra determina que se o valor do parâmetro for 0 então será retornado o valor 1, caso esta regra não se aplique, e for passado um valor diferente de zerão então o parâmetro é multiplicado por uma nova chamada a função utilizando deste valor menos um. Lindo não?

##Desconstruindo Valores em Pattern Matching

Vamos supor que desejamos somar dois pontos. Segue
 {% highlight fsharp %}
let addVectors v1 v2 =
    (fst v1 + fst v2, snd v1 + snd v2)
{% endhighlight %}
O código acima apresenta uma solução, que não utiliza de pattern matching. Pela utilização das funções fst e snd, que trabalham com tuplas com compilador do F# consegue inferir que os valores v1 e v2 são tuplas e soma os primeiros valores de cada ponto, e depois o segundo valor, e adicionalmente neste processo cria uma nova tupla como resultado.

No entanto poderíamos utilizar pattern matching em um processo de desconstrução para obtermos estes valores. Vejamos

 {% highlight fsharp %}
let addVectors v1 v2 =
    match v1, v2 with
    | (x1, y1), (x2, y2) -> (x1 + x2, y1 + y2)
{% endhighlight %}
Nesta função utilizamos uma nova expression "match .. with" que é necessária quando a função precisa de mais de dois parâmetros.  Nesta função é necessário notar algumas coisas importantes.

Ela tem o tipo int * int -> int * int -> int * int , ou seja, recebe duas tuplas de inteiro e retorna uma tupla de inteiro
Construímos uma tupla com os valores v1 e v2 em match v1, v2 with, para utilizarmos ambos os valores na regra
Realizamos o processo de desconstrução da tupla na regra obtendo diretamente o valores de x1, x2, y1, y2 não sendo necessário o uso das funções.
Dada a desconstrução criamos uma nova tupla com os resultados somados.
O mecanismo de desconstrução torna pattern matching extremamente poderoso e permite uma melhor leitura da regra a ser aplicada, tornando seu código ainda mais legível. É também extremamente sucinta.

##Pattern Matching em Funções para Tuplas

Acima nós utilizamos as funções fst e snd para obter o primeiro e o segundo valor para tuplas. No entanto vamos olhar como estas funções podem ser implementadas?

{% highlight fsharp %}
let first = function
    | (x, _, _) -> x
let second = function
    | (_, x, _) -> x
let third = function
    | (_, _, x) -> x
{% endhighlight %}

As três funções acima trabalham com desconstrução de tuplas de 3 elementos. Dada uma tupla de 3 elementos esta é desconstruída, e o primeiro elemento é marcado para o valor de x, feito isso a função retorna o valor de x. Feito isso podemos obter o valor de qualquer dos 3 membro de uma tupla de 3 elementos.

##O operador lógico AND

Bom vamos construir uma função que funciona como o operador AND (&&)
{% highlight fsharp %}
let and x y =
    match x,y with
    | true, true -> true
    | true, false -> false
    | false, true -> false
    | false, false -> false
{% endhighlight %}
Devido ao mecanismo de inferência a função and recebe dois valores do tipo Booleana e retorna um novo valor.

O compilador do F# é muito poderoso como já percebemos.. Como exercício tente retirar a terceira regra e compilar o código. Você perceberá que o compilador do F# emitirá um warning avisando que uma possível regra não está sendo validada, mas irá compilar. Caso a execução caia em uma regra inexistente uma exceção do tipo MatchFailureException será lançada.

##Agrupando Padrões

Vamos descrever uma função que teste se um determinado caractér é uma vogal.

{% highlight fsharp %}
let isVowel c =
    match c with
    | 'a' | 'e' | 'i' | 'o' | 'u' -> true
    | _ -> false
{% endhighlight %}
Perceba que neste caso estamos agrupando padrões, não havendo necessidade de criarmos uma regra para cada uma das vogais, visto que elas compartilham da mesma regra.

##Guardas, Guardas!

Você já deve ter percebido como Pattern Matching é útil e permite as mais diversas construções. No entanto existem outros casos que podem ser necessários e para isso temos o que chamamos de Guards que é utilizado quando precisamos de alguma logica para saber se uma regra vai combinar.

No exemplo abaixo faremos uso de diversas construções de pattern matching. Veja:

{% highlight fsharp %}
let bmiIndex weight height =
    let bmi = weight / height ** 2.0
    let (skinny, normal, fat) = (18.5, 25.0, 30.0)
    match bmi with
    | _ when bmi <= skinny -> "You're underweight"
    | _ when bmi <= normal -> "You're normal"
    | _ when bmi <= fat -> "You're fat"
    | _ -> "You're a whale"
{% endhighlight %}

A função acima serve para calcular o índice de massa corpórea e informar se você está acima ou abaixo do peso. Fizemos uma função externa chamada bmiIndex que recebe os parâmetros peso e altura. Na primeira linha interna calculamos o que chamamos de bmi. Em seguida usamos pattern matching em tuplas para definir três valores. Após fazemos pattern matching no bmi e aplicamos a regra "menor ou igual". Para essa regra não ser repetida 3 vezes, eu a extraímos para a função chamada bmi, para melhorar a legibilidade. Para podemos aplicar essas regras utilizamos a keyword "when", que chamamos de when guards!

##Desconstrução de Listas

Assim como fizemos com tuplas é possível descontruir listas também.

{% highlight fsharp %}
let listStatus = function
    | [] -> "the list is empty"
    | x::[] -> "The list has one element:" + string x
    | x::y::[] -> "The list has two elements " + string x + " and " + string y
    | x::y::_ -> "Too long"
{% endhighlight %}

A função acima dá o status de uma lista. Se ela está vazia, se possui um elemento, se possui dois elementos, ou se possuir mais elementos. Perceba que o operador :: realiza a desconstrução da lista em elementos. No caso x combina para o primeiro elemento e y para o segundo, caso possua. Caso não lembre, o operador :: é usado para construir listas como em

 {% highlight fsharp %}
let list = 1::2::3::[]
{% endhighlight %}

Que retornaria [1;2;3].

Conclusão -> **Pattern Matching Everywhere**

Pattern matching como vimos é um mecanismo poderoso, e que nos permite diversas construções. Ele é simples e sucinto, o que facilita muito sua leitura. O que foi mostrado aqui não é tudo que é possível fazer com Pattern Matching. É também possível utilizar com estruturas de dados complexas criadas pelo desenvolvedor além de listas e tuplas. Em F# pattern matching é largamente utilizado até mesmo em estruturas que não parecem fazer uso desta técnica. Como no exemplo abaixo!
 {% highlight fsharp %}
let xs = [(1,3), (2,4), (3,2), (6,2), (7,1)]
xs |> List.map (fun (x,y) -> x+y)
{% endhighlight %}
Por hoje era isso!
Este código muda estado de variáveis e usa quatro Statements para resolver o problema

{% highlight csharp %}
private int SumNumbers(int from, int to)
{
    if (from > to) return 0;
    int sumRest = SumNumbers(from + 1, to);
    return from + sumRest;
}
{% endhighlight %}

Como podem ver todo o corpo do método é uma expressão única que retorna um valor. Em C# isso significa que o método deveria começar com return. E nós também não poderíamos usar return e nenhum outro lugar. Em C# if-then-else é um Statement, já em F# é uma Expressão. Note que são abordagens diferentes, e linguagens funcionais utilizam Expressions em detrimento de Statements.

Por hoje era isso pessoal!

Abraço,
Rodrigo Vidal