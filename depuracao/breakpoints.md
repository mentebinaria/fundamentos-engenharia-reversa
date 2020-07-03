# Breakpoints

Um *breakpoint* nada mais é que um ponto do código onde o debugger vai parar para que você analise o que precisa. É o mesmo conceito dos *breakpoints* presentes nos IDE's como Visual Studio, NetBeans, CodeBlocks, etc. A diferença é que nestas IDE's colocamos *breakpoints* em determinadas linhas do código-fonte. Já nos debuggers destinados à engenharia reversa, colocamos *breakpoints* em **endereços** (VA's), onde há instruções.

## Seu primeiro *breakpoint*

Há várias maneiras de se colocar um *breakpoint* em um endereço utilizando o x64dbg. Você pode selecionar a instrução e pressionar F2, usar um dos comandos SetBPX/bp/bpx ou simplesmente clicar na pequena bolinha cinza à esquerda do endereço. Ao fazê-lo, este ficará com um fundo vermelho, como sugere a imagem:

![Colocando um breakpoint na CALL](../.gitbook/assets/x32dbg_03_breakpoint.png)

Um segundo clique desabilita o *breakpoint*, mas não o exclui da aba **Breakpoints** (Alt+B). O terceiro clique de fato o deleta.

Após colocar o *breakpoint* nesta CALL, rode o programa (F9). O que acontece? O debugger executa todas as instruções anteriores a este *breakpoint* e pára onde você pediu. Simples assim.

## Como *breakpoints* são implementados

Talvez você tenha notado que ao atingir um *breakpoint*, o x64dbg denuncia na barra de status: **INT3 breakpoint at analyseme-00.00401007 (00401007)!**. Este é um tipo de **breakpoint de software**. Existem ainda os de memória e de hardware, mas não trataremos neste curso básico.

A instrução INT é uma instrução Assembly que gera uma interrupção. A interrupção número 3 é chamada de **Breakpoint Exception (#BP)** no manual da Intel. Seu opcode (0xcc) tem somente um *byte*, o que facilita para que os debuggers implementem-na.

De forma resumida, para parar nesta CALL, o que o x64dbg faz é:

1. Substituir o primeiro *byte* do opcode da CALL (0xff, neste caso) por 0xcc e salvar o original numa memória.
2. Executar o programa.
3. Restaurar o primeiro *byte* do opcode da CALL, substituindo o 0xcc por 0xff (neste caso).

Isso poderia ser feito manualmente, mas os debuggers facilitam o trabalho, bastando você pressionar F2 ou clicar na bolinha para que todo este trabalho seja executado em segundo plano, sem que o usuário perceba. Incrível, não? :)

Você pode adicionar quantos *breakpoints* de software quiser numa sessão de debugging. Todos ficam salvos na aba **Breakpoints**, a não ser que você os exclua. Veja a imagem abaixo:

![Lista de breakpoints](../.gitbook/assets/x32dbg_04_breakpoints.png)

Você também pode assistir a aula a seguir do CERO, que trata sobre este assunto.

{% embed url="https://www.youtube.com/watch?v=823KK-FYV9s" caption="Curso de Engenharia Reversa Online - Aula 16 - Breakpoints de software" %}
