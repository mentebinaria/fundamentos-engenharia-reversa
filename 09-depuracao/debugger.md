# O Debugger

## Instalação

1. Na sua máquina Windows, baixe o _snapshot_ mais recente do x64dbg. É um arquivo .zip chamado _snapshot\_YYYY-MM-DD\_HH-MM.zip_ que vai variar dependendo da data e hora do _release_ (quando o software é liberado) pelos autores do projeto.
2. Ao descompactar o arquivo .zip, execute o arquivo _x96dbg.exe_ dentro do diretório _release_. Esse nome deve-se ao fato de que o x64dbg tem suporte tanto a 64 quanto a 32-bits, então o autor resolveu somar 32+64 e nomear o binário assim.
3. O _x96dbg.exe_ é o _launcher_ do x64dbg e tem três botões. Escolha **Install** e responda "Sim" para todas as perguntas.
4. À esta altura você já deve ter o atalho x64dbg na sua área de trabalho. Ao dar um duplo clique nele, você verá a tela inicial do _debugger_. A ideia é que você depure binários (.exe, .dll, etc) portanto, vamos abrir o AnalyseMe-00.exe e seguir.

## Configuração

Se o x64 estiver em português, mude o idioma para inglês. Vários termos em inglês não estão traduzidos e isso pode dificultar o aprendizado. Para mudar o idioma, vá em **Opções -\> Idiomas** e escolha **[en\_US] American English - United States**. Você precisará fechar o x64dbg e abri-lo novamente para carregar o novo idioma entrar em vigor. Depois que o software estiver em inglês, siga com a configuração:

1. Vá em **Options -\> Preferences** e, na aba **Events**, desmarque a caixa **System Breakpoint**. Isso vai fazer com que o _debugger_ pare direto no _entrypoint_ de um programa ao abrirmos.
2. Clique na aba **Engine** e marque a caixa **Disable ASLR**, que desabilita a randomização de endereços de memória.
3. Clique em **Save**.

Existem muitas outras opções de configuração que você pode experimentar, mas para o momento isso basta.

## Tela Inicial

Descompacte e abra o `AnalyseMe-00.exe` no x64dbg clicando em **File -\> Open**. Você deverá ver uma tela como esta:

![AnalyseMe-00.exe aberto no x32dbg][image-1]

A aba **CPU** é sem dúvida a mais utilizada no processo de _debugging_, por isso, fizemos questão de nomear algumas de suas áreas, que descreveremos agora.

### Disassembly

Nesta região são exibidos os endereços (VAs), _opcodes_ e argumentos de cada instrução, seu _disassembly_ (ou seja, o que significam em Assembly) e alguns comentários na quarta coluna, que podem ser automáticos (gerados pelo x64dbg ou por plugins) ou inseridos por você. No endereço inicial, por exemplo, há as  palavras _ OptionalHeader.AddressOfEntryPoint_ na quarta coluna, que nos diz que aquela instrução é o  _entrypoint_ do programa.

### Helper

Tomei a liberdade de nomear essa seção de **Helper**, porque de fato ela ajuda. Por exemplo, quando alguma instrução faz referência a um dado em memória ou em um registrador, ela já mostra que dado é este. Assim você não precisa ir buscar. É basicamente um economizador de tempo. Supondo que o _debugger_ esteja parado numa instrução que esteja lendo de `[rsp+20]`, no Helper aparecerá o valor que está na posição de memória RSP+20, assim você não precisa ir até lá manualmente para ver o valor.

### Dump

O dump é um visualizador que você pode usar para inspecionar _bytes_ em qualquer endereço. Por exemplo, você pode ir até o endereço RSP+20 e ver o que tem lá.

Há cinco abas de dump, onde cada uma pode mostrar o conteúdo de uma região de memória diferente para o mesmo alvo. Há ainda as abas **Watch**, **Locals** e **Struct**, que fogem do escopo deste livro, mas também estão ferramentas de inspeção.

### Registradores

Como o nome sugere, nesta região são mostrados os valores de cada registrador do processador, incluindo o do registrador de _flags_. Na verdade, o x64dbg vai um pouco além e mostra também variáveis globais úteis como `LastError` e `LastStatus`, ambas modificadas por chamadas à algumas funções da API do Windows.

### Convenção de Chamada

Nesta janela é possível configurar a convenção de chamada com a qual estamos trabalhando e o número de argumentos que você quer ver neste janela em cada chamada de função.

### Pilha

Mostra a pilha de memória, onde o endereço com fundo em preto indica o topo da pilha, ou seja, o endereço que está em RSP.

Na próxima seção, iremos depurar o binário de exemplo e devemos nos atentar às informações exibidas em cada uma das regiões da tela do _debugger_ acima apresentadas.


[image-1]:	../.gitbook/assets/x32dbg_01.png