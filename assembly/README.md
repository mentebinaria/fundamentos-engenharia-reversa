# ⚙️ Assembly

Agora que você já sabe como um binário PE é construído, está na hora de entender como o código contido em suas seções de código de fato executa. Acontece que um processador é programado em sua fábrica para entender determinadas sequências de _bytes_ como código e executar alguma operação. Para entender isso, vamos fazer uma analogia com um componente muito mais simples que um processador, um **circuito integrado** \(popularmente chamado de _chip_\).

Um circuito integrado \(CI\) bastante conhecido no mundo da eletrônica é o 7400 que tem o seguinte diagrama esquemático:

![](../.gitbook/assets/7400.png)

Se você já estudou portas lógicas, vai perceber que este CI tem 4 portas NAND \(AND com saída negada\). Cada porta possui duas entradas e uma saída, cada uma delas conectada a seu respectivo pino/perna.

Admitindo duas entradas \(A e B\) e uma saída S, a tabela verdade de cada uma das portas deste CI é a seguinte:

| A    | B    | A & B | S    |
| :--- | :--- | :---  | :--- |
| 0    | 0    | 0     | 1    |
| 1    | 0    | 0     | 1    |
| 0    | 1    | 0     | 1    |
| 1    | 1    | 1     | 0    |

Podemos dizer então que este CI faz uma única operação sempre, com as entradas de dados que recebe.

Se seu projeto precisasse também de portas OR, XOR, AND, etc você precisaria comprar outros circuitos integrados, certo? Bem, uma solução inteligente seria utilizar um _chip_ que fosse **programável**. Dessa forma, você o configuraria, via _software_, para atuar em certos pinos como porta NAND, outros como porta OR e por aí vai, de acordo com sua necessidade. Aumentando ainda mais a complexidade, temos os **microcontroladores**, que podem ser **programados** em linguagens de alto nível, contando com recursos como repetições, condicionais, e tudo que uma linguagem de programação completa oferece. No entanto, estes _chips_ requerem uma reprogramação a cada mudança no projeto, assim como se faz com o Arduino hoje em dia.

Neste sentido um microprocessador, ou simplesmente **processador** é muito mais poderoso. Ao invés de o usuário gravar um programa nele, o próprio fabricante já o faz, de modo que este microprograma entenda diferentes instruções para realizar diferentes operações de muito alto nível \(se comparadas às simples operações _booleanas_\). Sua entrada de dados também é muito mais flexível: ao invés de entradas binárias, um processador pode receber números bem maiores. O antigo Intel 8088 já possuía um barramento de 8 _bits_, por exemplo.

Isso significa que se um processador receber em seu barramento um conjunto de _bytes_ específico, sabe que vai precisar executar uma operação específica. À estes _bytes_ possíveis damos o nome de **_opcodes_**. Ao conjunto dos _opcodes_ + operandos damos o nome de **instrução**.

Supondo que queiramos então fazer uma operação OR entre os valores 0x20 e 0x18 utilizando um processador x86. Na documentação deste processador, constam as seguintes informações:

* Ao receber o _opcode_ 0xb8, os próximos quatro _bytes_ serão um número salvo em memória \(similar àquela memória M+ das calculadoras\), acessível através do _byte_ identificador 0xc8.
* Ao receber o _opcode_ 0x83, seguido de um _byte_ 0xc8 \(que identifica a memória\), seguido de mais um _byte_, o número armazenado na memória identificada pelo segundo _byte_ vai sofrer uma operação OR com este terceiro _byte_.

Precisaríamos então utilizar as duas instruções, da seguinte forma:

```text
B8 20 00 00 00
83 C8 18
```

Na primeira, que tem um total de 5 _bytes_, o _opcode_ 0xb8 é utilizado para colocar o número de 32-bits \(4 _bytes_\) na sequência em memória. Como nosso número desejado possui somente 1 _byte_, preenchemos os outros três com zero, respeitando o _endianess_.

A segunda instrução tem 3 _bytes_ sendo que o primeiro é o _opcode_ dela \(OR\), o segundo é o identificador da área de memória e o terceiro é o nosso operando 0x18, que deve sofrer o OR com o valor na área de memória indicada por C8.

Temos que concordar que criar um programa assim não é nada fácil. Para resolver este problema foi criada uma **linguagem de programação**, completamente presa à arquitetura do processador \(seus _opcodes_, suas instruções\), chamada **Assembly**. Com ela, os programadores poderiam escrever o programa acima praticamente em inglês:

```asm
MOV EAX, 20
OR EAX, 18
```

De posse de um compilador de Assembly, chamado na época de **assembler**, o resultado da compilação do código-fonte acima é justamente um arquivo \(objeto\) que contém os _opcodes_ e argumentos corretos para o processador alvo, onde o programa vai rodar.

{% hint style="warning" %}
Agora você sabe o motivo pelo qual um programa compilado não é compatível entre diferentes processadores, de diferentes arquiteturas. Como estes possuem instruções diferentes e _opcodes_ diferentes, não há mesmo compatibilidade.
{% endhint %}

Perceba que Assembly é uma linguagem legível para humanos, diferente da linguagem de máquina que não passa de uma "tripa de _bytes_". Os comandos da linguagem Assembly são chamados de **mnemônicos**. No exemplo de código acima, utilizamos dois: o MOV e o OR. Estudaremos mais mnemônicos em breve.

