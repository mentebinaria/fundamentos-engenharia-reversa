# Executáveis

Normalmente chamamos de arquivos executáveis os arquivos que quando clicados duas vezes (no caso do Windows) executam. Os mais famosos no Windows são os de extensão `.exe`, que são o foco do nosso estudo. Para entender como estes arquivos são criados, é preciso notar os seguintes passos:

1. Escrita do código-fonte na linguagem escolhida num arquivo de texto.
2. Compilação.
3. Linkedição (_Linking_).

O compilador cria os chamados arquivos **objeto** a partir do código-fonte. Estes objetos contém instruções de máquina e dados.

O _linker_ serve para **juntar** todos os objetos num único arquivo, **realocar** seus endereços e **resolver** seus símbolos (nomes de função importadas, por exemplo).

O processo de compilação é a transformação do código-fonte em texto em código de máquina. Como saída, ele um arquivo chamado de **objeto**.

No que diz respeito ao processo de _linking_, estes executáveis podem ser de dois tipos:

## Estáticos

Todo o código das funções externas ao executável principal é compilado junto a ele. O resultado é um executável livre de dependências, porém grande. É raro encontrar executáveis assim para Windows.

## Dinâmicos

O executável vai **depender** de bibliotecas externas (DLLs, no caso do Windows) para funcionar e fará uso da _Import Table_ conforme estudamos no capítulo anterior.

Como exemplo, vamos checar as dependências do binário da calculadora:

```
dumpbin /nologo /dependents c:\windows\system32\calc.exe

Dump of file c:\windows\system32\calc.exe

File Type: EXECUTABLE IMAGE

  Image has the following dependencies:

    SHELL32.dll
    KERNEL32.dll
    msvcrt.dll
    ADVAPI32.dll
    api-ms-win-core-synch-l1-2-0.dll
    api-ms-win-core-processthreads-l1-1-0.dll
    api-ms-win-core-libraryloader-l1-2-0.dll
```

Mas e as DLLs, como "rodam"? Vejamos agora.

