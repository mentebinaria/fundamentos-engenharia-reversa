# Sistemas de numeração

Conhecemos bem os dez símbolos latinos 0, 1, 2, 3, 4, 5, 6, 7, 8 e 9 utilizados no sistema de numeração decimal. Neste sistema, o símbolo 0 \(zero\) é utilizado para descrever uma quantidade nula, enquanto o símbolo 1 \(um\) descreve uma quantidade, o 2 \(dois\) duas quantidades e assim sucessivamente, até que atinjamos a quantidade máxima com apenas um dígito, que é 9 \(nove\). Para representar uma quantidade a mais que essa, a regra é: pegamos o símbolo que representa uma quantidade e colocamos à sua direita o que representa uma quantidade nula formando, assim, 10 \(dez\). O mesmo processo ocorre com este zero à direita, até que os dígitos "acabem" novamente e aí incrementamos o 1 da esquerda em uma unidade, até que chegamos ao 20. Estudos futuros definiram este conjunto como **números naturais** e adicionaram outros: números inteiros \(que contemplam os negativos\), fracionários, complexos, etc.

Mas este não é o único - nem é o primeiro - sistema para representação de quantidades. Ou seja, não é o único sistema de numeração possível. Os computadores, diferente dos humanos, são máquinas elétricas. Sendo assim, a maneira mais fácil de números fluírem por eles seria com um sistema que pudesse ser **interpretado** a partir de dois estados: ligado e desligado.

### Binário

O sistema binário surgiu há muito tempo e não vou arriscar precisar quando ou onde, mas em 1703 o alemão Leibniz publicou um [trabalho refinado](http://www.leibniz-translations.com/binary.htm) baseado na dualidade taoísta chinesa do _yin_ e _yan_ a qual descrevia o sistema binário moderno com dois símbolos: 0 \(nulo\) e 1 \(uma unidade\). Por ter somente dois símbolos, ficou conhecido como sistema binário, ou de base 2. A contagem segue a regra: depois de 0 e 1, pega-se o símbolo que representa uma unidade e se insere, à sua direita, o que representa nulo, formando o número que representa duas unidades neste sistema: 10.

{% hint style="info" %}
Daí vem a piada nerd que diz haver apenas 10 tipos de pessoas no mundo: as que entendem linguagem binária e as que não entendem.
{% endhint %}

Assim sendo, se formos contar até dez unidades, teremos: 0, 1, 10, 11, 100, 101, 110, 111, 1000, 1001 e 1010.

Perceba que a lógica de organização dos símbolos no sistema binário é a mesma do sistema de numeração decimal. No entanto, em binário, como o próprio nome sugere, só temos dois símbolos disponíveis para representar todas as quantidades.

Por utilizar dois símbolos que são idênticos aos do sistema decimal, num contexto genérico, números binários são normalmente precedidos com 0b para não haver confusão. Então para expressar dez quantidades faríamos 0b1010. Por exemplo, a seguinte linha na console do Python imprime o valor 10:

{% code-tabs %}
{% code-tabs-item title="Python" %}
```python
print(0b1010)
```
{% endcode-tabs-item %}

{% code-tabs-item title="Bash" %}
```bash
echo $((0b1010))
```
{% endcode-tabs-item %}

{% code-tabs-item title="C" %}
```c
printf("%d\n", 0b1010);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Octal

Como o próprio nome sugere, o sistema octal possui oito símbolos: 0, 1, 2, 3, 4, 5, 6 e 7. À esta altura já dá pra sacar que para representar oito quantidades em octal o número é 10. Nove é 11, dez é 12 e assim sucessivamente.

{% hint style="info" %}
O sistema octal é utilizado para as permissões de arquivo pelo comando chmod nos sistemas baseados em Linux e BSD. Os números 1, 2 e 4 representam permissão de execução, escrita e leitura, respectivamente. Para combiná-las, basta somar seus números correspondentes. Sendo assim, uma permissão 7 significa que se pode-se tudo \(rwx\) enquanto uma permissão 6 somente escrita e leitura \(rw-\). Tais números foram escolhidos para não haver confusão. Se fossem os números 1, 2 e 3 a permissão 3 poderia significar tanto ela mesma quanto 1+2 \(execução + escrita\). Usando 1, 2 e 4 não há brechas para dúvida. ;\)
{% endhint %}

{% hint style="danger" %}
Na programação normalmente um número octal é precedido de um algarismo 0 para diferenciar-se do decimal. Por exemplo, 12 seria doze \(decimal\), enquanto 012 é dez \(octal\). Portanto, cuidado ao ignorar o zero à esquerda!
{% endhint %}

Veja o exemplo:

{% code-tabs %}
{% code-tabs-item title="Python" %}
```python
>>> 012
10
```
{% endcode-tabs-item %}

{% code-tabs-item title="Bash" %}
```bash
echo "$((8#012))"
10
# Usando o bc:
echo "obase=10; ibase=8; 012" | bc
10
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Hexadecimal

Finalmente o queridinho hexa \(para os íntimos\); o sistema de numeração que mais vamos utilizar durante todo o livro. O hexadecimal apresenta várias vantagens sobre seus colegas, a começar pelo número de símbolos: 16. São eles: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E e F. Os números que eles formam são normalmente prefixados com 0x.

Perceba que os sistemas utilizam os mesmos símbolos latinos. Isso é só para facilitar mesmo. :\)

