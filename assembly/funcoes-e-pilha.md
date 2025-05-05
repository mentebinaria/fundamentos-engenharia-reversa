# Funções e Pilha

Apesar de não estudarmos todos os aspectos da linguagem Assembly, alguns assuntos são de extrema importância, mesmo para os fundamentos da engenharia reversa de software. Um deles é como funcionam as funções criadas em um programa e suas chamadas, que discutiremos agora.

## O que é uma Função

Basicamente, uma função é um **bloco de código reutilizável** num programa. Tal bloco faz-se útil quando um determinado conjunto de instruções precisa ser invocado em pontos diferentes no programa. Por exemplo, suponha que um programa em Python precise converter a temperatura de Fahrenheit para Celsius várias vezes no decorrer de seu código. Ele pode ser escrito assim:

```python
fahrenheit = 230.4
celsius = (fahrenheit - 32) * 5 / 9
print(celsius)

fahrenheit = 130.3
celsius = (fahrenheit - 32) * 5 / 9
print(celsius)

fahrenheit = 90.1
celsius = (fahrenheit - 32) * 5 / 9
print(celsius)
```

O programa funciona e a saída é a esperada:

```
110.22222222222223
54.611111111111114
32.27777777777778
```

No entanto, é pouco prático, pois repetimos o mesmo código várias vezes. Além disso, uma versão compilada geraria o mesmo conjunto de instruções várias vezes, ocupando um espaço desnecessário no binário final. Toda esta repetição também prejudica a manutenção do código, pois se precisarmos fazer uma alteração no cálculo, teríamos que alterar em todos os pontos onde o cálculo é feito. É aí que entram as funções. Analise a seguinte versão do mesmo programa:

```python
def fahrenheit2celsius(fahrenheit):
    return (fahrenheit - 32) * 5 / 9

celsius = fahrenheit2celsius(230.4)
print(celsius)

celsius = fahrenheit2celsius(130.3)
print(celsius)

celsius = fahrenheit2celsius(90.1)
print(celsius)
```

A saída é a mesma, mas agora o programa está utilizando uma função, onde o cálculo só foi definido uma única vez e toda vez que for necessário, o programa a chama.

Uma função pode ter:

1. Argumentos, também chamados de parâmetros, que são os dados que a função recebe, necessários para cumprir seu propósito.
2. Retorno, que é o resultado da conclusão do seu propósito.
3. Um nome (na visão de quem programa) ou um endereço de memória (na visão do processador).

Agora cabe a nós estudar como isso tudo funciona em baixo nível.

> Nos primórdios da computação as funções eram chamadas de **procedimentos** (_procedures_). Algumas linguagens mais antas de programação, no entanto, possuem tanto funções quanto procedimentos. Estes últimos são "funções que não retornam nada". É possível também que você encontre estes termos sendo usados como sinônimos.

## Funções em Assembly

Em baixo nível, uma função é implementada basicamente num bloco que não será executado até ser chamado por uma instrução CALL. Ao final de uma função, encontramos normalmente a instrução RET. Vamos analisar um programa cuja função principal chama uma simples função de soma:

```c
int soma(int x, int y) {
	return x + y;
}

int main(void) {
	int res = soma(3, 4);
	return 0;
}
```

Olha como este programa pode ficar ao ser compilado no Windows em 64-bits:

```
<soma>:
	140001010 | add ecx, edx
	140001012 | mov eax, ecx
	140001014 | ret

<main>:
    140001020 | sub rsp, 38                                  
	140001024 | mov edx, 4
	140001029 | mov ecx, 3
	14000102E | call 140001010
	140001033 | mov dword ptr ss:[rsp+20], eax
	140001037 | xor eax, eax
	140001039 | add rsp, 38
	14000103D | ret
```

O objetivo neste momento é apresentar as instruções que implementam as chamadas de função. Por hora, você só precisa entender que a instrução CALL (no endereço 0x14000102E em nosso exemplo) chama a função _soma()_ em 0x140001010 e a instrução RET (em 0x140001014) retorna para a instrução imediatamente após a CALL (0x140001033), para que a execução continue.

Uma vez entendido isso, vamos agora ver como os argumentos são passados para as funções.

## Passagem de parâmetros

A escolha das instruções que serão utilizadas para representar fielmente um código-fonte em uma linguagem de alto nível é uma decisão do compilador. No caso do exemplo com a função _soma()_, isso fica a cargo do compilador de C. Há incontáveis maneiras de se fazer a mesma coisa, o que pode envolver o uso de diferentes instruções, em diferentes contextos.

