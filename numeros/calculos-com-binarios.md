# Cálculos com binários

Nesta seção faremos alguns cálculos com números binários, considerando cada um de seus dígitos, também chamados de **bits**. Além das clássicas como adição, subtração, multiplicação e divisão, estudaremos aqui a conjunção, disjunção, negação e disjunção exclusiva. Também incluiremos outras operações bit-a-bit que fogem da álgebra tradicional, como deslocamento e rotação de bits. Todas são importantes pois existem no contexto do Assembly, que estudaremos no futuro, e são aplicadas a números de qualquer espécie, como endereços de memória.

{% hint style="info" %}
Você pode encontrar mais sobre este assunto pesquisando por álgebra booleana e operações bit-a-bit \(_bitwise_\).
{% endhint %}

## Conjunção \(AND\)

Dados dois bits x e y, a conjunção deles resulta em 1 se ambos forem iguais a 1. Na programação o seu símbolo é normalmente um &. Sendo assim, a chamada **tabela verdade** desta operação é:

| x | y | x & y |
| :--- | :--- | :--- |
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

Então suponha que queiramos calcular a conjunção do número 0xa com 12. Sim, estamos misturando dois sistemas de numeração \(hexadecimal e decimal\) na mesma operação. E por que não? O segredo é converter para binário e fazer o cálculo para cada bit, respeitando a tabela verdade da conjunção. Mãos à obra:

```text
0xa = 1010
12  = 1100
    -------
      1000
```

O resultado é 0b1000, ou 8 em decimal. Sendo assim, as linhas abaixo farão o mesmo cálculo, mesmo utilizando sistemas de numeração \(bases\) diferentes:

```text
>>> 0b1010 & 12
8
>>> 10 & 12
8
>>> 0xa & 0b1100
8
>>> 012 & 0xc
8
>>> 0xa & 0xc
8
```

Por que utilizei tantas bases diferentes? Quero com isso por na sua cabeça que um número é só um número, independente da base na qual ele está sendo representado ou visualizado.

## Disjunção \(OR\)

O resultado da disjunção entre dois bits x e y é 1 se pelo menos um deles for 1. Sendo assim, segue a tabela verdade:

| x | y | x \| y |
| :--- | :--- | :--- |
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

Na programação, o símbolo normalmente é a barra em pé: \|. Por exemplo, vamos calcular a disjunção entre 8 e 5:

```text
8 = 1000
5 = 0101 (perceba o zero à esquerda, para facilitar)
   ------
    1101
```

O resultado é 0b1101, que é 13 em decimal.

Aí você pode questionar:

* > Opa, então a disjunção é tipo a soma?

  Te respondo:

  > Mais ou menos.

Veja que o resultado da disjunção entre 9 e 5, também é 13:

```text
9 = 1001
5 = 0101
   ------
    1101
```

Isso porque numa soma entre 1 e 1 o resultado seria 10 \(2 em decimal\), já na operação OU o resultado é 1.

## Disjunção exclusiva \(XOR\)

A disjunção exclusiva entre x e y resulta em 1 se **somente** **um** deles for 1. Sendo assim:

| x | y | x ^ y |
| :--- | :--- | :--- |
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

Assim como a disjunção é normalmente chamada de "OU", a disjunção exclusiva é chamada de "OU exclusivo", ou simplesmente XOR. Já virou até verbo e é comum ouvir pessoas falando que "XORearam" um dado.

Algumas propriedades importantes desta operação são:

1. Você pode aplicá-la em qualquer ordem. Então, _a ^ \(b ^ c\) = \(a ^ b\) ^ c_ por exemplo.
2. Um número "XOReado" com ele mesmo é sempre zero.
3. Um número "XOReado" com zero é sempre ele mesmo.

A operação XOR tem vários usos em computação. Alguns exemplos:

### Detecção de diferenças

É possível saber se um número é diferente de outro com XOR. Se os números forem diferentes, o resultado é diferente de zero. Por exemplo, tomemos um XOR entre 8 e 5 e outro entre 5 e 5:

```text
 1000    0101
 0101    0101
------  ------
 1101    0000
```

### Zerar variáveis

Fica claro que é possível zerar variáveis bastando fazer uma operação XOR do valor dela com ele mesmo, independentemente de que valor é este:

```text
>>> x=90
>>> x = x ^ x
>>> x
0
```

### Troca de valores entre duas variáveis

O algoritmo conhecido por _XOR swap_ consiste em trocar os valores de duas variáveis somente com operações XOR, sem usar uma terceira variável temporária. Basta fazer:

* Um XOR entre x e y e armazenar o resultado em **x**.
* Outro XOR entre x e y e armazenar o resultado em **y**.
* Outro XOR entre x e y e armazenar o resultado em **x**.

Veja:

```text
>>> x=8
>>> y=5
>>> x = x ^ y
>>> y = x ^ y
>>> x = x ^ y
>>> x
5
>>> y
8
```

Analisando em binário:

```text
x = 0b1000  # 8 em decimal
y = 0b0101  # 5 em decimal
x = x ^ y   # 0b1101
y = x ^ y   # resulta em 0b1000 (já é o valor original de x)
x = x ^ y   # resulta em 0b0101 (o valor original de y)
```

### Cifragem

