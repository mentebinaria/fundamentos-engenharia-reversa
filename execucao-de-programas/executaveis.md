# Executáveis

Normalmente, chamamos de arquivos executáveis os arquivo que, quando clicados duas vezes \(no caso do Windows\), executam. A extensão mais comum - e foco do nosso estudo aqui - são os arquivos .exe. Para entender como estes arquivos são criados, é preciso notar os passos:

1. Escrita do código-fonte na linguagem escolhida num arquivo de texto.
2. Compilação.
3. _Linking_.

O compilador cria os chamados arquivos **objeto** a partir do código-fonte \(em texto\). Estes objetos contém instruções de máquina e dados.

O _linker_ serve para **juntar** todos os objetos num único arquivo, **realocar** seus endereços e **resolver** seus símbolos \(nomes de função importadas, por exemplo\).

O processo de compilação \(transformação do código-fonte em texto em código de máquina\) gera como saída um arquivo chamado de **objeto**.

No que diz respeito ao processo de _linking_, estes executáveis podem assumir dois tipos:

### Estáticos

Todo o código das funções externas ao executável principal é compilado junto a ele. O resultado é um executável livre de dependências, porém grande.

### Dinâmicos

O executável vai **depender** de bibliotecas externas \(DLL's, no caso do Windows\) para funcionar, como estudamos na seção Tabela de Importações.

O nosso binário CRACKME.EXE, que usamos em vários momentos deste livro é dinâmico. Nós já vimos isso ao analisar sua _Imports Table_ com o DIE, mas agora vamos utilizar a ferramenta [dumpbin](https://docs.microsoft.com/en-us/cpp/build/reference/dumpbin-options), parte integrante do Visual Studio da Microsoft, para checar suas dependências:

```text
D:\>dumpbin /dependents CRACKME.EXE
Microsoft (R) COFF/PE Dumper Version 14.12.25830.2
Copyright (C) Microsoft Corporation.  All rights reserved.


Dump of file CRACKME.EXE

File Type: EXECUTABLE IMAGE

  Image has the following dependencies:

    USER32.dll
    KERNEL32.dll
    COMCTL32.DLL
    GDI32.dll
    COMDLG32.dll

  Summary

        1000 .edata
        1000 .idata
        1000 .reloc
        2000 .rsrc
        1000 CODE
        1000 DATA
```

O mesmo resultado pode ser atingido utilizando a ferramenta **readpe**. A vantagem desta sobre o **dumpbin** é que o **readpe** é multi-plataforma e livre. Para entender como ele funciona, assista ao vídeo:

{% embed url="https://www.youtube.com/watch?v=7EI\_wRk3VBU" %}