Mas há uma área onde o sistema operacional coloca algumas regras. Uma delas diz respeito a como as funções serão chamadas pelos binários compilados. Essas regras são conhecidas como **convenções de chamadas**. Elas fazem parte do que chamamos de _Application Binary Interface (ABI)_, um conjunto de regras para os compiladores seguirem de modo que tudo corra bem com os binários compilados.

A convenção mais utilizada no Windows em 64-bits estabelece, dentre outras coisas, que:

- Os parâmetros do tipo inteiro serão passados nos registradores RCX, RDX, R8 e R9, nesta ordem.
- Se houver mais de quatro parâmetros, os excedentes são passados pela pilha. Falaremos mais da pilha em breve.
- O retorno é em RAX.

Voltando ao nosso exemplo de código, o trecho `soma(3, 4)` gerou, em Assembly:

```x86
mov edx, 4
mov ecx, 3
call 140001010
```

A convenção foi de fato seguida. O segundo parâmetro, o literal 4, foi posto em EDX. Como este MOV zera a parte alta de RDX, é o mesmo que dizer que o parâmetro foi posto em RDX.

O segundo parâmetro foi posto em RCX normalmente.

Agora vamos analisar como a função `soma()` recupera os parâmetros e retorna:

```x86
add ecx, edx
mov eax, ecx
ret
```

A primeira instrução soma os parâmetros recebidos e os armazenas em ECX. A segunda copia este resultado para EAX, porque o retorno precisa estar nele. Depois vem o RET, que desempilha o endereço da instrução após a CALL e põe em RIP.

Estamos falando em pilha tem tempo, mas ainda não a detalhamos. Vamos agora entender como essa estrutura funciona.

## A Pilha de Memória

A memória RAM para um processo é dividida em áreas com diferentes propósitos. Uma delas é a pilha, ou _stack_ em inglês.

Essa área de memória funciona de forma que o que é colocado lá fica no topo e o último dado colocado na pilha é o primeiro a ser retirado, como uma pilha de pratos ou de cartas de baralho. Esse esquema é conhecido por LIFO (_Last In First Out_), ou seja, o “o último dado a entrar é o primeiro a sair”.

Existem duas operações possíveis na pilha:

1. Adicionar um dado (empilhar).
2. Remover um dado (desempilhar).

Tanto para empilhar quanto para desempilhar um dado, é necessário conhecer o endereço do topo da pilha. O registrador que, por convenção, sempre tem essa informação é o RSP.

Veremos agora as instruções de manipulação de pilha. A primeira é a instrução PUSH (do inglês "empurrar") que, como o nome sugere, empilha um dado. Na forma abaixo, essa instrução faz com que o processador copie o conteúdo do registrador RAX para o topo da pilha:

```asm
push rax
```

Também é possível empilhar um valor literal. Por exemplo, supondo que o programa coloque o valor um na pilha:

```asm
push 1
```

Além de copiar o valor para o topo da pilha, a instrução PUSH **decrementa** o registrador RSP em 8 unidades, o tamanho da palavra em 64-bits.

Sua instrução antagônica é a POP, que só precisa de um registrador de destino para copiar lá o valor que está no topo da pilha. Por exemplo:

```asm
pop rdx
```

Seja lá o que estiver no topo da pilha, será copiado para o registrador RDX. Além disso, o registrador RSP será **incrementado** em 8 unidades.

Temos também a instrução CALL, que faz duas coisas:

1. Empilha o endereço da próxima instrução.
2. Coloca o seu parâmetro, ou seja, o endereço da função a ser chamada, no registrador RIP.

Por conta dessa atualização do RIP, o fluxo é desviado para o endereço da função chamada. A ideia de colocar o endereço da próxima instrução na pilha é para o processador saber para onde tem que voltar quando a função terminar. E, falando em terminar, a estrela do fim da festa é a instrução RET (de _RETURN_). Ela faz uma única coisa:

1. Retira um valor do topo da pilha e coloca no RIP.

Isso faz com que o fluxo de execução do programa volte para a instrução imediatamente após a CALL, que chamou a função.

## Passagem de parâmetros pela pilha

Em 32-bits, as convenções de chamadas mais usadas usavam a pilha quase que exclusivamente para a passagem de parâmetros, mas aqui em 64-bits ela só é usada para funções com mais de quatro parâmetros. Vamos ver um exemplo comentado:

