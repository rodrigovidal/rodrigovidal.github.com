---
layout: post
category : lessons
title: "Project Euler 2 em F#"
tags : [F#]
---
{% include JB/setup %}

Por hoje era isso pessoal!
Olá pessoal,

Estou caminhando com alguns problemas do Project Euler (mais lento do que eu gostaria), mas vamos lá. Desta vez resolvi o problema 2.

##O Problema

Cada novo termo da Sequencia de Fibonacci é gerado somando os dois termos anteriores. Começando por 1 e 2, os primeiros 10 termos seriam: 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, ...

Considerando os termos da Sequencia de Fibonacci cujos valores não excedam 4 milhões encontre a soma dos termos pares.

##A Solução

{% highlight fsharp %}
let fibonacci =
    (0, 1) 
    |> Seq.unfold(fun (current, next) -> Some(current, (next, current + next)))
    |> Seq.takeWhile(fun x -> x <= 4000000)
    |> Seq.filter(fun x -> x % 2 = 0)
    |> Seq.sum
{% endhighlight %}

Reparem que a solução é bem declarativa e simples.

Na linha 1 temos a definição do valor Fibonacci que não recebe nenhum parâmetro e retorna um inteiro. Podemos notar que nada disso foi informado no nosso código, no entanto o compilador consegue inferir o tipo da função baseado nas linhas seguintes.

Na linha 2 temos uma tupla (0,1), (muito elegante essa definição de Tuplas não?) que serve como estado inicial para nossa função Seq.unfold.

Na linha 3 temos o operador pipe-forward (|>) que como expliquei em um post anterior está usando nossa tupla e passando para a função Seq.unfold como ultimo parâmetro desta função. 

A função unfold está definida no modulo Seq, e é responsavel por gerar todos os números das serie de Fibonacci. Vamos analisar sua assinatura para podemos entender melhor o que essa função faz:

(‘State –> (‘T * ‘State) option) –>  ‘State –> seq<’T>

Uau! Agora ficou complexo hein! Bom percebi que ainda não expliquei em um blog post como ler esse tipo de assinatura, e ela trata de um elemento que também ainda não comentei por aqui. Como são assuntos extensos vou tratar em posts em seguida.

Mas então como podemos entender a função unfold?

Vamos tentar entender o seguinte: a função unfold recebe como segundo parâmetro um estado inicial, no nosso caso a tupla (0,1) e recebe como primeiro parâmetro uma função. Esta função recebe como parâmetro um estado inicial e retorna uma tupla de dois elementos. O primeiro elemento é do tipo inteiro, e o seguindo é do tipo tupla (também de dois elementos.

Com isso a função unfold consegue gerar toda a serie de Fibonacci. O tipo seq retornado é Lazy, então temos essa sequencia “infinita” sem problemas.

Na linha 4 temos uma função chamada takeWhile que recebe como parâmetro uma lambda function que é o equivalente a uma lambda expression no C#. Esta é denotada pela keyword fun. Essa keyword está filtrando os elementos abaixo de 4 milhões. toda vez que um novo estado é retornado, ele verifica esse pressuposto, caso atenda ele compõe a nova sequencia.

Na linha 5 já com a sequencia abaixo de 4 milhões filtramos todos números pares, usando a função filter e passando também uma lambda função como parâmetro.

Na linha 6 somamos todos os elementos da sequencia.

Com isso sabemos porque o compilador do F# consegue saber o tipo do valor Fibonacci. O resultado final retornado da função é do tipo inteiro, porque a função Seq.sum retorna um inteiro, uma vez que nossa sequencia é composta por inteiros.

Por hoje é isso pessoal.

Abraço,
Rodrigo Vidal