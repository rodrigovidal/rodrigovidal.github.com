---
layout: post
category : lessons
title: "Javascript - É ou não igual?!"
tags : [JavaScript]
---
{% include JB/setup %}

Olá pessoal,

Continuando a série, sobre JS, vamos falar um pouco sobre dois operadores extremamente simples. Eles são capazes de comparar dois valores e dizer se eles são iguais ou não. Realmente simples não? Provavelmente, você sabe muito bem, como isso funciona na sua linguagem server-side, ao comparar dois valores, dois arrays, ou até mesmo dois objetos. No entanto, olhando o código que já tive a oportunidade de observar nas empresas, percebo que os desenvolvedores não entendem muito bem como isso se dá no JavaScript. Então vamos entender de uma vez por todas, se é ou não é igual.

A primeira coisa a se saber é que Javascript possui dois operadores de igualdade! O que? Dois? É isso mesmo. Você provavelmente utiliza um deles com bastante frequência: O "==". Agora você pode pensar, nossa que obvio. Isso eu já sabia. Mas você realmente sabe como este operador funciona? Você conhece seu irmão menos brincalhão? O "===" ?

Não. Eu não errei o operador acima, realmente são 3 (três) "igual" seguidos. O desconhecimento deste segundo operador é a origem de muitos bugs estarem estourando por ai, sem você realmente entender o porquê, e geralmente não tão simples de entender, principalmente, se seu código não está cobertos por testes. Espera! Testes no Javascript? (Mas isso é tema pra outro post).

Os operadores == e === checam se dois valores são iguais mas com duas definições diferentes de igualdade. Ambos os operadores aceitam operandos de qualquer tipo, e ambos retornam true se os operando forem iguais e false se eles forem diferentes. O operador === é conhecido como operador de igualdade estrita, e checa quando os dois operandos são idênticos, usando uma definição mais estrita. O operador == é conhecido como operador de igualdade, ele checa quando os dois operador são iguais usando uma definição mais relaxada que permite conversões de tipo.

Javascript suporta os operadores =, ==, ===. Podemos lê-los da seguinte forma: "é atribuído para", "é igual a", "é estritamente igual a" respectivamente.

Os operadores != e !== testam para o oposto exato dos operadores == e ===.

Objetos no Javascript são comparados por referência e não por valor. Um objeto é igual a ele mesmo, mas diferente de qualquer outro objeto. Se dois objetos distintos tem o mesmo numero de propriedades, com os mesmo nomes e mesmo valores eles ainda assim não são iguais. Dois arrays com os mesmo elementos, na mesma ordem não são iguais uns aos outros.

###Como funciona o ===

O operador === avalia os operandos e então os compara, sem realizar conversão de tipo, dada a seguinte regra:

Se os dois valores são de tipo diferentes, eles não são iguais.
Se os dois valores são null ou ambos são undefined, eles são iguais.
Se ambos os valores são o valor booleano true ou ambos são o valor booleano false, eles são iguais
Se um ou mais valores são NaN, eles não são iguais. O valor NaN não é nunca igual a nenhum valor, incluindo ele mesmo.
Se ambos os valores são números e tem o mesmo valor, eles são iguais; Se um valor é 0 e o outro -0 eles também são iguais.
Se ambos os valores são strings e contém exatamente os mesmos 16 bits, nas mesmas posições, então são iguais. Se as strings diferirem em tamanho ou posicionamento, não são iguais.
Se ambos os valores referirem para o mesmo objeto, array, ou função, eles são iguais. Se referem para objetos diferentes não são iguais, mesmo que ambos tenham propriedades idênticas.

###Como funciona o ==

O operador == é menos estrito. Se os valores dos operandos não forem do mesmo tipo, ele tenta algumas conversões de tipo e realiza a comparação novamente.

Se os valores tem o mesmo tipo, ele testa para igualdade estrita, como descrito acima, Se os valores forem estritamente iguais, eles são iguais.
Se os dois valores não são do mesmo tipo, eles ainda podem ser considerados iguais:

Se um dos valores é null e o outro é undefined, eles são iguais.
Se um dos valores é um numero e o outro uma string, converte a string para um numero e tenta realizar a comparação novamente, usando o valor convertido.
Se um é true, converte para 1 e tenta a comparação novamente, Se um é false, converte para 0 e tenta novamente.
Se um valor é um objeto e o outro é um numero ou string converte o objeto para um primitivo e tenta novamente a comparação. Um objeto é convertido para um valor primitivo, usando o método toString() ou valueOf().
Qualquer outra combinação de valores não são iguais.
Ufa! Não tão simples não é mesmo?

Segue alguns exemplos práticos que tenta permear algumas das regras acima

{% highlight javascript %}
// Operador === 
 
1 === "1"
>> false
 
null === null
>> true
 
undefined === undefined
>> true
 
true === true
>> true
 
false === false
>> true
 
NaN === 1
>> false
 
NaN === NaN
>> false
 
2 === 2
>> true
 
0 === -0
>> true
 
"js" === " js"
>> false
 
"js" === "js"
>> true
 
var o1 = {}
>> undefined
 
var o2 = {}
>> undefined
 
o1 === o2
>> false
 
var r1 = [1,2,3]
var r2 = [1,2,3]
>> undefined
 
r1 === r2
>> false
 
r1 === r1
>> true
 
//Operador ==
 
null == undefined
>> true
 
1 == "1"
>> true
 
1 == true
>> true
 
0 == false
>> true
 
var o = {}
>> o == "teste"
{% endhighlight %}

Por hoje é só!

Abraço! 