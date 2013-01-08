---
layout: post
category : lessons
title: "Project Euler 1 em F#"
tags : [F#]
---
{% include JB/setup %}

Olá Developers! Vamos voltar a falar sobre F#.

Nos post anteriores vimos que a Programação Funcional, tem uma característica forte de ser mais declarativa do que imperativa. Para mostrar isso vamos resolver o primeiro problema do Projeto Euler.

O que é o Projeto Euler?

É uma serie de problemas desafiadores de programação envolvendo matemática e computação que irá requerer mais do que simplesmente conhecimento matemático para resolução. Embora matemática seje util para encontrar métodos eficientes e elegantes, o uso de computação e técnicas de programação são obrigatórias para resolver a maior parte dos problemas.

O Problema


”Se nós listarmos todos os números naturais abaixo de 10, que são multiplos de 3 ou 5, nós obtemos 3,5,6 e 9. A soma destes multiplos é 23.
Encontre a soma de todos os multiplos de 3 ou 5 abaixo de 1000.”  (Project Euler #1)


Os Testes

Resolvi o problema usando TDD. Para isso usei NUnit. No entanto estou utilizando uma DSL (Domain Specific Language) que estou desenvolvendo para tornar os testes mais legiveis, baseado em outras frameworks de testes para F# que não considerei maduros a uns 2 anos atrás. Quem quiser baixar o codigo e contribuir: https://github.com/rodrigovidal/FsxUnit Não se preocupe em entender tudo agora. Os testes revelam a linha que usei para resolver este problema bem simples.


{% highlight fsharp %}
module Tests
open NUnit.Framework
open FsxUnit
open ProjectEuler

type ``Project Euler 1 Tests``() = 
    [<Test>] member test.
        ``Given a empty list sum all members should be equal 0``() =
            [] |> sumAllMultiplesOf3Or5 |> should be (equal 0)
    [<Test>] member test.
        ``Given a list with 1 sum all members should be equal 0``() =
            [1] |> sumAllMultiplesOf3Or5 |> should be (equal 0)

    [<Test>] member test.
        ``Given a list with 2 sum all members should be equal 0``() =
            [2] |> sumAllMultiplesOf3Or5 |> should be (equal 0)
    
    [<Test>] member test.
        ``Given a list with 3 sum all members should be equal 3``() =
            [3] |> sumAllMultiplesOf3Or5 |> should be (equal 3)
    
    [<Test>] member test.
        ``Given a list with 4 sum all members should be equal 0``() =
            [4] |> sumAllMultiplesOf3Or5 |> should be (equal 0)

    [<Test>] member test.
        ``Given a list with 5 sum all members should be equal 5``() =
            [5] |> sumAllMultiplesOf3Or5 |> should be (equal 5)

    [<Test>] member test.
        ``Given a list with 1,2,3 sum all members``() =
            [1;2;3] |> sumAllMultiplesOf3Or5 |> should be (equal 3)

    [<Test>] member test.
        ``Given a list with 1,2,3,4,5 sum all members``() =
            [1;2;3;4;5] |> sumAllMultiplesOf3Or5 |> should be (equal 8)

    [<Test>] member test.
        ``Given a list with 1,2,3,4,5,6 sum all members``() =
            [1;2;3;4;5;6] |> sumAllMultiplesOf3Or5 |> should be (equal 14)

    [<Test>] member test.
        ``Given a list with 1,2,3,4,5,6,7,8,9,10 sum all members``() =
            [1;2;3;4;5;6;7;8;9;10] |> sumAllMultiplesOf3Or5 |> should be (equal 33)

    [<Test>] member test.
        ``Given a list with [1..999] sum all members``() =
            [1..999] |> sumAllMultiplesOf3Or5 |> should be (equal 233168)
{% endhighlight %}

##A Implementação Imperativa

{% highlight fsharp %}
let sumAllMultiplesOf3Or5 list = 
    let mutable total = 0
    for i in list do
        if i % 3 = 0 || i % 5 = 0 then
            total <- total + i
    total
{% endhighlight %}

Esta implementação faz uso de blocos que nós estamos acostumados a utilizar. For e If são examplos claro de uso do estilo imperativo. Podemos notar que dizemos o que o programa deve executar. Esta implementação diz “Como” deve ser executado.

Bem vamos ao codigo:

“sumAllMultiplesOf3Or5” é uma função e possui um parametro chamado “list”.
Os “espaços em branco” (tab) delimitam o escopo da função, assim como na linguagem Python, o que torna a linguagem mais sucinta e elegante. O padrão são 4 backspaces.
A keyword “mutable” cria um valor mutável (que seria equivalente a uma variável na maior parte dos casos).
O operador <- realiza uma atribuição em valores mutáveis.
O ultimo termo da função é o seu valor de retorno, dispensando o uso da palavra chave “return”.

##A Implementação Funcional
{% highlight fsharp %}
let isMultipleOf3Or5 x = x % 3 = 0 || x % 5 = 0

let sumAllMultiplesOf3Or5 list = List.sum (List.filter isMultipleOf3Or5 list)
{% endhighlight %}

Muito mais declarativo! Este código revela de maneira clara a sua intenção, deixando muito mais próximo o seu código do problema a ser resolvido, ou do seu dominio. #CleanCode

O modulo List possui funções especificas para lidar como listas, entre elas temos a função “filter” e “sum”. A função filter recebe como parametros uma nova função (esta que recebe um int e retorna um bool) e uma lista, e retorna uma nova lista. Enquanto a função “sum” recebe uma lista e retorna a soma de seus elementos.

Em programação funcional é extremamente importante conhecer as assinaturas das funções. Elas exprimem de maneira eficaz sua interface, e possibilitam seu entendendimento. #ficaDica.


##O Operador Pipe-Forward |>

Este é um dos operadores mais importantes do F#. Não vou entrar em muitos detalhes agora mas sua definição equivale ao seguinte:

{% highlight fsharp %}
let (|>) x f = f x
{% endhighlight %}

Sendo x um valor genérico, e f uma função.

O resultado final (refatorado) pode ser conferido abaixo:
{% highlight fsharp %}
let isMultipleOf3Or5 x = x % 3 = 0 || x % 5 = 0

let sumAllMultiplesOf3Or5Ref list = 
    list |> List.filter isMultipleOf3Or5 
         |> List.sum
{% endhighlight %}

Para quem quiser conferir estou hospedando o codigo dos problemas que irei resolver no github em: https://github.com/rodrigovidal/ProjectEuler

Por hoje era isso pessoal!

Abraço,
Rodrigo Vidal