# Números Negativos

Já vimos que um _byte_ pode armazenar números de 0 a 255 por conta de seus 8 _bits_. Mas como fazemos quando um número é negativo? Não temos sinal de menos (-), só _bits_. E agora? Não é possível ter números negativos então? Claro que sim, do contrário você não poderia fazer contas com números negativos e o código em Python abaixo falharia:

```python
>>> -3 + 1
-2
```

Mas não falhou! Isso acontece porque na computação dividimos as possibilidades quase que "ao meio". Por exemplo, sabendo que 1 _byte_ pode representar 256 possibilidades (sendo o 0 e mais 255 de números positivos), podemos dividir tais possibilidades, de modo a representar de -128 até +127. Continuamos com 256 possibilidades diferentes (incluindo o zero), reduzimos as quantidades máxima e miníma representáveis. :\)

O _bit_ mais significativo (mais à esquerda) é utilizado para representar o sinal. Se for 0, é um número positivo. Se for 1, é um número negativo.

Há ainda a técnica chamada de **complemento de dois**, necessária para calcular um valor negativo. Para explicá-la, vamos ao exemplo de obter o valor negativo -10 a partir do valor positivo 10, considerando o espaço de 1 _byte_. Os passos são:

1. Converter 10 para binário, que resulta em 0b1010.
2. Acrescentar à esquerda do valor binário os zeros para formar o _byte_ completo (8 _bits_): 0b00001010.
3. Inverter todos os _bits_: 0b11110101 (essa operação é chamada de complementação ou complemento de um).
4. Somar 1 ao resultado final, quando finalmente chegamos ao complemento de dois: 0b11110110.

Sendo assim, vamos checar em Python:

```python
>>> 0b11110110
246
```

O que aconteceu? Bem, realmente 0b11110110 é 246 (em decimal), se interpretado como número sem sinal. Acontece que temos que dizer explicitamente que vamos interpretar um número como número com sinal (que pode ser positivo ou negativo). Em Python, um jeito é usando o pacote **ctypes**:

```python
>>> import ctypes
>>> ctypes.c_byte(0b11110110).value
-10
```

Já em C, é preciso especificar se uma variável é _signed_ ou _unsigned_. O jeito como o processador reconhece isso foge do escopo deste livro, mas por hora entenda que não há mágica: 0b11110110 (ou 0xf6) pode ser tanto 246 quanto -10. Depende de como é interpretado, se com ou sem sinal.

Por fim, é importante notar que a mesma regra se aplica para números de outros tamanhos (4 _bytes_ por exemplo). Analise a tabela abaixo, que considera números de 32 _bits_:

| Binário                          | Hexadecimal | Com sinal   | Sem sinal  |
| -------------------------------- | ----------- | ----------- | ---------- |
| 10000000000000000000000000000000 | 80000000    | -2147483648 | 2147483648 |
| 11111111111111111111111111111111 | FFFFFFFF    | -1          | 4294967295 |
| 00000000000000000000000000000000 | 00000000    | 0           | 0          |
| 01111111111111111111111111111111 | 7FFFFFFF    | 2147483647  | 2147483647 |

Perceba que o número 0x7fffffff tem seu primeiro _bit_ zerado, portanto nunca será negativo, independente de como seja interpretado. Para ser um número negativo, é necessário que o primeiro _bit_ do número esteja _setado_, ou seja, igual a 1.
