# Manipulação do fluxo de execução

Agora que já sabemos o básico do *debugging* e sabemos colocar *breakpoints* de software, vamos começar a manipular o programa da maneira que queremos.

Em geral, quando falamos de manipulação, falamos de alguma alteração no código do programa, para que este execute o que queremos, da forma como queremos.

Tomemos como exemplo o AnalyseMe-00 mesmo. Supondo que não sabemos o que o mesmo faz. Um bom início para a engenharia reversa é a busca por chamdas intermodulares (o botão com um celular e uma seta azul). Ao clicar nele, encontrará duas chamdas à função DeleteFileA da KERNEL32.DLL. Colocaremos um breakpoint em todas as chamadas à estas funções, bastando para isso dar um clique com o botão direito do mouse em uma delas e escolher a opção "Set breakpoint on all calls to DeleteFileA", como sugere a imagem abaixo:

![Chamadas intermodulares](../.gitbook/assets/manipulacao_intermodular_calls.png)

## Encontrando o local para manipulação

Ao voltar à aba CPU e rodar o programa (F9), paramos aqui:

![Função que chama a DeleteFileA](../.gitbook/assets/manipulacao_deletefilea.png)

Esta função é bem simples. No endereço 00401105 há um PUSH que coloca o endereço 402020 na pilha, depois há a chamada da DeleteFileA em si.

O x64dbg já resolve a referência do endereço e, caso encontre uma string, exibe ao lado (na quarta coluna), como acontece com a string "C:\Windows\System32\cmd.exe". Ora, se este é o argumento passado para a função DeleteFile(), este é o caminho do arquivo que o programa AnalyseMe-00 pretende deletar.

O que a gente vai fazer é mudar esta string, mudando assim o programa que o AnalyseMe-00 tenta deletar. Para isso, clique com botão direito do mouse sobre a instrução PUSH e escolha "Follow in Dump -> 402020".

![Seguindo o endereço da string no Dump](../.gitbook/assets/manipulacao_follow_in_dump.png)

O endereço em questão é exibido no Dump 1. Outra opção seria ir no Dump 1, teclar Ctrl+G, digitar 402020 e clicar em OK. 😉

## Alterando a string

Para alterar a string, você vai precisar selecionar todos os bytes desejados, pois o x64dbg não sabe exatamente onde começa e onde termina cada bloco de dados usado pelo programa. Supondo que queiramos alterar "cmd.exe" para "calc.exe", fazendo assim com que o programa tente excluir a calculadora do Windows. Para este caso, selecionamos o trecho e pressionamos Ctrl+E, que é o equivalente ao clicar com o botão direito sobre a seleção e escolher "Binary -> Edit".

![Editando os bytes da string string](../.gitbook/assets/manipulacao_edit_string.png)

Após fazer a alteração e clicar em OK, perceba que o Dump 1 agora destaca os bytes alterados em vermelho:

![Bytes alterados exibidos em vermelho no Dump](../.gitbook/assets/manipulacao_dump_alterado.png)

Ao seguir com a execução da chamada à DeleteFileA (F8), o programa tenta excluir o calc.exe ao invés de o cmd.exe. No entanto, como em versões modernas do Windows o conteúdo deste diretório é protegido, a função retorna zero (perceba o registrador EAX zerado), que no caso desta função, indica que houve [falha](https://docs.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-deletefilea#return-value), e as variáveis [LastError](https://docs.microsoft.com/en-us/windows/win32/debug/last-error-code) e LastStatus são modificadas para refletir o que aconteceu.

![Retorno zero em EAX e variáveis LastError e LastStatus em vermelho](../.gitbook/assets/manipulacao_lasterror.png)

Espero que com esta seção você entenda que, tendo o programa sob o controle de um debugger, é possível modificar praticamente tudo o que queremos. Podemos impedir que funções sejam chamadas, podemos chamar novas funções, alterar dados, modificar parâmetros, enfim, a lista é quase infinita. Na próxima seção vamos ver como salvar as alterações feitas.
