# Instruções Básicas

Uma instrução é um conjunto definido por um código de operação (_opcode_) mais seus operandos, se houver. Ao receber _bytes_ específicos em seu barramento, o processador realiza determinada operação. O formato geral de uma instrução é:

```
opcode operando1, operando2, operando3
```

Onde _opcode_ representa um código de operação definido no manual da Intel, disponível em seu website. O número de operandos, que podem variar de 0 a 3 na IA-32 (Intel Architecture de 32-bits), consistem em números literais, registradores ou endereços de memória necessários para a instrução funcionar. Por exemplo, considere a seguinte instrução, que coloca o valor 2018 no registrador EAX:

```
B8 E2 07 00 00
```

O primeiro _byte_ é o _opcode_. Os outros 4 _bytes_ representam o primeiro e único argumento dessa instrução. Sabemos então que 0xB8 faz com que um valor seja colocado em EAX. Como este registrador tem 32-bits, nada mais natural que o argumento dessa instrução ser também de 32-bits ou 4 _bytes._ Considerando o _endianess_, como já explicado anteriormente neste livro, o valor literal 2018 (0x7E2 ou, em sua forma completa de 32-bits, 0x000007E2) é escrito em _little-endian_ com seus _bytes_ na ordem inversa, resultando em E2 07 00 00.

Na arquitetura Intel IA-32, uma instrução (considerando o _opcode_ e seus argumentos) pode ter de 1 à 15 _bytes_ de tamanho.

## Copiando Valores

Uma instrução muito comum é a MOV, forma curta de "move" (do Inglês, "mover"). Apesar do nome, o que a instrução faz é copiar o segundo operando (origem) para o primeiro (destino). O operando de origem pode ser um valor literal, um registrador ou um endereço de memória. O operando de destino funciona de forma similar, com exceção de não poder ser um valor literal, pois não faria sentido mesmo. Ambos os operandos precisam ter o mesmo tamanho, que pode ser de um _byte,_ uma _word_ ou uma _doubleword_, na IA-32. Analise o exemplo a seguir:

```
MOV EBX, B0B0CA
```

A instrução acima copia um valor literal 0xB0B0CA para o registrador EBX. A versão compilada desta instrução resulta nos seguintes _bytes_:

```
BB CA B0 B0 00
```

## Aritimética

Naturalmente, processadores fazem muitos cálculos matemáticos. Veremos agora algumas dessas instruções, começando pela instrução ADD, que soma valores. Analise:

```
MOV ECX, 7
ADD ECX, 1
```

No código acima, a instrução ADD soma 1 ao valor de ECX (que no nosso caso é 7, conforme instrução anterior). O resultado desta soma é **armazenado no operando de destino**, ou seja, no próprio registrador ECX, que passa a ter o valor 8.

Uma outra forma de atingir este resultado seria utilizar a instrução INC, que incrementa seu operando em uma unidade, dessa forma:

```
MOV ECX, 7
INC ECX
```

A instrução INC recebe um único operando que pode ser um registrador ou um endereço de memória. O resultado do incremento é armazenado no próprio operando, que em nosso caso é o registrador ECX.

O leitor pode se perguntar por que existe uma instrução INC se é possível incrementar um operando em uma unidade com a instrução ADD. Para entender, compile o escreva o seguinte programa:

```asm
BITS 32

global start

section .text
start:
    mov eax, 7
    add eax, 1
    inc eax
```

Salve como `soma.s` e compile com o NASM. Terminada a compilação, verifique o código objeto gerado com o comando **`objdump`**:

```
$ objdump -dM intel soma.o

soma.o:     file format elf32-i386

Disassembly of section .text:

00000000 <start>:
   0:    b8 07 00 00 00           mov    eax,0x7
   5:    83 c0 01                 add    eax,0x1
   8:    40                       inc    eax
```

Há duas diferenças básicas entre as instruções ADD e INC neste caso. A mais óbvia é que a instrução ADD EAX, 1 custou três _bytes_ no programa, enquanto a instrução INC EAX utilizou somente um. Isso pode parecer capricho, mas não é: binários compilados possuem normalmente milhares de instruções Assembly e a diferença de tamanho no resultado final pode ser significativa.

{% hint style="info" %}
Existem sistemas onde cada _byte_ economizado num binário é valioso. Alguns exigem que os binários sejam os menores possíveis, tanto em disco (ou memória _flash_) quanto sua imagem na memória RAM. Este consumo de memória é por vezes chamado de _footprint_, principalmente em literatura sobre sistemas embarcados.
{% endhint %}

