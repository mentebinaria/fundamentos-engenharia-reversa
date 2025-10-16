# Registradores

Os processadores possuem uma área física em seus _chips_ para armazenamento de dados (de fato, somente números, já que é só isso que existe neste mundo!) chamadas de **registradores**, justamente porque registram (salvam) um número por um tempo indeterminado.

Embora os processadores atuais possam trabalhar em diferentes modos, vamos focar aqui no _long mode_, onde processadores atuais trabalham com registradores de 64-bits.

## Registradores de Uso Geral

Um registrador de uso geral, também chamado de GPR (_General Purpose Register_) serve para armazenar temporariamente qualquer tipo de dado, para qualquer função.

Existem 16 registradores de 64-bits de uso geral na arquitetura x86-64. Apesar de poderem ser utilizados para qualquer coisa, como o termo GPR sugere, a seguinte convenção de uso existe:

| Registrador | Significado       | Uso sugerido                          |
| ----------- | ----------------- | ------------------------------------- |
| RAX         | Acumulador        | Usado em operações aritiméticas       |
| RBX         | Base              | Ponteiro para dados                   |
| RCX         | Contador          | Contador em repetições                |
| RDX         | Dados             | Usado em operações de E/S             |
| RSI         | Índice de origem  | Ponteiro para uma _string_ de origem  |
| RDI         | Índice de desitno | Ponteiro para uma _string_ de destino |
| RBP         | Ponteiro base     | Ponteiro para a base do _stack frame_ |
| RSP         | Ponteiro pilha    | Ponteiro para o topo da pilha         |
| R8-15       | -                 | Registradores adicionais              |

Não se preocupe se você não faz ideia de como seria este uso sugerido. Chegaremos lá. Enquanto isso, vamos ver como esses registradores se dividem.

## Subregistradores

Vários GPRs (mas não todos) podem ser subdivididos em registradores menores para fins de compatibilidade. Vamos analisar como isso se dá.

### Subregistradores de RAX, RBX, RCX e RDX

Tomemos o registrador RAX como exemplo. Ele tem 64 bits, mas ele também pode ser usado como um registrador de 32, 16 ou até de 8 bits. O legal é que essas subdivisões têm nome. Acompanhe o esquema a seguir, que detalha o como o registrador RAX divide seus 64 bits, começando pelo bit 0 à direita:

```
  63                            32 31            16 15     8 7      0
 +--------------------------------+----------------+--------+--------+
 | RAX                                                               |
 +--------------------------------+----------------+--------+--------+
                                  | EAX                              |
                                  +----------------+--------+--------+
                                                   | AX              |
                                                   +--------+--------+
                                                   | AH     | AL     |
                                                   +--------+--------+
```

O esquema anterior é poderoso e requer estudo. Cada um destes subregistradores é um registrador, por mais que compartilhem sua memória interna. Analise as afirmações a seguir:

* O registrador EAX tem 32 bits. Dizemos que ele é a **parte baixa da RAX**. Neste contexto, "parte" quer dizer "metade".
* O registrador AX é a parte baixa de EAX. Ele tem 16 bits.
* O registrador AH é a parte **alta** de AX. Ele tem, naturalmente, 8 bits.
* O registrador AL é a parte baixa de AX. Ele tem, também, 8 bits.

Perceba que somente o registrador AX se subdivide em dois registradores: AH para a parte alta e AL para a parte baixa.

Como já dito, os subregistradores compartilham a memória interna de seu registrador "pai". Então, ao fazer:

```
mov rax, 0x1122334455667788 ; copia um número de 64-bits para RAX
```

EAX conterá 0x55667788, AX conterá 0x7788, AH conterá 0x77 e, por fim, AL conterá 0x88. O mesmo se aplica aos registradores RBX, RCX e RDX.

Para facilitar nossa vida, outros registradores se subdividem de outras maneiras. Vejamos. :)

### Subregistradores de RSI, RDI, RBP e RSP

Para explicar a subdivisão destes registradores, vamos usar o RSI como exemplo. Analise:

```
  63                            32 31            16 15     8 7      0
 +--------------------------------+----------------+--------+--------+
 | RSI                                                               |
 +--------------------------------+----------------+--------+--------+
                                  | ESI                              |
                                  +----------------+--------+--------+
                                                   | SI              |
                                                   +--------+--------+
                                                            | SIL    |
                                                            +--------+
```

A única diferença é que não há um subregistrador para a parte alta de SI. Para a parte baixa de SI é o SIL. O mesmo se aplica a RDI, RBP e RSP.

### Subregistradores de R8-R15

Os subregistradores de 64 bits R8, R9, R10, R11, R12, R13, R14 e R15 seguem a mesma lógica anterior: não um registradores que se relacionam com a parte alta do subregistrador de 16-bits deles. No entanto, os nomes deles mudam e por isso cabe o diagrama novamente:

```
  63                            32 31            16 15     8 7      0
 +--------------------------------+----------------+--------+--------+
 | R8                                                               |
 +--------------------------------+----------------+--------+--------+
                                  | R8D                              |
                                  +----------------+--------+--------+
                                                   | R8W             |
                                                   +--------+--------+
                                                            | R8B    |
                                                            +--------+
```

Usei o R8 como exemplo, mas a lógica dos demais é a mesma.

## Exercícios

Para fixar o assunto, é importante trabalhar um pouco. Aba o **flat assembler (fasm)** e escreva o seguinte programa:

