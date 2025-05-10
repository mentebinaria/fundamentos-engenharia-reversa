# O Byte

Agora que já temos um olhar mais abstraído sobre os números, é necessário entender como o computador trabalha com eles. Acontece que é preciso armazenar estes números em algum lugar antes de usá-los em qualquer operação. Para isso foi criado o _byte_, a unidade de medida da computação. Consiste em um **espaço** para armazenar _bits_ (8 na arquitetura Intel, também chamado de **octeto**). Então, neste livro, sempre que falarmos em em 1 _byte_, leia-se, "um espaço onde cabem 8 _bits_". Sendo assim, o primeiro número possível num _byte_ é 0b00000000, ou simplesmente 0 (já que zero à esquerda não vale nada e zero em decimal representa a mesma quantidade que zero em binário). E o maior número possível é 0b11111111 que é igual a 0xff em hexa ou 255 em decimal.

> Uma maneira rápida de calcular o maior número positivo que pode ser representado num espaço de x _bits_ é usando a fórmula 2^x - 1. Por exemplo, para os 8 _bits_ que mencionamos, basta elevar 2 à oitava potência (que resulta em 256) e diminuir uma unidade: 2^8 - 1 = 255. E por que diminuir um? Porque que o zero precisa ser representado também. Se podemos representar 256 números diferentes e o zero é um deles, ficamos com a faixa de 0 a 255.

Agora que você já sabe o que é um _byte_, podemos apresentar seus primos _nibble_ (metade dele), _word_, _double word_ e _quad word_. Veja a tabela:

| Medida        | Tamanho (Intel) | Nomenclatura Intel |
| ------------- | --------------- | ------------------ |
| _nibble_      | 4 bits          |                    |
| _byte_        | 8 bits          | BYTE               |
| _word_        | 16 bits         | WORD               |
| _double word_ | 32 bits         | DWORD              |
| _quad word_   | 64 bits         | QWORD              |

Fica claro que o maior valor que cabe, por exemplo, numa variável, depende de seu tamanho (quantidade de espaço para armazenar algum dado). Normalmente um tipo inteiro tem 32 bits, portanto, podemos calcular 2 elevado a 32 menos 1, que dá 4294967295. O inteiro de 32 _bits_ ou 4 _bytes_ é muito comum na arquitetura Intel.
