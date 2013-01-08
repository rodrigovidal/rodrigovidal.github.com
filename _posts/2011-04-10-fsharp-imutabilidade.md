---
layout: post
category : lessons
title: "F# - Imutabilidade"
tags : [F#, C#]
---
{% include JB/setup %}

Pode-se notar que, eu de maneira suspeita não utilizei a palavra variável nos demais artigos sobre F#, mas sim a palavra “value”. A razão para isso é que em programação funcional, a estruturas que você declara são imutáveis por padrão, ou seja, não podem ser mudadas.

Se uma função em algum momento muda o estado do programa, como escrever em um arquivo ou mudar o valor de uma variavel global em memória, então é conhecido como “side effect” ou efeito colateral. Por exemplo, invocando a função printfn, ela retorna unit, mas tem o efeito colateral de escrever um texto na tela. De maneira similar, se uma função altera o valor de alguma coisa em memoria, este também é um efeito colateral.

Efeito colaterais não são de todo ruins, mas efeitos colaterais não intencionais são a raiz de muitos bugs. Até mesmo o mais bem intencionado dos programadores cometem erros se eles não estiverem cientes dos efeitos de uma função. Valores imutáveis ajudam a escrever código seguro porque você não pode estragar aquilo que não pode mudar.

Vamos entender o código abaixo que demonstra um exemplo de imutabilidade escrito em C#.

{% highlight csharp %}
public class Color
    {
        private readonly int red;
        private readonly int green;
        private readonly int blue;

        public Color(int red, int green, int blue)
        {
            this.red = red;
            this.green = green;
            this.blue = blue;
        }

        public Color MixColor(Color color)
        {
            var newRed = CalculateComponent(this.red, color.red);
            var newGreen = CalculateComponent(this.red, color.green);
            var newBlue = CalculateComponent(this.red, color.blue);

            return new Color(newRed, newGreen, newBlue);
        }

        private int CalculateComponent(int oldColor, int newColor)
        {
            return (oldColor + newColor)/2;
        }
    }
{% endhighlight %}

No código acima, podemos perceber que existe um construtor para o objeto Cor que recebe três parametros seguindo o padrão RGB. Seus atributos são readonly, e atribuidos somente durante a contrução do objeto. Existe também um método chamado MixColor que permite que uma Cor seje misturada à outra. Em um cenário imperativo, nós atualizariamos as propriedades do nosso objeto, no entanto isso não é possivel, pois nosso objeto é imutável. Ao invés disso, estamos criando um nova Cor, que representa uma instância da cor modificada e a retornando como resultado.  Desta forma nosso objeto é imutável, mais facil de testar e menos sujeito a efeitos colaterais.

Vamos para outro exemplo de Imutabilidade e Mutabilidade.

Dado o código abaixo.

{% highlight csharp %}
static void Main(string[] args)
        {
            Console.WriteLine("--System.String é imutável");
            string linguagem = "F#";
            string outraLinguagem = linguagem;
            outraLinguagem = "Boo";
            Console.WriteLine(linguagem);
            Console.WriteLine(outraLinguagem);

            Console.WriteLine("--StringBuilder é mutável");
            StringBuilder nome = new StringBuilder("Rodrigo Vidal");
            StringBuilder outroNome = nome;
            outroNome.Append(" - Don Syme");
            Console.WriteLine(nome.ToString());
            Console.WriteLine(outroNome.ToString());

            string teste1 = "Rodrigo";
            string teste2 = "Rodrigo";
            ToUpper(teste1);
            Console.WriteLine(teste1);
            Console.WriteLine(teste2);
            Console.ReadKey();
        }
{% endhighlight %}

Podemos notar que no codigo acima, ao modificarmos a variável outraLinguagem com o valor “Boo”, a variavel linguagem não foi alterada. A razão para isso é que o Tipo System.String é imutável, ou seja, toda vez que uma string é modificada, na realidade é criada uma nova string.

Uma vez entendido este conceito vamos para o F#. Como foi dito, anteriormente, por default tudo em F# é imutável.

Vamos aos exemplos.

{% highlight fsharp %}
let nome = "Rodrigo Vidal"
let nome = "Don Syme"
{% endhighlight %}

Exibirá como erro ao rodar no F# Interactive: error FS0037: Duplicate definition of value 'nome'
Até ai tudo bem.. não podemos ter duas definições de value com o mesmo nome. Próximo:

{% highlight fsharp %}
let nome = "Rodrigo Vidal"
nome = "Don Syme"
{% endhighlight %}

Este trecho terá erro em tempo de compilação, agora ele é irrelevante, mas ele fará você tentar usar o codigo abaixo para resolver o problema.

{% highlight fsharp %}
let nome = "Rodrigo Vidal"
nome <- "Don Syme"
{% endhighlight %}

Você pode imaginar que esta é a forma de se tentar “burlar” a imutabilidade, mas não é. Este codigo exibirá a mensagem error FS0027: This value is not mutable. Ou seja, o Value nome não é mutável, e esta operação não é permitida.

Bom para ressaltar a imutabilidade por default no F# é aplicado não só a Values mas também a todos os tipos de estrutura. Trabalhar com valores difere bastante de trabalhar com variáveis, então usar o termo Value ao invés de Variável, não é apenas uma questão de terminologia, mas sim de conceito. Por isso usarei essa terminologia durante os próximos posts. Usar valores imutáveis requer que nós expressemos muitos problemas de maneiras diferentes das que estamos acostumados. Nos próximos posts, poder ver na prática como isso se torna importante, para refatorações, testes unitários, lazy evaluation, e paralelismo, entre outros assuntos.,

Abraço,
Rodrigo Vidal