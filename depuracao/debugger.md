# O debugger

## Instalação

1. Numa máquina virtual com o Windows (aqui utilizamos o Windows 7 mas qualquer um superior a este serve), baixe o [snapshot](https://sourceforge.net/projects/x64dbg/files/snapshots/) mais recente do x64dbg. É um arquivo .zip chamado _snapshot\_YYYY-MM-DD\_HH-MM.zip_ que vai variar dependendo da data e hora do _release_ (quando o software é liberado) pelo autor do projeto.

2. Ao descompactar o arquivo .zip, execute o arquivo _x96dbg.exe_ dentro do diretório _release_. Esse nome deve-se ao fato de que o x64dbg tem suporte tanto a 32 quanto a 64-bits, então o autor resolveu somar 32+64 e nomear o binário assim. :)

3. O _x96dbg.exe_ é o _launcher_ do x64dbg e tem três botões. Escolha **Setup** e responda "Sim" para todas as perguntas.

4. À esta altura você já deve ter o atalho x32dbg na área de trabalho. Ao clicar, você verá a tela inicial do _debugger_. A ideia é que você depure binários (.exe, .dll, etc) portanto, vamos abrir o AnalyseMe-00.exe e seguir.

## Configuração

1. Vá em **Options -> Settings** e desmarque a caixa **System breakpoint**. Isso vai fazer com que o _debugger_ pare direto no _entrypoint_ de um programa ao abrirmos. Clique em **Save**.

Existem muitas outras opções de configuração que você pode experimentar, mas para o momento esta basta.

## Tela inicial

Se ainda não o fez, faça download do nosso binário de exemplo, o [AnalyseMe-00.exe](https://www.mentebinaria.com.br/applications/core/interface/file/attachment.php?id=557) e abra-o no x32dbg clicando em **File -> Open**. Você deverá ver uma tela como esta:

![AnalyseMe-00.exe aberto no x32dbg](../.gitbook/assets/x32dbg_01.png)

A aba **CPU** é sem dúvida a mais utilizada no processo de _debugging_, por isso fizemos questão de nomear algumas de suas áreas, que descreveremos agora.

### Disassembly

Nesta região são exibidos os endereços (VA's), os _opcodes_ e argumentos em _bytes_ de cada instrução, seu _disassembly_ (ou seja, o que significam em Assembly) e alguns comentários úteis na quarta coluna, como a palavra _EntryPoint_, na primeira instrução do programa a ser executada (em 40104D no nosso exemplo).

### Helper

Tomei a liberdade de nomear essa seção de **Helper**, porque de fato ela ajuda. Por exemplo, quando alguma instrução faz referência à um dado em memória ou em um registrador, ela já mostra que dado é este. Assim você não precisa ir buscar. É basicamente um economizador de tempo. Supondo que o _debugger_ esteja parado na instrução ```push esi```, no Helper aparecerá o valor do registrador ESI.

### Dump

O dump é um visualizador de que você pode usar para inspecionar _bytes_ em qualquer endereço. Por padrão há cinco abas de dump, mas você pode adicionar mais se precisar.

### Registradores

Como o nome sugere, mostra o valor de cada registrador do processador.

### Pilha

Mostra a pilha de memória, onde o endereço com fundo em preto indica o topo da pilha.

Na próxima seção, iremos depurar o binário de exemplo e devemos nos atentar às informações exibidas em cada uma das regiões da tela do debugger, acima apresentadas. Preparado? :)