Dado um número x, é possível calcular o resultado de uma operação XOR com um valor que chamamos de chave. Se usarmos a mesma chave num XOR com este resultado, obtemos novamente o número original:

```text
>>> x = 2017
>>> x = x ^ 0x51
>>> x
1968
>>> x = x ^ 0x51
>>> x
2017
```

Portanto, para uma cifrabem básica, se quiser esconder o valor original de um número antes de enviá-lo numa mensagem, basta _XOReá-lo_ com uma chave que só eu e o receptor da mensagem conheça \(0x51 no exemplo\). Assim eu uso tal chave para fazer a operação XOR com ele e instruo o receptor da mensagem \(por outro canal\) a usar a mesma chave e operação XOR, afim de obter o número original.

{% hint style="warning" %}
Em textos matemáticos sobre lógica, o acento circunflexo ^ representa conjunção ao invés de disjunção exclusiva. Já em softwares matemáticos, pode significar potência, por exemplo: 2^32 é dois elevado à trigésima segunda potência.
{% endhint %}

{% hint style="info" %}
Na língua Portuguesa utilizamos a palavra "OU" no sentido de "OU exclusivo". Por exemplo, quando você pede "Pizza de presunto ou pepperoni ou lombo", quer dizer que só quer um dos sabores \(exclusividade\). Se fosse uma disjunção tradicional "OU", o garçom poderia trazer presunto com pepperoni, ou mesmo todos os três ingredientes e você não poderia reclamar. :-D
{% endhint %}

## Deslocamento \(SHL e SHR\)

O deslocamento para a **esquerda** \(_shift left_\) consiste em deslocar todos os _bits_ de um número para a esquerda e completar a posição criada mais à direita com zero. Tomemos por exemplo o número 7 e uma operação SHL com 1 \(deslocar uma vez para a esquerda\):

```text
0111  # 7 em decimal
   1  # Deslocar uma vez para a esquerda (SHL)
----
1110  # 14
```

Assim podemos perceber que deslocar à esquerda dá no mesmo que multiplicar por 2. Veja:

```text
>>> x = 7
>>> x = x << 1
>>> x
14
>>> x = x << 1
>>> x
28
>>> x = x << 1
>>> x
56
```

No exemplo acima deslocamos 1 _bit_ do número 7 \(0b111\) para a esquerda três vezes, que resultou em 56. Seria o mesmo que deslocar 3 _bits_ de uma só vez:

```text
>>> 7 << 3
56
```

De forma análoga, o deslocamento para a **direita** \(_shift right_\), ou simplesmente SHR, consiste em deslocar todos os \_bits\_de um número para a direita e completar a posição criada à esquerda com zero. Tomando o mesmo 7 \(0b111\):

```text
>>> 0b111 >> 1
3
```

O resultado é uma divisão inteira \(sem considerar o resto\) por 2. Assim, 7/2 = 3 \(e sobra 1, que é desconsiderado neste cálculo\). Esta é de fato uma maneira rápida de dobrar ou calcular a metade de um número.

## Rotação \(ROL e ROR\)

Assim como no deslocamento, a rotação envolve deslocar os _bits_ de um número para a esquerda \(_rotate left_\) ou direita \(_rotate right_\) mas o _bit_ mais significativo \(mais à esquerda\) é posto no final \(mais à direita\), no lugar de zero. Por isso é necessário considerar o tamanho ... Tomemos o número 5 como exemplo:

```text
 00000101  # 5 em decimal
        1  # ROL com 1
----------
 00001010  # 10 em decimal
```

O _bit \_zero mais à esquerda "deu a volta" e veio parar ao lado direito do \_bit_ 1 mais à direita do número 0b00000101. Analisa com o número 133 agora:

```text
 10000101 # 133 em decimal
        1 # ROL com 1
----------
 00001011 # 11 em decimal
```

Desta vez o _bit_ 1, que estava mais à esquerda veio parar ao lado direito do _bit_ mais à direita, e todos os outros _bits_ foram "empurrados" para a esquerda.

Não estamos limitados a fazer operações ROL e ROR somente com 1. O byte 133 ROL 3 por exemplo resulta em 0x2c. Você é capaz de conferir?

## Negação \(NOT\)

Para negar um bit, basta invertê-lo:

| x | ~x |
| :--- | :--- |
| 0 | 1 |
| 1 | 0 |

No entanto, para inverter o número 0b100 é preciso saber seu tamanho:

| Tamanho | 0b100 | ~0b100 |
| :--- | :--- | :--- |
| 1 byte | 0b00000100 | 0b11111011 |
| 2 _bytes_ \(WORD\) | 0b0000000000000100 | 0b1111111111111011 |
| 4 \_bytes \_\(DWORD\) | 0b00000000000000000000000000000100 | 0b11111111111111111111111111111011 |

Isso é o mesmo que calcular o complemento \(ou "complemento de um"\) de um número. Para obter seu simétrico, é preciso ainda somar uma unidade, como vimos anteriormente. Por isso, um NOT _bit-a-bit_ no número 4, por exemplo, resulta em -5. Veja:

```text
>>> ~4
-5
```

Os processadores Intel x86 trabalham com muitas outras operações _bitwise_, mas que não serão discutidas neste livro. Conforme você avançar no estudo de engenharia reversa, vai se deparar com elas. ;-\)

