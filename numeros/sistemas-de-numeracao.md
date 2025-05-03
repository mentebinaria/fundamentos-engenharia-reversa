# Sistemas de Numeração

Conhecemos bem os dez símbolos latinos utilizados no sistema de numeração decimal: 0, 1, 2, 3, 4, 5, 6, 7, 8 e 9. Neste sistema, o símbolo 0 (zero) é utilizado para descrever uma quantidade nula, enquanto o símbolo 1 (um) descreve uma quantidade, o 2 (dois) duas quantidades e assim sucessivamente, até que atinjamos a quantidade máxima representável com apenas um dígito, que é 9 (nove). Para representar uma quantidade a mais que essa, a regra é: pegamos o símbolo que representa uma quantidade e colocamos à sua direita o que representa uma quantidade nula formando, assim, o 10 (dez). O mesmo processo ocorre com este zero à direita, até que os dígitos "acabem" novamente e aí incrementamos o 1 da esquerda em uma unidade, até que chegamos ao 20. Estudos futuros definiram este conjunto como **números naturais** e adicionaram outros: números inteiros (que contemplam os negativos), fracionários, complexos, etc.

Mas este não é o único - nem é o primeiro - sistema para representação de quantidades. Ou seja, não é o único sistema de numeração possível. Os computadores, diferente dos humanos, são máquinas elétricas. Sendo assim, a maneira mais fácil de números fluírem por eles seria com um sistema que pudesse ser **interpretado** a partir de dois estados: ligado e desligado.

## Binário

O sistema binário surgiu há muito tempo e não vou arriscar precisar quando ou onde, mas em 1703 o alemão Leibniz publicou um trabalho refinado, com tradução para o inglês disponível em http://www.leibniz-translations.com/binary.htm, baseado na dualidade taoísta chinesa do _yin_ e _yan_ a qual descrevia o sistema binário moderno com dois símbolos: 0 (nulo) e 1 (uma unidade). Por ter somente dois símbolos, ficou conhecido como sistema binário, ou de base 2. A contagem segue a regra: depois de 0 e 1, pega-se o símbolo que representa uma unidade e se insere, à sua direita, o que representa nulo, formando o número que representa duas unidades neste sistema: 10.

{% hint style="info" %}
Daí vem a piada nerd que diz haver apenas 10 tipos de pessoas no mundo: as que entendem linguagem binária e as que não entendem.
{% endhint %}

Assim sendo, se formos contar até dez unidades, teremos: 0, 1, 10, 11, 100, 101, 110, 111, 1000, 1001 e 1010.

Perceba que a lógica de organização dos símbolos no sistema binário é a mesma do sistema de numeração decimal. No entanto, em binário, como o próprio nome sugere, só temos dois símbolos disponíveis para representar todas as quantidades.

Por utilizar dois símbolos que são idênticos aos do sistema decimal, num contexto genérico, números binários são normalmente precedidos com 0b para não haver confusão. Então para expressar dez quantidades faríamos 0b1010. Por exemplo, a seguinte linha na console do Python imprime o valor 10 na tela:

```python
>>> 0b1010
```

Ou em linguagem C:

```c
printf("%d\n", 0b1010);
```

Na prática, os códigos acima funcionam como conversores de binário para decimal.

## Octal

Como o próprio nome sugere, o sistema octal possui oito símbolos: 0, 1, 2, 3, 4, 5, 6 e 7. À esta altura já dá pra sacar que para representar oito quantidades em octal o número é 10. Nove é 11, dez é 12 e assim sucessivamente.

{% hint style="info" %}
O sistema octal é utilizado para as permissões de arquivo pelo comando chmod nos sistemas baseados em Linux e BSD. Os números 1, 2 e 4 representam permissão de execução, escrita e leitura, respectivamente. Para combiná-las, basta somar seus números correspondentes. Sendo assim, uma permissão 7 significa que se pode-se tudo \(leitura, escrita e execução\) enquanto uma permissão 6 permite somente escrita e leitura. Tais números foram escolhidos para não haver confusão. Se fossem os números 1, 2 e 3 a permissão 3 poderia significar tanto ela mesma quanto 1+2 (execução + escrita). Usando 1, 2 e 4 não há brechas para dúvidas. ;\)
{% endhint %}

Veja um exemplo em Python (lembre-se: abra o Python e estude junto agora):

```python
>>> 0o12
10
```

