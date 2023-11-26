---
description: American Standard Code for Information Interchange
---

# ASCII

## ASCII 7-bits

Computadores trabalham com números, mas humanos trabalham também com texto. Sendo assim, houve a necessidade de criar um padrão de representação textual - e também de controle, que você entenderá a seguir.

O **A**merican **S**tandard **C**ode for **I**nformation **I**nterchange, ou em português, Código Padrão Americano para o Intercâmbio de Informação, é uma codificação criada nos EUA (como o nome sugere), já que o berço da computação foi basicamente lá.

Na época em que foi definido, lá pela década de 60, foi logo usado em equipamentos de telecomunicações e também nos computadores. Basicamente é uma tabela que relaciona um **número** de 7 _bits_ com sinais de controle e caracteres imprimíveis. Por exemplo, o número 97 (0b1100001) representa o caractere 'a', enquanto 98 (0b1100010) é o 'b'. Perceba que tais números não excedem os 7 _bits_, mas como em computação falamos quase sempre em _bytes_, então acaba que um caractere ASCII possui 8 _bits_ mas só usa 7. A tabela ASCII vai de 0 a 127 e pode ser encontrada no apêndice Tabela ASCII, que você deve consultar agora.

Há vários testes interessantes que você pode fazer para entender melhor as _strings_ ASCII. Ao saber que o caractere `'a'` é o número 97, você pode usar a função `chr()` no Python, por exemplo:

```python
>>> print chr(97)
a
```

Ou no próprio shell do Linux, o comando _echo_:

```bash
$ echo -ne \\x61  # 0x61 em hexa é 97 em decimal
a
```

Viu? Quando você digita 'a', o computador entende o _byte_ 0x61. De forma análoga, quando um programa que exibe um texto na tela encontra o _byte_ 0x61, ele exibe 'a'. Como você pode ver na tabela ASCII, todas as letras do alfabeto americano estão lá, então é razoável concluir que uma frase inteira seja na verdade uma sequência de _bytes_ onde cada um deles está dentro da faixa da tabela ASCII. Isso fica facilmente visualizável no shell do Linux com o comando _hd_ (hexdump):

```
$ echo menteb.in | hd
00000000  6d 65 6e 74 65 62 2e 69  6e 0a                    |menteb.in.|
0000000a
```

É exatamente assim que um texto ASCII vai parar dentro de um programa ou arquivo.

Agora olhe novamente a tabela e perceba o seguinte:

* O primeiro sinal é o NUL, também conhecido como _null_ ou nulo. É o _byte_ 0.
* Outro _byte_ importante é 0x0a, conhecido também por \n, _line feed_, LF ou simplesmente "caractere de nova linha".
* O MS-DOS e o Windows utilizam na verdade **dois** caracteres para delimitar uma nova linha. Antes do 0x0a, temos um 0x0d, conhecido também por \r, _carriage return_ ou CR. Essa dupla é também conhecida por **CrLf**.
* O caractere de "espaço em branco" é o 0x20.
* Os dígitos vão de 0x30 a 0x39.
* As letras maiúsculas vão de 0x41 a 0x5a.
* As letras minúsculas vão de 0x61 a 0x7a.

Agora, algumas relações:

* Se somarmos 0x20 ao número ASCII equivalente de um caractere maiúsculo, obtemos o número equivalente do caractere minúsculo em questão. Da mesma forma, se diminuirmos 0x20 ao valor de um caractere minúsculo, obtemos o seu maiúsculo. Perceba que basta mudar o bit 5 (da direita para a esquerda, com a contagem começando em zero) do valor para alternar entre maiúsculo e minúsculo.
* Se diminuirmos 0x30 de um dígito, temos o equivalente numérico do dígito. Por exemplo, o dígito 5 possui o valor 0x35. Então, 0x35 - 0x30 = 5.

{% hint style="info" %}
Sabe quando no Linux você dá um `cat` num arquivo que não é de texto e vários caracteres "doidos" aparecem na tela enquanto você escuta alguns beeps? Esses sons são, na verdade, os bytes 0x07 encontrados no arquivo. Experimente!
{% endhint %}

Para complementar esta seção, assista ao vídeo **Entendendo a tabela ASCII** no nosso canal no YouTube.

{% embed url="https://www.youtube.com/watch?v=IN9ElO90uLc" %}

## ASCII Estendida

A tabela ASCII padrão de 7 _bits_ é limitada ao idioma inglês no que diz respeito ao texto. Perceba que uma simples letra 'á' (com acento agudo) é impossível nesta tabela. Sendo assim, ela foi estendida e inteligentemente passou-se a utilizar o último _bit_ do _byte_ que cada caractere ocupa, tornando-se assim uma tabela de 8 _bits_, que vai de 0 a 255 (em decimal).

Essa extensão da tabela ASCII varia de acordo com a **codificação** utilizada. Isso acontece porque ela foi criada para permitir texto em outros idiomas, mas somente 128 caracteres a mais não são suficientes para representar os caracteres de todos os idiomas existentes. A codificação mais conhecida é a ISO-8859-1, também chamada de Latin-1, que você vê no Apêndice Tabela ISO-8859-1/Latin-1.

{% hint style="info" %}
Outro nome para ASCII é US-ASCII. Alguns textos referem-se a texto em ASCII como ANSI strings também.
{% endhint %}
