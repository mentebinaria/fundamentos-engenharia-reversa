# Instruções Básicas

Uma instrução é um conjunto definido por um código de operação (_opcode_) mais seus operandos, se houver. Ao receber _bytes_ específicos em seu barramento, o processador realiza determinada operação. O formato geral de uma instrução é:

	opcode operando1, operando2, operando3

Onde _opcode_ representa um código de operação definido no manual da Intel, disponível em seu website. Os operandos de uma instrução consistem em números literais, registradores ou endereços de memória necessários para que ela seja codificada corretamente. Por exemplo, considere a seguinte instrução, que coloca o valor 2025 no registrador EAX:

	B8 E9 07 00 00

O primeiro _byte_ é o _opcode_. Os outros 4 _bytes_ representam o primeiro e único argumento dessa instrução. Sabemos então que 0xB8 faz com que um valor seja colocado em EAX. Como este registrador tem 32-bits, nada mais natural que o argumento dessa instrução ser também de 32-bits ou 4 _bytes._ Considerando o _endianess_, como já explicado anteriormente neste livro, o valor literal 2025 (0x7E9 ou, em sua forma completa de 32-bits, 0x000007E9) é escrito em _little-endian_ com seus _bytes_ na ordem inversa, resultando nos bytes E9 07 00 00.

## Copiando Valores

Uma instrução muito comum é a MOV, forma curta de "move" (do inglês, "mover"). Apesar do nome, o que a instrução faz é copiar o segundo operando (origem) para o primeiro (destino). O operando de origem pode ser um valor literal, um registrador ou um endereço de memória. O operando de destino funciona de forma similar, mas não pode ser um valor literal, pois não faria sentido. Ambos os operandos precisam ter o mesmo tamanho, que pode ser de um _byte,_ uma _word_, uma _doubleword_ ou uma _quadword_. Analise o exemplo a seguir:

	mov rbx, 1122334455667788

A instrução acima copia um valor literal 0x 1122334455667788 para o registrador RBX. A versão compilada desta instrução resulta nos seguintes _bytes_:

	48 BB 88 77 66 55 44 33 22 11

## Aritmética

Naturalmente, processadores fazem muitos cálculos matemáticos. Veremos agora algumas dessas instruções, começando pela instrução ADD, que soma valores. Analise:

	mov rcx, 7
	add rcx, 1

No código acima, a instrução ADD soma 1 ao valor de RCX (que no nosso caso é 7, conforme instrução anterior). O resultado desta soma é **armazenado no operando de destino**, ou seja, no próprio registrador RCX, que passa a ter o valor 8.

Uma outra forma de atingir este resultado seria utilizar a instrução INC, que incrementa seu operando em uma unidade, dessa forma:

	mov rcx, 7
	inc rcx

A instrução INC recebe um único operando que pode ser um registrador ou um endereço de memória. O resultado do incremento é armazenado no próprio operando, que em nosso caso é o registrador RCX.

Claro que você atingiria o mesmo objetivo se utilizasse `ADD RCX, 1` ao invés de `INC RCX`. No entanto, a INC é uma instrução menor em quantidade de _bytes_, simples de ler e mais rápida para executar e isso pode fazer diferença num programa grande, que executa dezenas de milhares de instruções.

> Existem sistemas onde cada _byte_ economizado num binário é valioso. Alguns exigem que os binários sejam os menores possíveis, tanto em disco (ou memória _flash_) quanto sua imagem na memória RAM. Este consumo de memória é por vezes chamado de _footprint_, principalmente em literatura sobre sistemas embarcados.

Outra vantagem da INC sobre a ADD é a velocidade de execução, já que a segunda requer que o processador leia os operandos.

A instrução SUB funciona de forma similar e, para subtrair somente uma unidade, também existe uma instrução DEC (de decremento). Vamos então estudar um pouco sobre a instrução MUL agora. Esta instrução tem o primeiro operando (o de destino) **implícito**, ou seja, você não precisa fornecê-lo: será sempre RAX ou um subregistrador dele, dependendo do tamanho do segundo operando (de origem), que pode ser um outro registrador ou um endereço de memória. Analise:

	mov rax, 5
	mov rbx, 2
	mul rbx

A instrução MUL RBX vai realizar uma multiplicação sem sinal (sempre positiva) de RBX com RAX e armazenar o resultado em RAX.

Não se pode fazer diretamente MUL RAX, 2. É preciso colocar o valor 2 em outro registrador antes, já que a MUL não aceita um valor literal como operando.

> Por padrão, compiladores tentam otimizar seu código sempre que podem. Por exemplo, a instrução `MOV RBX, 2` tem o mesmo efeito de `MOV EBX, 2` e o compilador pode escolher utilizar esta última porque ela é menor (em número de _bytes_ ocupados no programa). O efeito é o mesmo porque ao copiar um valor de 32-bits para um registrador de 64-bits, os 32 bits mais altos deste registrador são zerados.

A instrução DIV funciona de forma similar, mas estudar aritmética a fundo foge do nosso objetivo. Consulte um bom livro de Assembly se assim desejar.

## Operações Bit-a-bit

