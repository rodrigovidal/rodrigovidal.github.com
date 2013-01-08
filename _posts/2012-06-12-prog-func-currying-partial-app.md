---
layout: post
category : lessons
title: "Programação Funcional - Currying e Aplicação Parcial de Funções"
tags : [F#]
---
{% include JB/setup %}

Olá pessoal,

Na ultima semana palestrei no DNAD12 onde falei sobre diversos conceitos de programação funcional. Com isso, muitas pessoas vieram falar comigo sobre o assunto, interessados em entender mais. Então, pretendo nas próximas semanas destrinchar grande parte dos assuntos que tratei na minha palestra. Escolhi este primeiro depois do @raphaelmolesim e posteriormente o @giovannibassi me perguntarem especificamente sobre ele. Para tentar sanar de uma vez por todas o entendimento sobre aplicação parcial e currying escrevo este post para todos  .

De uma maneira simples programação funcional é sobre aplicação de funções. Haskell Curry é o matemático cujo nome originou a expressão Currying, assim como a linguagem funcional Haskell. Currying permite ver todas as funções como membro de uma classe de funções que recebe apenas um parâmetro independente do número de argumentos necessário para realizar a computação em questão. Esta técnica permite então aplicação parcial de funções, como veremos neste post.

##Currying

Currying é uma técnica de transformação, que inicia com uma função que recebe múltiplos parâmetros e a converte em uma sequencia de funções, em que cada uma dessas funções aceita apenas um parâmetro por vez e retorna a próxima função da sequência.

IMAGEM

Para entender programação funcional é extremamente importante que você entenda essa notação de flechas.

Temos no primeiro caso, uma função que recebe dois parâmetros “a” e “b”, e retorna uma valor “c”. Repare que não estamos nos importando com tipos aqui, e adianto que currying se aplica para funções que recebem parâmetros de qualquer tipo. Então, esta função que inicialmente recebe dois parâmetros após sofrer a transformação Curry retorna uma nova função que recebe um parâmetro, e retorna uma nova função que recebe um parâmetro e retorna o valor final. Como assim?! Não entendi essa parte. Ainda não vi a diferença da primeira função para a segunda. Vamos lá!

Vamos imaginar uma função que calcula a adição entre dois inteiros.

Em C# poderíamos escrever da seguinte forma:

{% highlight csharp %}
int Add(int x, int y)
{
     return x + y;
}
{% endhighlight %}

No entanto também podemos escrever este método como uma função da seguinte forma:

 {% highlight csharp %}
Func<int,int, int> add = (x, y) => x + y
{% endhighlight %}

Que é representada em notação de flechas como:

**(int, int) –> int**

Esta função pode ser aplicada fazendo

 {% highlight csharp %}
var result = add(2,3);
{% endhighlight %}

O resultado é 5. Como sabemos no C#, para aplicar essa função temos de passar os dois argumentos, não podemos passar apenas um deles. Currying trata justamente disso.

A versão Curried correspondente a função add poderia se escrito da seguinte forma:

 {% highlight csharp %}
Func<int, Func<int, int>> add = x => y => x + y
{% endhighlight %}

Que é representada em notação de flechas como:

**int –> int –> int**

Para facilitar o entendimento, é importante dizer que em notação de flechas a notação é associativa à direita, que significa, que a notação anterior pode ser escrita como:

**int –> (int –> int)**

Ou seja.. uma função que recebe um inteiro, e retorna uma função que recebe um inteiro e retorna um inteiro.

Legal, será que nós conseguiríamos criar uma função que transformasse então no C# uma função que recebe dois parâmetros em uma versão Curried? Veja:

{% highlight csharp %}
public static Func<T1, Func<T2, TR>> Curry<T1, T2, TR>(this Func<T1, T2, TR> func)
{
    return par1 => par2 => func(par1, par2);
}
{% endhighlight %}

Essa seria uma maneira de definir um método genérico Curry que funciona para funções que recebem dois parâmetros.

Assim poderíamos fazer algo como:

{% highlight csharp %}
static int Add(int a, int b)
{
    return a + b;
}
 
static void Main(string[] args)
{
    Func<int,int,int> add = Add;
    Func<int,Func<int,int>> curriedAdd = add.Curry();
}
{% endhighlight %}

O que confunde, as vezes, os programadores é que em linguagens funcionais como Haskell e F# as funções são Curried por padrão. Ou seja, você não precisa fazer essa transformação, pois ela já é o padrão de aplicação de funções. Logo quando você define em F# ou Haskell (a sintaxe é a mesma):

 {% highlight fsharp %}
let add x y = x + y
{% endhighlight %}

que equivale a

 {% highlight fsharp %}
let add = fun x -> fun y -> x + y
{% endhighlight %}

ou em Haskell

 {% highlight haskell %}
let add = \x -> \y -> x + y
{% endhighlight %}

Você está definindo uma função do tipo

**int –> int –> int**

Os tipos são inferidos automaticamente, dispensando a sintaxe verbosa que o C# necessita, por não ser uma linguagem primariamente funcional.

##Aplicação Parcial de Funções

Aplicação parcial envolve passar menos argumentos para uma função que recebe múltiplos argumentos. Esse processo cria uma nova função que recebe menos argumentos.

IMAGEM

Obs.: Currying é um processo que habilita a usarmos aplicação parcial de funções. Na sessão anterior obtemos uma função em sua forma curried, agora iremos utilizá-la de forma parcial.

De maneira simples podemos aplicar parcialmente a função “add” definida na sessão anterior para criar uma nova função:

**C#**

{% highlight csharp %}
static void Main(string[] args)
{
    Func<int,int,int> add = Add;
    Func<int,Func<int,int>> curriedAdd = add.Curry();
    Func<int, int> add5 = curriedAdd(5);
    int result = add5(6);
}
{% endhighlight %}

**F#**

{% highlight fsharp %}
let add x y = x + y
 
let add5 = add 5
 
let result = (add 5) 6
{% endhighlight %}

Criei uma nova função add5 a partir da função add passando como parâmetro da minha função o valor 5. Simples não?

A aplicação de funções em F# ou Haskell é associativa a esquerda. Coloquei os parênteses, para facilitar o entendimento do que isso significa. Primeiro a função add é aplicada com apenas um parâmetro 5, essa aplicação retorna uma nova função, e então essa nova função é aplicada com o valor 6.

Repare, no exemplo em C# transformamos uma função Uncurried, em uma função Curried via função Curry. Com isso fomos capazes de aplicar a função parcialmente, passando apenas um dos parâmetros necessários. No exemplo em F# não é necessário fazer Curry, pois a função já está na forma Curried por padrão, logo conseguimos aplicá-la parcialmente com grande facilidade.

Outro exemplo:

**F#**

{% highlight fsharp %}
let result = [1..100]
                 |> List.map (fun x -> x * 2)
                 |> List.filter (fun x -> x < 50)
                 |> List.sum
{% endhighlight %}

Note que estamos aplicando as funções map e filter parcialmente. Isso acontece pois a função map assim como a filter recebem dois parâmetros, uma função (no caso do map uma projeção e no caso do filter um predicado) e uma lista como parâmetro. Em ambos os casos como resultado temos uma função do tipo:

**int list -> int list**

Ou seja, que recebe uma lista e retorna uma lista. Como já vimos em posts anteriores, o operador pipe-forward pode ser definido como:

{% highlight fsharp %}
let (|>) x f = f x
{% endhighlight %}

Assim ele aplica a função f que é nossa função parcial sobre o valor x que neste caso é nossa lista.

**C#**

Em C# é necessário fazer algumas modificações. Por exemplo não é possivel fazer curry do Select e do Filter diretamente pois eles são métodos de extensão. Ou seja operam sobre objetos. No entanto escrevi uma versão do select em uma classe estatica, para que possamos exemplificar, a pedido do Gustavo que comentou este post.

{% highlight csharp %}
public static class IEnumerableEx
{
    public static IEnumerable<R> Select<T, R>(IEnumerable<T> source, Func<T, R> f)
    {
        foreach (var item in source)
            yield return f(item);
    }
}
static void Main(string[] args)
{
    var lista = Enumerable.Range(1, 100);
    //Versão normal do select
    Func<IEnumerable<int>, Func<int,int>, IEnumerable<int>> select = IEnumerableEx.Select;
    //Versão curried
    Func<IEnumerable<int>, Func<Func<int, int>, IEnumerable<int>>> selectCurried = select.Curry();
    //Aplicação parcial - passando apenas 1 parametro ao invés de 2
    var listaSelecionada = selectCurried(lista);
    //Aplicação parcial - passando o segundo parametro que faltava
    var listaDobrada = listaSelecionada(x => x * 2);
    var listaTriplicada = listaSelecionada(x => x * 3);
    var listaAoQuadrado = listaSelecionada(x => x * x);
}
{% endhighlight %}

É facil perceber que este tipo de abordagem é bem mais verbosa que em F#, basicamente pelo C# não inferir o tipo dessas funções.

Este tipo de composição é largamente utilizada em programação funcional e nos permite reaproveitar e compor funções e outras estruturas de forma maestral. Bom pessoal espero que tenha ficado claro a separação dos dois conceitos, caso reste alguma dúvida por favor, não hesite em usar os comentários 

Abraço,

Rodrigo vidal