```asm
; reserva 72 bytes (0x48) na pilha
sub rsp, 48
; copia o sexto argumento para a pilha
mov dword ptr ss:[rsp+28], 6
; copia o quinto argumento para a pilha
mov dword ptr ss:[rsp+20], 5
; quarto argumento em R9D
mov r9d, 4
; terceiro em R8D
mov r8d, 3
; segundo em EDX
mov edx, 2
; primeiro em ECX
mov ecx, 1
; empilha o endereço da MOV após a CALL
; e desvia o fluxo para a função soma()
call soma.140001010
; armazena o retorno numa variável local na pilha
mov dword ptr ss:[rsp+30], eax
; zera EAX, que contém o retorno da main()
xor eax,eax
; libera os bytes pré-reservados
add rsp, 48
; retorna para o sistema operacional / fim da main()
ret
```

Para este exemplo eu não pus o código-fonte de propósito, afinal este é um livro de engenharia reversa e precisamos começar a nos acostumar com isso. :)

Este exemplo pode gerar dúvidas. Vamos lá:

**Por que reservar tanto espaço na pilha?**
Esta foi uma decisão do compilador. Acontece que a convenção de chamadas é um pouco mais complexa do que cobrimos aqui. Existe um espaço chamado de _shadow space_ que precisa ser reservado. Ele existe por vários motivos, mas o principal é que a função chamada pode salvar os parâmetros recebidos por registradores na pilha se precisar. Só ele já precisa de 32 bytes pois são quatro parâmetros passados por registrador, de 8 bytes cada.

**Ok, mas e os outros 40 bytes?**
Destes 40, 16 serão usados pelos dois argumentos copiados para a pilha (os literais 6 e 5). Outros 8 são usados para a variável local que guarda o resultado. E por fim, há um alinhamento em 16 bytes exigido pela ABI. O assunto foge do nosso escopo aqui, mas encorajo você a pesquisar sobre.

**O que é dword ptr ss:[rsp+28]?**
- “dword” significa _double word_ e isto nos diz que a instrução está trabalhando com dados de 4 _bytes_.
- "ss" abrevia _stack segment_ e nos conta que o endereço está na _stack_.
- Os colchetes são uma derreferência. Significa que o conteúdo sera armazenado (isto está no operado de destino de um MOV) no endereço apontado por RSP + 28 (em hexa).

Sabendo disso, o código que gerou essas instruções provavelmente foi algo como:

```c
int main(void) {
	int res = soma(1, 2, 3, 4, 5, 6);
	return 0;
}
```

Uma curiosidade: mesmo sem saber o tamanho de um tipo `int` em C, pelos registradores usados nas instruções, dá para saber que são de 32-bits. No final é isso: para quem lê Assembly, todo programa é _open source_. :)

Com isso podemos partir para uma análise mais real. Na próxima seção vamos ver como fica um programa que usa uma função da API do Windows em Assembly.

## Análise da MessageBox

Veja este código:

```
sub rsp, 28
xor r9d, r9d
lea r8, qword ptr ds:[140002020]
lea rdx, qword ptr ds:[140002038]
xor ecx, ecx
call qword ptr ds:[<MessageBoxW>]
xor ecx, ecx
call qword ptr ds:[<ExitProcess>]
nop
add rsp, 28
ret
```

Mesmo que não conhecêssemos a função MessageBoxW, dá para ver que ela está recebendo 4 parâmetros. Considerando a convenção de chamadas, temos:

```
MessageBoxW(0, 0x14002038, 0x140002020, 0);
```

O primeiro zero é o NULL do C. Depois vem o endereço da string que possui o conteúdo a ser exibido na mensagem, seguido pelo endereço da string de título. Por fim, outro zero, provavelmente expandido de `MB_OK`. Sabendo que cada instrução é composta de bytes (opcodes e parâmetros), no que você acha que consiste a engenharia reversa senão em entender e poder alterar tais bytes de acordo com o que desejarmos? É este o poder que a engenharia reversa te dá, mas ela pede algo em troca: estudo. Veja o quanto você já leu até chegar aqui. Parabéns!

Assembly é, por si só, um assunto extenso e bastante atrelado à arquitetura na qual se está trabalhando. Este capítulo apresentou uma introdução ao Assembly x86-64 e considerou o Windows como plataforma. Dois bons recursos de Assembly, são os livros gratuitos **Aprendendo Assembl**y, do Felipe Silva e **Linguagem Assembly para i386 e x86-64**, do Frederico Pissara, ambos disponíveis em menteb.in.
