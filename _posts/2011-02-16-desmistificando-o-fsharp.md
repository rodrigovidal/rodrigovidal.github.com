---
layout: post
category : lessons
title: "Desmistificando o F#"
tags : [F#]
---
{% include JB/setup %}

Vamos começar a escrever nossos primeiros códigos utilizando F#. Para iniciar com assuntos mais complexos e interessantes, antes precisamos fundamentar as bases da linguagem. Ouvi de muito colegas da comunidade que F# é complicado, e difícil de aprender, então vamos tentar desmistificar ao longos dos próximos posts esta dificuldade e nos familiarizar com sua sintaxe.

##Configurando o ambiente

A Linguagem F# foi embarcada no Visual Studio 2010. Se tornando uma linguagem de primeira classe. Ao instalar o Visual Studio 2010 você tem a opção de instalar o F#. Caso não o tenha feito no momento da instalação não tem problema, você pode baixá-lo aqui e instalá-lo. O F# também funciona para você que usa Visual Studio 2008, basta utilizar o mesmo link anterior.

A maneira mais fácil de começar com F# através do VS2010 é criando um novo projeto. Para isso siga os passos File –> New Project –> Visual F# –> F# Application.

Pronto agora temos um simples projeto em branco onde podemos codificar.
{% highlight haskell %}

let message = "Olá F#"
printfn "%s" message

{% endhighlight %}

Embora este não seja o mais simples possível, não haveria algo muito interessante na possibilidade mais simples. Então vamos explorar este simples fragmento.

##Let Keyword

A palavra-chave “let” realiza um value-binding. Em um primeiro momento podemos pensar como uma declaração de um valor e uma atribuição. No entanto, há uma pequena diferença que neste momento não é perceptível, mas que falaremos mais a frente sobre.

Após o valor “Olá F#” ser atribuído ao símbolo “message”, o programa continua com uma chamada a função “printfn”. É importante notar que os argumentos passados para a funções em F# são geralmente separados apenas por espaços ao invés de se utilizar parênteses e virgulas. No entanto essa sintaxe também pode ser utilizada, quando fizer sentido. Mas aviso que na maior parte dos sistemas escritos em F# seu uso não é comum.

O Primeiro argumento da função “printfn” é uma string de formatação. No nosso exemplo ela especifica que nossa função terá apenas mais um argumento que será do tipo string. O tipo é especificado pelo %s na string de formatação e o compilador é perfeitamente capaz de checar o tipo.

Agora você pode perfeitamente executar seu primeiro aplicativo escrito em F#.

## F# Interactive

F# possui um ambiente interativo conhecido como F# Interactive que nada mais é do que um read-eval-print-loop (REPL).

Através dele você pode testar snnipets e trechos de código interativamente de maneira bem explorativa.

Você pode executar o F# Interative através do executável fsi.exe ou através da janela disponível dentro do próprio Visual Studio.  Neste ultimo caso use o atalho CTRL + ALT + F para acessar o console.

Para executar o código escrito no seu projeto diretamente na janela interativa, basta selecionar o código a ser executado e digitar ALT + ENTER.

Você pode ainda digitar diretamente o código no console. Para finalizar uma expressão no console você deve digitar “;;” no final como mostrado abaixo. Para que então o console possa processar o código que você inseriu.

<img src="{{BASE_PATH}}/imgs/interactive.png" />

Vamos a outro exemplo:
{% highlight haskell %}

let x = 5
let y = 2
let result = x + y

{% endhighlight %}

<img src="{{BASE_PATH}}/imgs/fsharp1.png" />

Repare que em ambos os exemplos o compilador conseguiu inferir o tipo dos valores embora não tenhamos explicitamente declarado. Isso se dá porque F# possui uma Inferência de Tipo extremamente poderosa, capaz de inferir mesmo em funções complexas.

Neste post vimos como é simples trabalhar com o ambiente básico do F#. Aos poucos vamos conhecendo as peculiaridades da linguagem e nos tornando produtivos com ela.

Abraço,

Rodrigo Vidal