Aqui cabe uma tabela comparativa, só para exercitar:

| Hexadecimal | Decimal | Octal | Binário |
| :--- | :--- | :--- | :--- |
| **0** | **0** | **0** | **0** |
| **1** | **1** | **1** | **1** |
| **2** | **2** | **2** | 10 |
| **3** | **3** | **3** | 11 |
| **4** | **4** | **4** | 100 |
| **5** | **5** | **5** | 101 |
| **6** | **6** | **6** | 110 |
| **7** | **7** | **7** | 111 |
| **8** | **8** | 10 | 1000 |
| **9** | **9** | 11 | 1001 |
| **A** | 10 | 12 | 1010 |
| **B** | 11 | 13 | 1011 |
| **C** | 12 | 14 | 1100 |
| **D** | 13 | 15 | 1101 |
| **E** | 14 | 16 | 1110 |
| **F** | 15 | 17 | 1111 |

Existem algumas propriedades interessantes que cabe ressaltar quando relacionamos os diferentes sistemas de numeração vistos aqui. São elas:

* Quanto mais símbolos \(dígitos\) existem no sistema, menos dígitos utilizamos para representar mais quantidades.
* 0xF = 0b1111 assim como 0xFF = 0b**1111**1111 e 0xFE = 0b**1111**1110.
* 0x10 = 16. Então, 0x20 = 32 e 0x40 = 64.
* Em hexadecimal, 9 + 1 = A, então 19 + 1 = 1A.
* Na arquitetura x86 os endereços são de 32-bits. Ao analisar a pilha de memória, por exemplo, é bom se acostumar com decrementos de 4 bytes, tipo: 12FF**90**, 12FF**8C**, 12FF**88**, 12FF**84**, 12FF**80**, 12FF**7C**...
* Cada dígito hexa pode ser compreendido como 4 dígitos binários, então 0x**B**0**B**0**C**A = 0b**1011**0000**1011**0000**1100**1010. Fica fácil se aplicarmos dessa maneira, veja:

```text
 B    0    B    0    C    A
1011 0000 1011 0000 1100 1010
```

Teste no terminal:

{% code-tabs %}
{% code-tabs-item title="Python" %}
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
{% endcode-tabs-item %}

{% code-tabs-item title="Bash" %}
```bash
echo "$((16#a))"
10
echo "$((16#0A))"
10
echo "$((16#000000000000000000000a))"
10
echo "$((16#A))"
10
```
{% endcode-tabs-item %}
{% endcode-tabs %}


{% hint style="info" %}
Em Python, C e outras linguagens, não importa se escrevemos números hexadecimais com letras maiúsculas ou minúsculas \(mais comum\), desde que os prefixemos com 0x. Os zeros à esquerda \(imediatamente após o 0x\) também não importam.
{% endhint %}

Falaremos bastante em endereços de memória no conteúdo de engenharia reversa e todos estão em hexadecimal, por isso é importante "pensar em hexa" daqui pra frente. Mas não se preocupe, se precisar calcular algo, sempre poderá recorrer à calculadora ou ao Python mesmo. Uma alternativa no shell do Linux é o comando bc, que o vídeo a seguir explica bem:

{% embed url="https://www.youtube.com/watch?v=vLhABLeb11o" caption="Cálculo no shell com o bc" %}

### Crie seu próprio sistema de numeração

Um bom exercício é criar o seu sistema de numeração, com símbolos diferentes dos habituais. Pode ser qualquer coisa. Digamos que farei um sistema ternário chamado Lulip's que possui os seguintes símbolos para representar zero, uma e duas quantidades respectivamente: @, \# e $. Olha só como ficaria a comparação com decimal:

| Decimal | Lulip's |
| :--- | :--- |
| 0 | @ |
| 1 | \# |
| 2 | $ |
| 3 | \#@ |
| 4 | \#\# |
| 5 | \#$ |
| 6 | $@ |
| 7 | $\# |
| 8 | $$ |
| 9 | \#@@ |
| 10 | \#@\# |
| 11 | \#@$ |

É importante que o leitor perceba a lógica utilizada para contar no sistema Lulip's. Ele não existe, mas criar o próprio sistema é um bom exercício para compreender como qualquer um pode ser convertido entre si.

