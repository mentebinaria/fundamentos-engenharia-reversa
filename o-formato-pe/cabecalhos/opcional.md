# Opcional

Não parece, mas este cabeçalho é muito importante. Seu nome deve-se ao fato de que ele é opcional para arquivos objeto, mas é requerido em arquivos executáveis, nosso foco de estudo. Seu tamanho não é fixo. Ao contrário, é definido pelo campo _SizeOfOptionalHeader_ do cabeçalho COFF, que vimos anteriormente. Sua estrutura para arquivos PE de 32-bits, também chamados de PE32, é a seguinte:

```c
typedef struct {
    uint16_t Magic;
    uint8_t MajorLinkerVersion;
    uint8_t MinorLinkerVersion;
    uint32_t SizeOfCode;
    uint32_t SizeOfInitializedData;
    uint32_t SizeOfUninitializedData;
    uint32_t AddressOfEntryPoint;
    uint32_t BaseOfCode;
    uint32_t BaseOfData;
    uint32_t ImageBase;
    uint32_t SectionAlignment;
    uint32_t FileAlignment;
    uint16_t MajorOperatingSystemVersion;
    uint16_t MinorOperatingSystemVersion;
    uint16_t MajorImageVersion;
    uint16_t MinorImageVersion;
    uint16_t MajorSubsystemVersion;
    uint16_t MinorSubsystemVersion;
    uint32_t Reserved1;
    uint32_t SizeOfImage;
    uint32_t SizeOfHeaders;
    uint32_t CheckSum;
    uint16_t Subsystem;
    uint16_t DllCharacteristics;
    uint32_t SizeOfStackReserve;
    uint32_t SizeOfStackCommit;
    uint32_t SizeOfHeapReserve;
    uint32_t SizeOfHeapCommit;
    uint32_t LoaderFlags;
    uint32_t NumberOfRvaAndSizes;
    IMAGE_DATA_DIRECTORY DataDirectory[MAX_DIRECTORIES];
} IMAGE_OPTIONAL_HEADER_32;
```

Vamos analisar agora os campos mais importantes no nosso estudo:

## Magic

O primeiro campo, de 2 bytes, é um outro número mágico que identifica o tipo de executável em questão. O valor 0x10b significa que o executável é um PE32 \(executável PE de 32-bits\), enquanto o valor 0x20b diz que é um PE32+ \(executável PE de 64-bits\).

{% hint style="danger" %}
Perceba que a Microsoft chama os executáveis de PE de 64-bits de PE32+ e não de PE64.
{% endhint %}

## AddressOfEntryPoint

Este é talvez o campo mais importante do cabeçalho opcional. Nele está contido o endereço do ponto de entrada \(_entrypoint_\), abreviado EP, que é onde o código do programa deve começar. Para arquivos executáveis este endereço é relativo à base da imagem \(campo que veremos a seguir\). Para bibliotecas, ele não é necessário e pode ser zero, já que as funções de bilbioteca podem ser chamadas arbitrariamente.

## ImageBase

Imagem é como a Microsoft chama um arquivo executável \(para diferenciar de um código-objeto\) quando vai para a memória. Neste campo está o endereço de memória que é a base da imagem, ou seja, onde o programa será carregado em memória. Para arquivos executáveis \(.EXE\) o padrão é 0x400000. Já para bibliotecas \(.DLL\), o padrão é 0x10000000, embora executáveis do Windows como o _calc.exe_ também apresentem este valor no _ImageBase_.

## SubSystem

Este campo define o tipo de subsistema necessário para rodar o programa. Valores interessantes para nós são:

* 0x002 - Windows GUI \(_Graphical User Interface_\) - para programas gráficos no Windows \(que usam janelas, etc\).
* 0x003 - Windows CUI \(_Character User Interface_\) - para programas de linha de comando.

## DllCharacteristics

Ao contrário do que possa parecer, este campo não é somente para DLL's. Ele está presente e é utilizado para arquivos executáveis também. Assim como o campo _Characteristics_ do cabeçalho COFF visto anteriormente, este campo é uma máscara de _bits_ com destaque para os possíveis valores:

| Bit | Nome |
| :--- | :--- |
| 6 | IMAGE\_DLLCHARACTERISTICS\_DYNAMIC\_BASE |
| 8 | IMAGE\_DLLCHARACTERISTICS\_NX\_COMPAT |

O estado _bit_ 6 nos diz se a randomização de endereços de memória, também conhecida por ASLR \(_Address Space Layout Randomization_\), está ativada para este binário, enquanto o estado do _bit_ 8 diz respeito ao DEP \(_Data Execution Prevention_\), também conhecido pela sigla NX \(_No eXecute_\). O estudo aprofundado destes recursos foge do escopo inicial deste livro, mas é importante que saibamos que podemos desabilitar tais recursos simplesmente forçando estes _bits_ para zero.

## Diretórios de dados

Ainda como parte do cabeçalho opcional, temos os diretórios de dados, ou _Data Directories_. São 16 diretórios ao todo, cada um com uma função. Concentraremos, no entanto, nos mais importantes para este estudo inicial. A estrutura de cada diretório de dados é conhecida por [IMAGE\_DATA\_DIRECTORY](https://msdn.microsoft.com/en-us/library/windows/desktop/ms680305%28v=vs.85%29.aspx) e tem a seguinte definição:

```text
typedef struct _IMAGE_DATA_DIRECTORY {
    uint32_t   VirtualAddress;
    uint32_t   Size;
} IMAGE_DATA_DIRECTORY;
```

Vejamos agora alguns diretórios:

## Export Table

O primeiro diretório de dados aponta para a tabela de _**exports**_, ou seja, de funções exportadas pela aplicação. Por isso mesmo sua presença \(campos _VirtualAddress_ e _Size_ diferentes de zero\) é muito comum em bibliotecas.

O campo _VirtualAddress_ aponta para uma outra estrutura chamada EDT \(_Export Directory Table_\), que contém os nomes das funções exportadas e seus endereços, além de um ponteiro para uma outra estrutura, preenchida em memória, chamada de EAT \(_Export Address Table_\). Entenderemos mais sobre estas e outras estruturas em breve.

## Import Table

Sendo a contraparte da _Export Table_, a _Import Table_ aponta para a tabela de _**imports**_, ou seja, de funções importadas pela aplicação. O campo _VirtualAddress_ aponta para a IDT \(_Import Directory Table_\), que tem um ponteiro para a IAT \(_Import Address Table_\), que estudaremos mais à frente.

## Resource Table

Aponta para uma estrutura de árvore binária que armazena todos os _resources_ num executável \(ícones, janelas, strings - principalmente quando o programa suporta vários idiomas\), etc. Também estudaremos um pouco mais sobre estes "recursos" no futuro.

