# Breakpoints

Um _breakpoint_ nada mais é que um ponto no código onde o _debugger_ vai parar para que você analise o que precisa. É o mesmo conceito dos _breakpoints_ presentes nos IDEs como Visual Studio, NetBeans, CodeBlocks, etc. A diferença é que nestas IDEs colocamos _breakpoints_ em determinadas linhas do código-fonte. Já nos _debuggers_ destinados à engenharia reversa, colocamos _breakpoints_ em **endereços** (VAs), onde há instruções.

## Seu Primeiro _Breakpoint_

Há várias maneiras de se colocar um _breakpoint_ em um endereço utilizando o x64dbg. Você pode selecionar a instrução e pressionar F2, usar um dos comandos SetBPX/bp/bpx, dar um duplo clique sobre os _bytes_ da instrução ou simplesmente clicar na pequena bolinha cinza à esquerda do endereço. Ao fazê-lo, este ficará com um fundo vermelho, como sugere a imagem:

![Colocando um breakpoint na CALL][image-1]

Um segundo clique desabilita o _breakpoint_, mas não o exclui da aba **Breakpoints** (Alt+B). Um terceiro clique de fato o deleta.

Após colocar o _breakpoint_ nesta CALL, rode o programa (F9). O que acontece? O _debugger_ executa todas as instruções anteriores a este _breakpoint_ e pára onde você pediu. Simples assim.

## Como _Breakpoints_ são Implementados

Talvez você tenha notado que ao atingir um _breakpoint_, o x64dbg mostra na barra de status a palavra “Paused” e a frase ** INT3 breakpoint at analyzeme00.0000000140001019!**. Este é um tipo de **breakpoint de software**. Existem ainda os de memória e de hardware, mas não trataremos neste livro.

A instrução INT é uma instrução Assembly que gera uma interrupção. A interrupção número 3 é chamada de **Breakpoint Exception (#BP)** no manual da Intel. Seu _opcode_ (0xcc) tem somente um _byte_, o que facilita sua implementação nos _debuggers_.

De forma resumida, para parar nesta CALL, o que o x64dbg faz é:

1. Substituir o primeiro _byte_ do _opcode_ da CALL (0xff, neste caso) por 0xcc e salvar o original numa memória.
2. Rodar o programa.
3. Restaurar o primeiro _byte_ do _opcode_ da CALL, substituindo o 0xcc por 0xff (neste caso).

Isso poderia ser feito manualmente, mas os _debuggers_ facilitam o trabalho, bastando você pressionar F2 ou clicar na bolinha para que todo este trabalho seja executado em segundo plano, sem que o usuário perceba. Incrível, não é?

Você pode adicionar quantos _breakpoints_ de software quiser numa sessão de _debugging_. Todos ficam acessíveis na aba **Breakpoints**, a não ser que você os exclua. Veja a imagem abaixo:

![Lista de breakpoints][image-2]

Você também pode assistir a [aula do CERO][1], que trata sobre este assunto.

[1]:	https://youtu.be/823KK-FYV9s?si=S8VwxxvJHwoYbiEa

[image-1]:	../.gitbook/assets/x32dbg%5C_03%5C_breakpoint.png
[image-2]:	../.gitbook/assets/x32dbg%5C_04%5C_breakpoints.png