Já explicamos o que são as operações bit-a-bit quando falamos sobre cálculo com binários. Vamos cobrir agora as particularidades de seu uso. Por exemplo, a instrução XOR, que faz a operação OU EXCLUSIVO, pode ser utilizada para zerar um registrador, o que seria equivalente a mover o valor 0 para o registrador, só que mais rápido. Analise:

	mov rcx, 0
	xor rcx, rcx

Além de menor em _bytes_, a versão com XOR é também mais rápida. Em ambas as instruções, depois de executadas, o resultado é que o registrador RCX terá o valor 0 e a _flag_ ZF será _ligada_, como em qualquer operação que resulte em zero.

Há também as instruções AND, OR, SHL, SHR, ROL, ROR e NOT. Todas essas operações foram cobertas no capítulo Números.

## Comparando Valores

Sendo uma operação indispensável ao funcionamento dos computadores, a comparação precisa ser muito bem compreendida. Instruções chave aqui são a CMP (_Compare_) e TEST. Analise o código a seguir:

	mov    eax, 0xb0b0
	cmp    eax, 0xfe10

A instrução CMP neste caso compara o valor de EAX (que é 0xB0B0 após a instrução MOV) com 0xFE10. Como será que tal comparação é feita matematicamente? Acertou se você pensou em diminuir de EAX o valor a ser comparado. Dependendo do resultado, podemos saber o resultado da comparação da seguinte maneira:

* Se o resultado for **zero**, então os operandos de destino e origem são **iguais**.
* Se o resultado for um número **negativo**, então o operando de destino é **maior** que o de origem.
* Se o resultado for um número **positivo**, então o operando de destino é **menor** que o de origem.

O resultado da comparação é armazenado no registrador RFLAGS, o que significa dizer que a instrução CMP **altera** este registrador para que instruções futuras tomem decisões baseadas nelas. Por exemplo, para operandos iguais, como o resultado é zero, a CMP liga a _zero flag_ no registrador RFLAGS.

A instrução CMP é normalmente precedida de um salto, como veremos a seguir.

## Alterando o Fluxo do Programa

A ideia de fazer uma comparação é tomar uma decisão na sequência. Neste caso, **decisão** significa para onde transferir o fluxo de execução do programa, o que é equivalente a dizer para onde **pular**, **saltar**, ou para onde **apontar o RIP** (o ponteiro de instrução). Uma maneira de fazer isso é com as instruções de saltos (_jumps_).

### Salto Incondicional

Existem vários tipos de saltos. O mais simples é o salto **incondicional** produzido pela instrução JMP, que possui apenas um operando, podendo ser um valor literal, um registrador ou um endereço de memória. Para entender, analise o programa abaixo:

	   0:    b8 01 00 00 00           mov    eax,0x1
	   5:    eb 03                    jmp    0xa
	   7:    83 c0 04                 add    eax,0x4
	   a:    40                       inc    eax

A instrução ADD EAX, 4 nunca será executada pois o salto faz a execução pular para o endereço 0x0A, onde temos a instrução INC EAX. Portanto, o valor final de EAX será 2.

Note aqui o _opcode_ do salto incondicional JMP, que é o 0xEB. Seu argumento, é o número de _bytes_ que serão pulados, que no nosso caso, são 3. Isso faz a execução pular a instrução ADD EAX, 4 inteira, já que ela tem exatamente 3

Você pode entender o salto incondicional JMP como um comando **goto** na linguagem de programação C.

### Saltos Condicionais Sem Sinal

Os saltos condicionais J\_cc\_ onde _cc_ significa _condition code_, podem ser de vários tipos. O mais famoso deles é o **JE (**_**Jump if Equal**_**)**, utilizado para saltar quando os valores da comparação anterior são iguais. Em geral ele vem precedido de uma instrução CMP, como no exemplo abaixo:

	   0:    b8 01 00 00 00           mov    eax,0x1
	   5:    83 f8 01                 cmp    eax,0x1
	   8:    74 03                    je     0xd
	   a:    83 c0 03                 add    eax,0x3
	   d:    40                       inc    eax

A instrução no endereço 0x5 compara o valor de EAX com 1 e vai sempre resultar em verdadeiro neste caso, o que significa que a _zero flag_ será _ligada_.

O salto JE ocorre se ZF=1, ou seja, se a _zero flag_ estiver _ligada_. Por essa razão, ele também é chamado de **JZ (**_**Jump if Zero**_**)**. Abaixo uma tabela com os saltos que são utilizados para comparações entre números sem sinal e as condições para que o salto ocorra:

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
| JCXZ (CX Zero)   | -                                       | Registrador CX=0  |
| JECXZ (ECX Zero) | -                                       | Registrador ECX=0 |

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

Não se preocupe com a quantidade de diferentes instruções que você apresentadas aqui. O segredo é estudá-las conforme o necessário, na medida em que elas aparecerem nos programas que você analisa. Para avançar, só é preciso que você entenda o conceito do salto. Muitos problemas de engenharia reversa são resolvidos com o entendimento de um simples JE (ZF=1). Se você já entendeu isso, é suficiente para prosseguir. Se não, releia até entender. É normal não compreender tudo de uma vez e vários dos assuntos necessitam de revisão e exercícios para serem completamente entendidos.
