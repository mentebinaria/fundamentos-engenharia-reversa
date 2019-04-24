# Registradores

Os processadores possuem uma área física em seus _chips_ para armazenamento de dados \(de fato, somente números, já que é só isso que existe neste mundo!\) chamadas de **registradores**, justamente porque registram \(salvam\) um número por um tempo, mesmo este sendo não determinado.

Os registradores possuem normalmente o tamanho da palavra do processador, logo, se este é um processador de 32-bits, seus registradores possuem este tamanho também.

### Registradores de uso geral

Um registrador de uso geral, também chamado de GPR \(_General Purpose Register_\) serve para armazenar temporariamente qualquer tipo de dado. :-\)

Existem 8 registradores de uso geral na arquitetura Intel x86. Apesar de poderem ser utilizados para qualquer coisa, como seu nome sugere, a seguinte convenção é normalmente respeitada:

| Registrador | Significado | Uso sugerido |
| :--- | :--- | :--- |
| EAX | Accumulator | Usado em operações aritiméticas |
| EBX | Base | Ponteiro para dados |
| ECX | Counter | Contador em repetições |
| EDX | Data | Usado em operações de E/S |
| ESI | Source Index | Ponteiro para uma _string_ de origem |
| EDI | Destination Index | Ponteiro para uma _string_ de destino |
| ESP | Stack Pointer | Ponteiro para o topo da pilha |
| EBP | Base Pointer | Ponteiro para a base do _stack frame_ |

Para fixar o assunto, é importante trabalhar um pouco. Vamos escrever o seguinte programa em Assembly no Linux ou macOS:

{% code-tabs %}
{% code-tabs-item title="ou.s" %}
```assembly
global start

section .text
start:
	mov eax, 0x30
	or eax, 0x18
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Para compilar, é preciso instalar o [NASM](https://www.nasm.us/), um moderno compilador Assembly:

{% code-tabs %}
{% code-tabs-item title="Linux" %}
```text
$ sudo apt install nasm
$ nasm -felf ou.s
```
{% endcode-tabs-item %}

{% code-tabs-item title="macOS" %}
```
$ brew install nasm
$ nasm -fmacho ou.s
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Confira como ficou o código **compilado** no arquivo objeto com a ferramenta **objdump**, do pacote [binutils](https://www.gnu.org/software/binutils/):

{% code-tabs %}
{% code-tabs-item title="Linux" %}
```text
$ objdump -dM intel ou.o

ou.o:     file format elf32-i386


Disassembly of section .text:

00000000 <start>:
   0:	b8 30 00 00 00       	mov    eax,0x30
   5:	83 c8 18             	or     eax,0x18
```
{% endcode-tabs-item %}

{% code-tabs-item title="macOS" %}
```
$ objdump -d -x86-asm-syntax=intel -print-imm-hex ou.o

ou.o:	file format Mach-O 32-bit i386

Disassembly of section __TEXT,__text:
start:
       0:	b8 30 00 00 00 	mov	eax, 0x30
       5:	83 c8 18 	or	eax, 0x18
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Percebe os _opcodes_ e argumentos idênticos aos exemplificados na introdução deste capítulo? Prova que não mentimos. rs

{% hint style="info" %}
O NASM compilou corretamente a linguagem Assembly para código de máquina. O objdump mostrou tanto o código de máquina quanto o equivalente em Assembly, processo conhecido como **desmontagem** ou _**disassembly**_.
{% endhint %}

Agora cabe à você fazer mais alguns testes com outros registradores de uso geral. Um detalhe importante é que os primeiros quatro registradores de uso geral podem ter sua parte baixa manipulada diretamente. Por exemplo, é possível trabalhar com o registrador de 16 bits AX, a parte baixa de EAX. Já o AX pode ter tanto sua parte alta \(AH\) quanto sua parte baixa \(AL\) manipulada diretamente também, onde ambas possuem 8 bits de tamanho. O mesmo vale para EBX, ECX e EDX.

Para entender, analise as seguintes instruções:

```assembly
mov eax, 0xaabbccdd
mov ax, 0xeeff
```

Após a execução das instruções acima, EAX conterá o valor **0xaabbeeff**, já que somente sua parte **baixa** foi modificada pela segunda instrução. Agora analise o seguinte trecho:

```assembly
mov eax, 0xaabbccdd
mov ax, 0xeeff
mov ah, 0xcc
mov al, 0xdd
```

Após a execução das quatro instruções acima, EAX volta para o valor **0xaabbccdd**.

A imagem a seguir ilustra os ditos vários registradores dentro dos quatro primeiros registradores de uso geral.

![Sub-divisões de EAX, EBX, ECX e EDX](../.gitbook/assets/registradores.png)

Os registradores EBP, ESI, EDI e ESP também podem ser utilizados como registradores de 16-bits BP, SI, DI e SP, respectivamente. Note porém que estes últimos não são sub-divididos em partes alta \(_high_\) e baixa \(_low_\).

### Registradores de segmento

Estes registradores armazenam o que chamamos de seletores, ponteiros que identificam segmentos na memória, essenciais para operação em modo real. Em modo protegido, que é o modo de operação do processador que os sistemas operacionais modernos utilizam, a função de cada registrador de segmento fica a cargo do SO. Abaixo a lista dos registradores de segmento e sua função em modo protegido:

| Registrador | Significado |
| :--- | :--- |
| CS | Code Segment |
| DS | Data Segment |
| ES | Data Segment |
| FS | Data Segment \(no Windows em x86, aponta para o o [TEB](https://msdn.microsoft.com/en-us/library/windows/desktop/ms686708%28v=vs.85%29.aspx) \(_Thread Environment Block_\) da _thread_ atual do processo em execução\) |
| GS | Data Segment |

Não entraremos em mais detalhes sobre estes registradores por fugirem do escopo deste livro.

### Registrador de flags

O registro de _flags_ EFLAGS é um registrador de 32-bits usado para _flags_ de estado, de controle e de sistema.

{% hint style="info" %}
_Flag_ é um termo genérico para um dado, normalmente "verdadeiro ou falso". Dizemos que uma _flag_ está _setada_ quando seu valor é verdadeiro, ou seja, é igual a 1.
{% endhint %}

Existem 10 _flags_ de sistema, uma de controle e 6 de estado, onde as mais comuns são:

| Nome | Sigla | Descrição |
| :--- | :--- | :--- |
| Zero | ZF | Setada quando o resultado de uma operação é zero. Do contrário, é zerada. Muito usada em comparações. |
| Sign | SF | Setada de acordo com o MSB \(_Most Significant Bit_\) do resultado, que é justamente o _bit_ que define se um inteiro com sinal é positivo \(0\) ou negativo \(1\), conforme visto na seção Números negativos. |
| Carry | CF | Setada quando o resultado estourou o limite do tamanho do dado. É o famoso "vai-um" para números sem sinal \(_unsigned_\). |
| Overflow | OF | Estouro para números com sinal. |

Além das outras _flags_, há ainda os registradores de debug, MMX, 3DNow!, MSR \(_Model-Specific Registers_\), dentre outros, que fogem do escopo deste livro.

Agora que já reunimos bastante informação sobre os registradores, é hora de treinar um pouco com as instruções básicas do Assembly.

