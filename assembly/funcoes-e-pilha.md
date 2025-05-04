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
	140001000 | mov dword ptr ss:[rsp+10], edx
	140001004 | mov dword ptr ss:[rsp+8], ecx
	140001008 | mov eax, dword ptr ss:[rsp+10]
	14000100C | mov ecx, dword ptr ss:[rsp+8]
	140001010 | add ecx, eax
	140001012 | mov eax, ecx
	140001014 | ret

<main>:
    140001020 | sub rsp, 38                                  
	140001024 | mov edx, 4
	140001029 | mov ecx, 3
	14000102E | call 140001000
	140001033 | mov dword ptr ss:[rsp+20], eax
	140001037 | xor eax, eax
	140001039 | add rsp, 38
	14000103D | ret
```

O objetivo neste momento é apresentar as instruções que implementam as chamadas de função. Por hora, você só precisa entender que a instrução CALL (no endereço 0x14000102E em nosso exemplo) chama a função _soma()_ em 0x140001000 e a instrução RET (em 0x140001014) retorna para a instrução imediatamente após a CALL (0x140001033), para que a execução continue.

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
call 140001000
```

A convenção foi de fato seguida. O segundo parâmetro, o literal 4, foi posto em EDX. Como este MOV zera a parte alta de RDX, é o mesmo que dizer que o parâmetro foi posto em RDX.

O segundo parâmetro foi posto em RCX normalmente. Falta entender como a função recuperou estes parâmetros. Para isso, vamos falar de pilha.

## A Pilha de Memória

A memória RAM para um processo é dividida em áreas com diferentes propósitos. Uma delas é a pilha, ou _stack_.

Essa área de memória funciona de forma que o que é colocado lá fique no topo e o último dado colocado na pilha seja o primeiro a ser retirado, como uma pilha de pratos ou de cartas de baralho mesmo. Esse método é conhecido por LIFO (_Last In First Out_).

Seu principal uso é no uso de funções, tanto para passagem de argumentos (parâmetros da função) quanto para alocação de variáveis locais da função (que só existem enquanto a função executa).

Na arquitetura IA-32, a pilha é alinhada em 4 _bytes_ (32-bits). Por consequência, todos os seus endereços também o são. Logo, se novos dados são colocados na pilha (empilhados), o endereço do topo é **decrementado** em 4 unidades. Se um dado for desempilhado, o endereço do topo é **incrementado** em 4 unidades. Perceba a lógica invertida, porque a pilha começa num endereço alto e cresce em direção a endereços menores.

Existem dois registradores diretamente associados com a pilha de memória alocada para um processo. São eles:

* O ESP, que aponta para o topo da pilha.
* O EBP, que aponta para a base do _stack frame_.

Veremos agora as instruções de manipulação de pilha. A primeira é a instrução PUSH (do inglês "empurrar") que, como o nome sugere, empilha um dado. Na forma abaixo, essa instrução faz com que o processador copie o conteúdo do registrador EAX para o topo da pilha:

```asm
push eax
```

Também é possível empilhar um valor literal. Por exemplo, supondo que o programa coloque o valor um na pilha:

```asm
push 1
```

Além de copiar o valor proposto para o topo da pilha, a instrução PUSH **decrementa** o registrador ESP em 4 unidades, conforme já explicado o motivo. Sempre.

Sua instrução antagônica é a POP, que só precisa de um registrador de destino para copiar lá o valor que está no topo da pilha. Por exemplo:

```asm
pop edx
```

Seja lá o que estiver no topo da pilha, será copiado para o registrador EDX. Além disso, o registrador ESP será **incrementado** em 4 unidades. Sempre.

Temos também a instrução CALL, que faz duas coisas:

1. Coloca o endereço da próxima instrução na pilha de memória (no caso do exemplo, 0x8048432).
2. Coloca o seu parâmetro, ou seja, o endereço da função a ser chamada, no registrador EIP (no exemplo é o endereço 0x804840b).

Por conta dessa atualização do EIP, o fluxo é desviado para o endereço da função chamada. A ideia de colocar o endereço da próxima instrução na pilha é para o processador saber para onde tem que voltar quando a função terminar. E, falando em terminar, a estrela do fim da festa é a instrução RET (de _RETURN_). Ela faz uma única coisa:

1. Retira um valor do topo da pilha e coloca no EIP.

Isso faz com que o fluxo de execução do programa volte para a instrução imediatamente após a CALL, que chamou a função.



```x86
mov dword ptr ss:[rsp+10], edx
mov dword ptr ss:[rsp+8], ecx
mov eax, dword ptr ss:[rsp+10]
mov ecx, dword ptr ss:[rsp+8]
add ecx, eax
mov eax, ecx
ret
```

Parece difícil, mas não é. Vamos juntos:

- A primeira instrução pega o valor de EDX (segundo parâmetro, o 4) e copia para 

## Análise da MessageBox

Vamos agora analisar a pilha de memória num exemplo com a função MessageBox, da API do Windows:

```
00401516 | push 31                                     |
00401518 | push msgbox.404000                          | 404000:"Johnny"
0040151D | push msgbox.404007                          | 404007:"Cash"
00401522 | push 0                                      |
00401524 | call <user32.MessageBoxA>                   |
```

Perceba que quatro parâmetros são empilhados antes da chamada à _MessageBoxA_ (versão da função _MessageBox_ que recebe _strings_ ASCII, por isso o sufixo **A**).

Os parâmetros são empilhados na ordem inversa.

Já estudamos o protótipo desta função no capítulo que apresenta a Windows API e por isso sabemos que o 0x31, empilhado em 00401516, é o parâmetro `uType` e, se o decompormos, veremos que 0x31 é um OU entre 0x30 (MB\_ICONEXCLAMATION) e 0x1 (MB\_OKCANCEL).

O próximo parâmetro é o número 404000, um ponteiro para a _string_ "Johnny", que é o título da mensagem. Depois vem o ponteiro para o texto da mensagem e por fim o zero (NULL), empilhado em 00401522, que é o _handle_.

O resultado é apresentado a seguir:

![Resultado da chamada à MessageBox][image-1]

É importante perceber que, após serem compreendidos, podemos controlar estes parâmetros e alterar a execução do programa conforme quisermos. Este é o assunto do próximo capítulo, sobre depuração.

Assembly é, por si só, um assunto extenso e bastante atrelado à arquitetura e ao sistema operacional no qual se está trabalhando. Este capítulo apresentou uma introdução ao Assembly Intel x86 e considerou o Windows como plataforma. Dois bons recursos de Assembly, que tomam o Linux como sistema base, são os livros gratuito Aprendendo Assembly, do Felipe Silva e Linguagem Assembly para i386 e x86-64, do Frederico Pissara.

[image-1]:	../.gitbook/assets/msgbox.png