# Diretórios de Dados

No final do cabeçalho opcional, mas ainda como parte dele, temos os diretórios de dados, ou _Data Directories_. São 16 diretórios ao todo, mas um executável pode conter somente alguns. Nos concentraremos, nos mais importantes para este estudo inicial. A estrutura de cada diretório de dados é conhecida por IMAGE\_DATA\_DIRECTORY e tem a seguinte definição:

```c
typedef struct _IMAGE_DATA_DIRECTORY {
    uint32_t   VirtualAddress;
    uint32_t   Size;
} IMAGE_DATA_DIRECTORY;
```

Vejamos alguns diretórios importantes neste momento:

## Export Table

O primeiro diretório de dados aponta para a tabela de _**exports**_, ou seja, de funções exportadas pela aplicação. Por este motivo, a presença deste diretório (campos _VirtualAddress_ e _Size_ com valores diferentes de zero) é muito comum em bibliotecas.

O campo _VirtualAddress_ aponta para uma outra estrutura chamada EDT (_Export Directory Table_), que contém os nomes das funções exportadas e seus endereços, além de um ponteiro para uma outra estrutura, preenchida em memória, chamada de EAT (_Export Address Table_).

## Import Table

Sendo a contraparte do _Export Table_, o diretório _Import Table_ aponta para uma tabela de _**imports**_, ou seja, de funções importadas pela aplicação. Tal tabela é chamada de IDT (_Import Directory Table_). Nós a estudaremos em mais à frente.

## Resource Table

Aponta para uma estrutura de árvore binária que armazena todos os _resources_ num executável como ícones, janelas e _strings_, principalmente quando o programa suporta vários idiomas.

## Certificate Table

Antigamente chamado de "Security", este diretório contém o endereço da _Certificate Table_, que pode conter um certificado para binários assinados digitalmente.

## IAT

Este diretório aponta para a **Import Address Table**, que veremos em breve.

## CLR Runtime Header

Para binários criados em .NET, há um outro cabeçalho específico que é apontado por este diretório.

A ordem na qual estes diretórios aparecem no arquivo PE é especificada na documentação do formato. Vamos agora para o nosso último cabeçalho, o das Seções.
