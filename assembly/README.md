# ⚙️ Assembly

Agora que você já sabe como um binário PE é construído, está na hora de entender como o código contido em suas seções de código de fato é executado. Acontece que um processador é programado em sua fábrica para entender determinadas sequências de _bytes_ como código e executar alguma operação. Para entender isso, vamos fazer uma analogia com um componente muito mais simples que um processador, um **circuito integrado** (genericamente chamado de _chip_).

Um circuito integrado (CI) bastante conhecido no mundo da eletrônica é o 7400. Seu funcionamento interno é detalhado no seguinte diagrama a seguir.

![](../.gitbook/assets/7400.png)

Se você já estudou portas lógicas, vai perceber que este CI tem 4 portas NAND (AND com saída negada). Cada porta possui duas entradas e uma saída, cada uma delas conectada a seu respectivo pino/perna do CI.

Admitindo duas entradas (A e B) e uma saída S, a tabela verdade de cada uma das portas deste CI é a seguinte:

| A   | B   | A & B | S   |
| --- | --- | ----- | --- |
| 0   | 0   | 0     | 1   |
| 1   | 0   | 0     | 1   |
| 0   | 1   | 0     | 1   |
| 1   | 1   | 1     | 0   |

Podemos dizer então que este CI faz uma única operação sempre, com as entradas de dados que recebe.

Se seu projeto precisasse também de portas OR, XOR, AND, etc você precisaria comprar outros circuitos integrados, certo? Alternativamente, uma solução seria utilizar um _chip_ que fosse **programável**. Dessa forma, você o configuraria, via _software_, para atuar em certos pinos como porta NAND, em outros como porta OR e por aí vai, de acordo com sua necessidade. Para casos assim, existem os **microcontroladores**. Eles podem podem ser **programados** em linguagens de alto nível que contam com recursos como repetições, condicionais, e tudo que uma linguagem de programação completa oferece. No entanto, estes _chips_ requerem uma reprogramação a cada mudança no projeto, assim como se faz com o Arduino hoje em dia.

Neste sentido um microprocessador, ou simplesmente **processador** é muito mais poderoso. Ao invés nós gravarmos um programa nele como fazemos com os microcontroladores, o próprio fabricante do processador é que grava um programa lá, de modo que este microprograma entenda diferentes instruções para realizar diferentes operações de muito mais complexidade se comparadas às simples operações _booleanas_. Sua entrada de dados também é muito mais larga, de modo que mais _bits_ podem ser enviados por vez.

Isso significa que se um processador receber em seu barramento um conjunto de _bits_ específico, sabe que vai precisar executar uma operação específica. Considere agora _bytes_ como conjuntos de 8 _bits_, como já aprendemos. À estes _bytes_ possíveis damos o nome de _**opcodes**_. Ao conjunto dos _opcodes_ + operandos damos o nome de **instrução**.

Para entender melhor, suponha que queiramos então fazer uma operação OR entre os valores 0x20 e 0x18 utilizando um processador de arquitetura x86-64. Na documentação deste processador, suponha que constem as seguintes informações:

* Ao receber o _opcode_ 0xb8, os próximos quatro _bytes_ serão um número salvos numa memória interna (similar àquela memória M+ das calculadoras).

* Para acessar essa memória interna específica, utiliza-se o _byte_ identificador 0xc8.

* Ao receber o _opcode_ 0x83, o próximo _byte_ identifica a memória interna a ser acessada e o _byte_ seguinte é um operando que efetua uma operação OR dele com o número de quatro _bytes_ armazenado na memória interna.

Precisaríamos então enviar para este processador as duas instruções, da seguinte forma:

```
B8 20 00 00 00
83 C8 18
```

Na primeira, que tem um total de 5 _bytes_, o _opcode_ 0xb8 é utilizado para colocar o número de 32-bits (próximos 4 _bytes_) na memória interna. Como nosso número desejado possui somente 1 _byte_, preenchemos os outros três com zero, respeitando o _endianness_.

A segunda instrução tem 3 _bytes_ sendo que o primeiro é o _opcode_ dela (OR), o segundo é o identificador da memória interna e o terceiro é o nosso operando 0x18.

Temos que concordar que criar um programa assim não seria nada fácil. Para resolver este problema foi criada uma **linguagem de programação**, completamente atrelada à arquitetura do processador, aos seus _opcodes_ e às suas instruções, chamada **Assembly**. Ela dá nomes, por exemplo, para a tal memória interna, que são chamadas de **registradores**. Além disso, ao invés de usar os _opcodes_ numéricos, você pode usar **mnemônicos** (palavras) em inglês para sinalizar a instrução desejada. Veja se não é mais fácil de entender assim:

```asm
MOV EAX, 20 ; Coloca o valor 0x20 no registrador EAX
OR EAX, 18  ; Faz um OR do valor em EAX com 0x18 e salva o resultado em EAX
```

De posse de um compilador de Assembly, muitas vezes chamado de  **assembler** (ou **montador** em Português), o resultado da compilação do código-fonte acima é justamente um arquivo objeto que contém os _opcodes_ e argumentos corretos para o processador alvo onde o programa vai rodar.

{% hint style="warning" %}
Agora você sabe o motivo pelo qual um programa compilado não é compatível entre diferentes processadores de diferentes arquiteturas. Como estes possuem instruções diferentes e _opcodes_ diferentes, não há mesmo essa compatibilidade.
{% endhint %}

Perceba que Assembly é uma linguagem legível para seres humanos, diferente da linguagem de máquina que não passa de uma "tripa de _bytes_" onde você tem que sar seu jeito para fazer sentido.

Como afirmados antes, as palavras em linguagem Assembly que você usa para sinalizar instruções são chamadas de **mnemônicos**. No exemplo de código acima, utilizamos dois: o MOV e o OR. Estudaremos mais mnemônicos em breve.

[image-1]:	../.gitbook/assets/7400.png