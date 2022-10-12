# Executáveis

Normalmente, chamamos de arquivos executáveis os arquivos que quando clicados duas vezes (no caso do Windows) executam. Os mais famosos no Windows são os de extensão `.exe`, que serão o foco do nosso estudo. Para entender como estes arquivos são criados, é preciso notar os passos:

1. Escrita do código-fonte na linguagem escolhida num arquivo de texto.
2. Compilação.
3. _Linking_.

O compilador cria os chamados arquivos **objeto** a partir do código-fonte (em texto). Estes objetos contém instruções de máquina e dados.

O _linker_ serve para **juntar** todos os objetos num único arquivo, **realocar** seus endereços e **resolver** seus símbolos (nomes de função importadas, por exemplo).

O processo de compilação (transformação do código-fonte em texto em código de máquina) gera como saída um arquivo chamado de **objeto**.

No que diz respeito ao processo de _linking_, estes executáveis podem ser de dois tipos:

## Estáticos

Todo o código das funções externas ao executável principal é compilado junto a ele. O resultado é um executável livre de dependências, porém grande. É raro encontrar executáveis assim para Windows.

## Dinâmicos

O executável vai **depender** de bibliotecas externas (DLL's, no caso do Windows) para funcionar, como estudamos na seção Tabela de Importações.

O nosso binário CRACKME.EXE, que usamos em vários momentos deste livro é dinâmico. Nós já vimos isso ao analisar sua _Imports Table_ com o DIE, mas agora vamos utilizar novamente a ferramenta dumpbin para checar suas dependências:

```
D:\>dumpbin /nologo /dependents CRACKME.EXE

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

Existem vários _parsers_ de executáveis alternativos ao **dumpbin**. Consulta o apêndice Ferramentas para ver uma lista.
