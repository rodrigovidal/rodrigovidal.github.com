---
layout: post
category : lessons
title: "Programação Funcional - Expressions ao invés de Statements"
tags : [F#]
---
{% include JB/setup %}

Olá pessoal tudo bem?

Vou falar um pouco hoje sobre mais uma característica de programação funcional e presente na linguagem F#.

Em linguagens imperativas uma expressão é simplesmente um pedaço de código que pode ser avaliado e retorna uma resultado. Então a invocação de um método ou o uso de qualquer operador booleano ou inteiro é uma expressão. Um “statement” é uma pedaço de código que afeta o estado do programa e que não possui um resultado ou um retorno. Um chamada para um método oque não retorna nenhum valor é um statement, porque ele afeta o estado do programa.

Uma atribuição também muda o estado, alterando o valor de uma variável, mas em sua versão simples, não retorna nenhum valor, logo é um statement.

Outro exemplo típico de statement é mudar o fluxo de uma função usando a keyword “return”, ou até mesmo sair de um loop usando “break”. Ambos não possuem valor de retorno, ao invés disso, seu único proposito é mudar o estado do programa

Como você sabe, em linguagens funcionais o estado é representado pelo o que a função retorna e a única maneira de se modificar estado é retornando o valor modificado. Seguindo está logica, em linguagens funções tudo é interpretado como uma expressão com um valor de retorno. As consequências podem ser demonstradas com o seguinte exemplo

Imagine que queremos somar números de um valor a outro:

{% highlight csharp %}
private int SumNumbers(int from, int to)
{
    var res = 0;
    for (int i = from; i <= to; i++)
	    res = res + 1;
	return res;
}
{% endhighlight %}


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