Outra vantagem da INC sobre a ADD é a velocidade de execução, já que a segunda requer que o processador leia os operandos.

A instrução SUB funciona de forma similar e para subtrair somente uma unidade, também existe uma instrução DEC (de decremento). Vamos então estudar um pouco sobre a instrução MUL agora. Esta instrução tem o primeiro operando (o de destino) **implícito**, ou seja, você não precisa fornecê-lo: será sempre EAX ou uma sub-divisão dele, dependendo do tamanho do segundo operando (de origem), que pode ser um outro registrador ou um endereço de memória. Analise:

```asm
BITS 32

global start

section .text
start:
  mov eax, 5
  mov ebx, 2
  mul ebx
```

A instrução MUL EBX vai realizar uma multiplicação sem sinal (sempre positiva) de EBX com EAX e armazenar o resultado em EAX.

{% hint style="warning" %}
Perceba que não se pode fazer diretamente MUL EAX, 2. Foi preciso colocar o valor 2 em outro registrador antes, já que a MUL não aceita um valor literal como operando.
{% endhint %}

A instrução DIV funciona de forma similar, no entanto, é recomendável que o leitor faça testes e leia sobre estas instruções no manual da Intel caso queira se aprofundar no entendimento delas.

Neste ponto acredito que o leitor esteja confortável com a aritimética em processadores x86, mas caso surjam dúvidas, não deixe de discuti-las em https://menteb.in/forum.

## Operações Bit-a-bit

Já explicamos o que são as operações bit-a-bit quando falamos sobre cálculo com binários, então vamos dedicar aqui à particularidades de seu uso. Por exemplo, a instrução XOR, que faz a operação OU EXCLUSIVO, pode ser utilizada para zerar um registrador, o que seria equivalente a mover o valor 0 para o registrador, só que muito mais rápido. Analise:

```
b9 00 00 00 00           mov    ecx,0x0
31 c9                    xor    ecx,ecx
```

Além de menor em _bytes_, a versão XOR é também mais rápida. Em ambas as instruções, depois de executadas, o resultado é que o registrador ECX terá o valor 0 e a _flag_ ZF será _setada_, como em qualquer operação que resulte em zero.

Faça você mesmo testes com as instruções AND, OR, SHL, SHR, ROL, ROR e NOT. Todas as suas operações já foram explicadas na seção Cálculos com Binários.

## Comparando Valores

Sendo uma operação indispensável ao funcionamento dos computadores, a comparação precisa ser muito bem compreendida. Instruções chave aqui são a CMP (_Compare_) e TEST. Analise o código a seguir:

```
b8 b0 b0 00 00           mov    eax,0xb0b0
3d 10 fe 00 00           cmp    eax,0xfe10
```

A instrução CMP neste caso compara o valor de EAX (previamente _setado_ para 0xB0B0) com 0xFE10. O leitor tem alguma ideia de como tal comparação é feita matematicamente? Acertou quem pensou em diminuir de EAX o valor a ser comparado. Dependendo do resultado, podemos saber o resultado da comparação da seguinte maneira:

* Se o resultado for **zero**, então os operandos de destino e origem são **iguais**.
* Se o resultado for um número **negativo**, então o operando de destino é **maior** que o de origem.
* Se o resultado for um número **positivo**, então o operando de destino é **menor** que o de origem.

{% hint style="danger" %}
O resultado da comparação é configurado no registrador EFLAGS, o que significa dizer que a instrução CMP **altera** as _flags_, para que instruções futuras tomem decisões baseadas nelas. Por exemplo, para operandos iguais, a CMP faz ZF=1.
{% endhint %}

A instrução CMP é normalmente precedida de um salto, como veremos a seguir.

## Alterando o Fluxo do Programa

A ideia de fazer uma comparação é tomar uma decisão na sequencia. Neste caso, **decisão** significa para onde transferir o fluxo de execução do programa, o que é equivalente a dizer para onde **pular**, **saltar**, ou para onde **apontar o EIP** (o ponteiro de instrução). Uma maneira de fazer isso é com as instruções de saltos (_jumps_).

### Salto Incondicional

Existem vários tipos de saltos. O mais simples é o salto **incondicional** produzido pela instrução JMP, que possui apenas um operando, podendo ser um valor literal, um registrador ou um endereço de memória. Para entender, analise o programa abaixo:

```
   0:    b8 01 00 00 00           mov    eax,0x1
   5:    eb 03                    jmp    0xa
   7:    83 c0 04                 add    eax,0x4
   a:    40                       inc    eax
```

A instrução ADD EAX, 4 nunca será executada pois o salto faz a execução pular para o endereço 0x0A, onde temos a instrução INC EAX. Portanto, o valor final de EAX será 2.

{% hint style="info" %}
Note aqui o _opcode_ do salto incondicional JMP, que é o 0xEB. Seu argumento, é o número de _bytes_ que serão pulados, que no nosso caso, são 3. Isso faz a execução pular a instrução ADD EAX, 4 inteira, já que ela tem exatamente 3 _bytes_.
{% endhint %}

{% hint style="info" %}
Você pode entender o salto incondicional JMP como um comando **goto** na linguagem de programação C.
{% endhint %}

### Saltos Condicionais Sem Sinal

Os saltos condicionais J\_cc\_ onde _cc_ significa _condition code_, podem ser de vários tipos. O mais famoso deles é o **JE (**_**Jump if Equal**_**)**, utilizado para saltar quando os valores da comparação anterior são iguais. Em geral ele vem precedido de uma instrução CMP, como no exemplo abaixo:

```
   0:    b8 01 00 00 00           mov    eax,0x1
   5:    83 f8 01                 cmp    eax,0x1
   8:    74 03                    je     0xd
   a:    83 c0 03                 add    eax,0x3
   d:    40                       inc    eax
```

A instrução no endereço 0x5 compara o valor de EAX com 1 e vai sempre resultar em verdadeiro neste caso, o que significa que a _zero flag_ será _setada_.

O salto JE ocorre se ZF=1, ou seja, se a _zero flag_ estiver _setada_. Por essa razão, ele também é chamado de **JZ (_Jump if Zero_)**. Abaixo uma tabela com os saltos que são utilizados para comparações entre números sem sinal e as condições para que o salto ocorra:

| Instrução        | Alternativa                             | Condição          |
| ---------------- | --------------------------------------- | ----------------- |
| JZ (Zero)        | JE (Equal)                              | ZF=1              |
| JNZ (Not Zero)   | JNE (Not Equal)                         | ZF=0              |
| JC (Carry)       | JB (Below) ou JNAE (Not Above or Equal) | CF=1              |
| JNC (Not Carry)  | JNB (Not Below) ou JAE (Above or Equal) | CF=0              |
| JA (Above)       | JNBE (Not Below or Equal)               | CF=0 e ZF=0       |
| JNA (Not Above)  | JBE (Below or Equal)                    | CF=1 ou ZF=1      |
| JP (Parity)      | JPE (Parity Even)                       | PF=1              |
| JNP (Not Parity) | JPO (Parity Odd)                        | PF=0              |
| JCXZ (CX Zero)   |                                         | Registrador CX=0  |
| JECXZ (ECX Zero) |                                         | Registrador ECX=0 |

Nem é preciso dizer que vai ser necessário você criar programas em Assembly para treinar a compreensão de cada um dos saltos, é?

### Saltos Condicionais Com Sinal

Já vimos que comparações são na verdade subtrações, por isso os resultados são diferentes quando utilizados números com e sem sinal. Apesar de a instrução ser a mesma (CMP), os saltos podem mudar. Eis os saltos para comparações com sinal:

| Instrução              | Alternativa                 | Condição |
| ---------------------- | --------------------------- | -------- |
| JG (Greater)           | JNLE (Not Less or Equal)    |          |
| JGE (Greater or Equal) | JNL (Not Less)              |          |
| JL (Less)              | JNGE (Not Greater or Equal) |          |
| JLE (Less or Equal)    | JNG (Not Greater)           |          |
| JS (Sign)              |                             | SF=1     |
| JNS (Not Sign)         |                             | SF=0     |
| JO (Overflow)          |                             | OF=1     |
| JNO (Not Overflow)     |                             | OF=0     |

Não se preocupe com a quantidade de diferentes instruções na arquitetura. O segredo é estudá-las conforme o necessário, na medida em que surgem nos programas que você analisa. Para avançar, só é preciso que você entenda o conceito do salto. Muitos problemas de engenharia reversa são resolvidos com o entendimento de um simples JE (ZF=1). Se você já entendeu isso, é suficiente para prosseguir. Se não, releia até entender. É normal não compreender tudo de uma vez e vários dos assuntos necessitam de revisão e exercícios para serem completamente entendidos.