## Hexadecimal

Finalmente o queridinho hexa; o sistema de numeração que mais vamos utilizar durante todo o livro.

O hexadecimal apresenta várias vantagens sobre seus colegas, a começar pelo número de símbolos: 16. São eles: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E e F. Os números que eles formam são normalmente prefixados com 0x, embora alguns programas utilizem um sufixo **h**. Por exemplo: 0x1234 ou 1234h.

Perceba que todos os sistemas apresentados até agora utilizam os mesmos símbolos latinos. Isso é só para facilitar mesmo.

Aqui cabe uma tabela comparativa, só para exercitar:

| Hexadecimal | Decimal | Octal | Binário |
| ----------- | ------- | ----- | ------- |
| **0**       | **0**   | **0** | **0**   |
| **1**       | **1**   | **1** | **1**   |
| **2**       | **2**   | **2** | 10      |
| **3**       | **3**   | **3** | 11      |
| **4**       | **4**   | **4** | 100     |
| **5**       | **5**   | **5** | 101     |
| **6**       | **6**   | **6** | 110     |
| **7**       | **7**   | **7** | 111     |
| **8**       | **8**   | 10    | 1000    |
| **9**       | **9**   | 11    | 1001    |
| **A**       | 10      | 12    | 1010    |
| **B**       | 11      | 13    | 1011    |
| **C**       | 12      | 14    | 1100    |
| **D**       | 13      | 15    | 1101    |
| **E**       | 14      | 16    | 1110    |
| **F**       | 15      | 17    | 1111    |

## Curiosidades

Existem algumas propriedades interessantes quando relacionamos os diferentes sistemas de numeração vistos aqui. São elas:

* Quanto mais símbolos existem no sistema, menos dígitos utilizamos para representar grandes quantidades.
* 0xF é igual a 0b1111, assim como 0xFF equivale a 0b**1111**1111 e 0xFE é o mesmo que 0b**1111**1110.
* 0x10 é 16. Então, 0x20 é 32 e 0x40 é 64.
* Em hexadecimal, 9 + 1 é A, então 19 + 1 é 1A.
* Na arquitetura x86, os endereços são de 32-bits. Ao analisar a pilha de memória, é bom se acostumar com decrementos de 4 bytes. Por exemplo, 12FF**90**, 12FF**8C**, 12FF**88**, 12FF**84**, 12FF**80**, 12FF**7C** e assim sucessivamente.
* Na conversão de hexadecimal para binário, cada dígito hexa pode ser compreendido como quatro dígitos binários. Para exemplificar, tomemos o número 0x**B**0**B**0**C**A. Separando cada dígito hexa e convertendo-o para binário, temos:

```
 B    0    B    0    C    A
1011 0000 1011 0000 1100 1010
```

Por isso podemos dizer que 0x**B**0**B**0**C**A é 0b**1011**0000**1011**0000**1100**1010.

* Em hexadecimal, zeros à esquerda depois do prefixo e letras maiúsculas ou minúsculas não importam. Veja no Python:

```python
>>> 0xa
10
>>> 0x0A
10
>>> 0x000000000000000000000a
10
>>> 0xA
10
```

Falaremos bastante em endereços de memória no conteúdo de engenharia reversa e todos estão em hexadecimal, por isso é importante "pensar em hexa" daqui para frente. Mas não se preocupe, se precisar calcular algo, sempre poderá recorrer à calculadora ou ao Python.

## Criando seu próprio Sistema de Numeração

Um bom exercício para fixar este conteúdo é criar o seu sistema de numeração com símbolos à sua escolha. Por exemplo, inventarei agora um sistema ternário chamado Lulip's que possui os seguintes símbolos para representar zero, uma e duas quantidades respectivamente: @, # e $. Olha só como ficaria a comparação dele com o sistema decimal:

| Decimal | Lulip's |
| ------- | ------- |
| 0       | @       |
| 1       | #       |
| 2       | $       |
| 3       | #@      |
| 4       | ##      |
| 5       | #$      |
| 6       | $@      |
| 7       | $#      |
| 8       | \$$     |
| 9       | #@@     |
| 10      | #@#     |
| 11      | #@$     |

É importante que você perceba a lógica utilizada para contar no sistema Lulip's. Apesar de não ser um sistema que exista por aí, ele serve de base para você entender como qualquer valor, em qualquer sistema, pode ser convertido para outro.
