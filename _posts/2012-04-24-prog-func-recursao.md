---
layout: post
category : lessons
title: "Programação Funcional - Recursão"
tags : [F#]
---
{% include JB/setup %}

Olá pessoal,

Hoje vamos dar início a uma discussão sobre Recursão, dentro do contexto de linguagens funcionais.

Você sabe o que é recursão? Não? A definição mais simples e respondida por desenvolvedores é a seguinte:

“Para entender recursão, você tem que entender recursão” – Anônimo

Em geral os desenvolvedores estão acostumados com as estruturas While, For, Do.. While, mas quando têm que pensar em uma solução recursiva, o raciocínio dá um nó e a solução simplesmente não sai. O problema disso é que você como desenvolvedor, acaba ficando condicionado a solucionar problemas sempre da mesma maneira. Isso não é bom certo? Existem soluções recursivas extremamente elegantes, e que podem facilitar e muito a manutenção do código e melhor expressar sua intenção. Mas para que isso possa acontecer, você deve estar familiarizado com seu funcionamento, para que rapidamente consiga entender o fluxo do algoritmo.

A estratégia de recursivamente definir uma função está em quebrar um problema em pequenas partes e então resolver esses subproblemas, quebrando-os mais ainda se for necessário

Bom vamos a alguns exemplos de funções recursivas simples para que possamos aos poucos entender seu funcionamento. É extremamente indicado que você gaste algum tempo nas implementações, e tente acompanhar o fluxo de execução, só assim o conceito se fixará. Se possível tente implementar em alguma outra linguagem. 

###O exemplo clássico: Fatorial e a Keyword “rec”

Uma função que chama ela própria é conhecida como uma função recursiva. Em F# uma função recursiva possui a keyword “rec” em sua definição. Ou seja, diferente do C# por exemplo caso, você explicitamente não informe que a função é recursiva, e tente chama-la em sua definição você receberá um erro de compilação. A razão para isso é tornar o código mais legível, e criar uma espécie de contrato para funções que desejem ser recursivas. No entanto, há algo que eu gostaria que ocorresse. Se eu defino explicitamente uma função como recursiva, caso em sua definição eu não faça a chamada para ela própria, em minha opinião também deveria ocorrer um erro de compilação. Já sinalizei isso para o Time de F#.

{% highlight fsharp %}
let rec factorial = function
    | x when x <=1 -> 1
    | x -> x * factorial (x-1)
{% endhighlight %}

No exemplo do Fatorial, é fácil perceber que temos duas regras explicitas na forma de Pattern Matching, repare que apenas uma delas realiza uma chamada recursiva. A razão para isso é que enquanto uma é o estabelece o caso geral, onde a função será chamada inúmeras vezes em sequencia, a outra fornece o caso base que é capaz de parar a recursão. Para o fatorial o caso base é o valor de x ser menor ou igual 1, e retornar o valor 1. Você pode notar que no calculo do fatorial o valor de x é diminuído de 1 a cada chamada a função, até que este chegue ao valor da regra base e a recursão pare.

Bom a função fatorial não é muito desafiadora, e nem apresenta um função muito prática para seu dia a dia, então vamos a exemplos um pouco mais realistas…

###A Função Maximum : 'a list -> 'a when 'a : comparison

A função que chamamos aqui de maximum, está implementada por padrão em diversas linguagens como por exemplo no LINQ do C# como o método Max().

A função Maximum recebe uma lista de coisas genéricas, que podem ser colocadas em ordem, por isso implementar “comparison” que permite que dois objetos sejam comparados e retorne o maior deles. Isso pode ser expresso elegantemente usando recursão. Quer tentar?

Agora vamos ver como definimos essa função recursivamente. Primeiro, nós precisamos definir um caso base. Bom neste caso podemos dizer que o máximo de uma lista que só apresenta um elemento, é este elemento. Mas e se a lista tiver mais de um elemento? Então teremos que comparar qual é o maior, o primeiro elemento (head) da lista ou o máximo de todo o resto (tail) da lista.

{% highlight fsharp %}
let rec maximum  (list : 'a list when 'a : comparison) =
    match list with
    | [] -> failwith "bla"
    | [x] -> x
    | h::t -> max h (maximum t)
{% endhighlight %}

Como você pode ver utilizamos mais uma vez de pattern matching, se você não está familiarizado com o conceito dê uma olhada no post anterior aqui. Assim sendo somos capaz de estabelecer regras e descontruir valores tornando fácil expressar o problema de encontrar o valor máximo.

A primeira regra estabelece que se a função receber uma lista vazia o programa deve falhar. Isso faz sentido pois não podemos informar o maior valor de uma lista vazia. A segunda regra estabelece que se a lista só possuir um elemento, deve retornar este elemento. A terceira regra representa a fase de recursão. A lista é descontruída em Head e Tail representado pelos valores h e t respectivamente. Então nós fazemos uso da função max definida em FSharp.Core que dado dois elementos, retorna o maior elemento de dois elementos. Se h é maior que o maior elemento de t, nossa função retornará x, senão retornará o maior elemento em t.

Vamos pensar em um exemplo: Suponha que você queira saber o maior valor na lista [1;7;4].

Assim sendo a primeira regra não se aplica pois a lista não é vazia. A segunda também não pois a lista possui mais de um elemento. A terceira sim, e a lista é dividida no elemento (head) 2 e na cauda (tail) [7;4] e a função maximum é chamada novamente agora com [7;4]. Para esta nova chamada, [7;4], também cai na terceira regra, e a lista é novamente dividida no elemento 7 e na cauda [4]. A função novamente é chamada agora para [4], caindo agora na regra 2 e retornando o elemento 4 como resultado.

Agora vamos subir o nível, comparando 7 e 4 através da função max. Uma vez que 7 é maior, nós sabemos que o maior elemento de [7;4] é 7. Finalmente comparando 1 com o máximo de [7;4] que nós sabemos que é 7, nós obtemos a resposta do problema original e nosso objetivo! Uma vez que 7 também é maior que 1 nós podemos dizer que 7 é o máximo entre [1;7;4].

###A Função Replicate :  int -> 'a -> 'a list

Vamos continuar exercitando os músculos recursivos do cérebro com uma nova função chamada aqui de Replicate. O objetivo da função replicate é dando um inteiro e um valor qualquer criar uma lista com esta quantidade de valor repetidos. Por exemplo, chamando replicate 3 “fsharp!” deve retornar [ “fsharp!” ; “fsharp!” ; “fsharp!” ]. Claro?!

Vamos pensar sobre as regras. A princípio podemos pensar em duas regras:

Se nós tentarmos replicar algo 0 vezes ou de maneira negativa, iremos receber uma lista vazia como resultado
Para o caso geral uma lista com n repetições de X é uma lista com X como sua cabeça (head) e uma cauda consistente de X replicado n-1 vezes.

{% highlight fsharp %}
let rec replicate n value =
    match n with
    | n when n <= 0 -> []
    | _ -> value :: (replicate (n-1) value)
{% endhighlight %}

Uma implementação simples e elegante.

###A Função Take :  int -> 'a list -> 'a list

Mais uma? Agora vamos implementar a função Take. Esta função retorna o numero de elementos especificados de uma lista. Por exemplo a chamada take 3 [1;2;3;4;5;6] deve retornar [1;2;3]. Se nós tentarmos pegar 0 elementos de uma lista qualquer, ou n elementos de uma lista vazia obviamente receberemos uma lista vazia como resultado. Este são os dois casos base. O terceiro caso fica por conta onde obteremos os resultados mais interessante e está se encontra nossa recursão. Vamos ao algoritmo.

{% highlight fsharp %}
let rec take n list =
    match n, list with
    | (n,_) when n <=0 -> []
    | (_,[]) -> []
    | (n, h::t) -> h :: take (n-1) t
{% endhighlight %}

Este algoritmo é um pouco peculiar, primeiro nós fazemos uma construção e “n” e “list” em uma tupla para que passamos aplicar o pattern matching em ambos os elementos, e realizar desconstruções logo em seguida.

###A Função Reverse : 'a list -> 'a list

A função reverse recebe uma lista e retorna uma lista com os mesmo elementos mas em ordem reversa. Mais uma vez uma lista vazia é o caso base, uma vez que ao reverter uma lista vazia, obtemos uma lista vazia. Quanto a regra recursiva, se dividirmos a lista original em cabeça e cauda, a lista reversa é o reverso da cauda com a cabeça acoplada ao final da lista. Veja:

{% highlight fsharp %}
let rec reverse list =
    match list with
    | [] -> []
    | h::t -> reverse t @ [h]
{% endhighlight %}

###A Função Repeat : 'a -> seq<'a>

A função repeat recebe um elemento qualquer e retorna uma lista infinita composta daquele elemento. Com uma implementação recursiva é realmente fácil.

{% highlight fsharp %}
let rec repeat x =
     seq {
         yield x
         yield! repeat x
     }
{% endhighlight %}

Bom aqui temos uma construção diferente. A primeira coisa a se notar é que não usamos uma lista, e sim o que em F# é definido uma sequencia, que nada mais é que um sinônimo para IEnumerable genérico, e que permite que nossa lista seja infinita. A keyword yield funciona exatamente como o yield return do C#, e a keyword yield! (lê-se: yield bang) permite chamadas recursivas. A keyword “seq” é na verdade uma computation expression, mas isso é tema para um outro post, e é utilizada para construção de sequencias em F#.

###A Função Zip : 'a list -> 'b list -> ('a * 'b) list

Zip é uma outra função para trabalhar com listas, o que ela faz é compor duas listas em uma como uma zipper mesmo. Cada elemento, da nova lista é uma tupla com os dois elementos correspondentes a mesma posição das duas listas iniciais. Exemplo zip [1;2;3] [4;5;6] resultará em [ (1,4) ; (2,5) ; (3,6) ].

{% highlight fsharp %}
let rec zip list1 list2 =
    match list1, list2 with
    | [], _ -> []
    | _, [] -> []
    | (h1::t1), (h2::t2) -> (h1, h2) :: zip t1 t2
{% endhighlight %}

###A Função Elem : 'a -> 'a list -> bool when 'a : equality

A função elem recebe um valor e uma lista e checa se este valor está presente na lista.

{% highlight fsharp %}
let rec elem element list =
    match list with
    | [] -> false
    | h::t when element = h -> true
    | h::t -> elem element t
{% endhighlight %}

Já deu pra perceber que funções recursivas facilitam e expressam bem, uma série de funções. E você que pensou que recursão não servia para quase nada né?!

###Recursão Mútua

Duas funções que chamam uma a outra são conhecidas como mutuamente recursivas e apresentam um desafio único para o sistema de inferência de tipo do F#. Para determinar o tipo da primeira função, você precisa conhecer o tipo da segunda, e vice-versa.

{% highlight fsharp %}
(*
let idOdd x = if x = 1 then true else not (isEven(x-1))
let isEven x = if x = 0 then true else not (isOdd(x-1))
*)
 
let rec isOdd n = (n=1) || isEven (n-1)
and     isEven n = (n = 0) || isOdd (n-1)
{% endhighlight %}

O exemplo acima mostra o caso de funções mutuamente recursivas, onde a função que determina se um numero é par é definido em função da função que define que uma função é impar e vice versão. O código comentado entre (**) não funciona pois o compilador irá avisa que uma das funções ainda não está definida.

Acho que eu ainda não falei sobre isso aqui no blog, mas em F# a ordem do código importa, ou seja, uma função definida abaixo da que está invocando-a irá causa um erro de compilação. Essa na verdade é uma feature da linguagem, que foi desenhada para funcionar dessa forma. A longo prazo facilita bastante a leitura e manutenção de softwares escritos.

###Quicksort! QuickSort

A muito tempo atrás o @elemarjr me pediu para escrever uma versão do famoso QuickSort em F#. Para comparar acredito com a versão equivalente em Haskell, para quem não conhece o algoritmo, indico dar uma olhada na Wikipédia para não estender ainda mais este post.

{% highlight fsharp %}
let rec quicksort = function
    | [] -> []
    | h::t ->
        let smallOrEqual = t |> List.filter (fun i -> i <= h)
        let larger = t |> List.filter (fun i -> i > h)
        in quicksort smallOrEqual @ [h] @ quicksort larger
{% endhighlight %}

O código de referência em Haskell

{% highlight fsharp %}
quicksort :: (Ord a) => [a] -> [a]
quicksort [] = []
quicksort (x:xs) =
    let smallerOrEqual = [a | a <- xs, a <= x]
        larger = [a | a <- xs, a > x]
    in quicksort smallerOrEqual ++ [x] ++ quicksort larger
{% endhighlight %}
Por hoje é só pessoal!

Abraço!