```
format PE64 GUI
entry start

section '.text' code readable executable

  start:
        mov eax, 0x20
        or eax, 0x18
```

Este pequeno programa em Assembly faz algum sentido para você? Vamos comentá-lo:

* Na **linha 1** estamos dizendo que o arquivo de saída será um PE de 64-bits. Apesar de o nome oficial ser PE32+, muita gente "forçou" o uso do termo PE64 em alguns lugares e é assim que informamos o **fasm** para usar este formato. Ainda na linha 1, temos a palavra **GUI**. Ela pede ao fasm que coloque o valor 2 naquele campo SubSystem do cabeçalho Opcional, lembra? :)
* A **linha 2** define o endereço do _entrypoint_ através de um rótulo (_label_ em inglês). Será explicado melhor na linha 6.
* A **linha 4** cria uma seção chamada .text que conterá código e que precisa ser mapeada em páginas de memória com permissões de leitura e execução. Se isso soa familiar, é porque realmente o é. :)
* A **linha 6** define onde o rótulo start começa, dentro da seção .text. Ou seja, o _entrypoint_ configurado na linha 2 será seja qual for o endereço do primeiro _byte_ da seção .text.
* Nas **linhas 7 e 8** temos as instruções em Assembly que desejamos codificar. Em outras palavras, converter para código de máquina.

Agora é só pedir para o fasm fazer o trabalho. Salve o arquivo como `ou.asm` e, para compilar, clique em **Run ► Compile** ou pressione `Ctrl+F9`. Um arquivo `ou.exe` será gerado no mesmo diretório onde você salvou seu código-fonte.

Abra agora o `ou.exe` no HxD. Como é um programa pequeno, mesmo no olho você consegue notar as instruções que codificou. Destaquei o campo PointerToRawData da seção .text que aponta para o início da seção .text na imagem a seguir.

!\[Instruções MOV e OR na seção .text]\[image-1]

Perceba os _opcodes_ e argumentos idênticos aos exemplificados na introdução deste capítulo.

## Ponteiro de Instrução

Existe um registrador de 64 bits chamado de RIP (o IP quer dizer _Instruction Pointer_), ou também de PC (_Program Counter_) em algumas literaturas, que aponta para a próxima instrução a ser executada. Não é possível copiar um valor literal para este registrador. O valor dele é atualizado de um jeito especial: ele é incrementado com o número de _bytes_ da última instrução executada. Para fixar, analise o exemplo a seguir.

| Endereço Virtual (VA) | Opcodes e parâmetros | Assembly                  |
| --------------------- | -------------------- | ------------------------- |
| 140001740             | 48B88877665544332211 | mov rax, 1122334455667788 |
| 14000174A             | 4831C0               | xor rax, rax              |

Quando a primeira instrução do trecho acima estiver prestes à ser executada, o registrador RIP conterá o valor 0x140001740. Após a execução desta instrução MOV, o RIP será incrementado em 10 unidades, já que tal instrução possui 10 _bytes_. Por isso o endereço da instrução seguinte é 0x14000174A. Perceba, no entanto, que a instrução no endereço 0x14000174A possui apenas 3 _bytes_, o que vai fazer com o que o registrador RIP seja incrementado em 3 unidades para apontar para a próxima instrução. Qual será o valor do registrador RIP após a execução desta instrução? Se você pensou em 0x14000174D, acertou.\
Vamos agora ver um registrador muito importante sobre o qual temos pouco controle, mas que precisamos entender bem.

## Registrador de Flags

O registrador de _flags_ RFLAGS é um registrador de 64-bits usado para armazenar _flags_ de estado, de controle e de sistema.

> _Flag_ é um termo genérico para um dado, normalmente "verdadeiro ou falso". Dizemos que uma _flag_ está _setada_ quando seu valor é verdadeiro, ou seja, é igual a 1.

Existem 10 _flags_ de sistema, uma de controle e 6 de estado. As _flags_ de estado são utilizadas pelo processador para reportar o estado da última operação (pense numa comparação, por exemplo - você pede para o processador comparar dois valores e a resposta vem através de uma _flag_ de estado). As mais comuns são:

| Bit | Nome     | Sigla | Descrição                                                                                                                                                                                                |
| --- | -------- | ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0   | Carry    | CF    | Setada quando o resultado estourou o limite do tamanho do dado. É o famoso "vai-um" na matemática para números sem sinal (_unsigned_).                                                                   |
| 6   | Zero     | ZF    | Setada quando o resultado de uma operação é zero. Do contrário, é zerada. Muito usada em comparações.                                                                                                    |
| 7   | Sign     | SF    | Setada de acordo com o MSB (_Most Significant Bit_) do resultado, que é justamente o _bit_ que define se um inteiro com sinal é positivo (0) ou negativo (1), conforme visto na seção Números negativos. |
| 11  | Overflow | OF    | Estouro para números com sinal.                                                                                                                                                                          |

Além das outras _flags_, há ainda os registradores de segmento, da FPU (_Float Point Unit_), de debug, de controle, XMM (parte da extensão SSE), MMX, 3DNow!, MSR (_Model-Specific Registers_), e possivelmente outros que não abordaremos neste livro em prol da brevidade.

Agora que já reunimos bastante informação sobre os registradores, é hora de treinarmos um pouco com as instruções básicas do Assembly.
