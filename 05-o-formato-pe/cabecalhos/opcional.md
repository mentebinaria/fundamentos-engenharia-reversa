# Opcional

A julgar pelo seu nome, pode não parecer, mas este cabeçalho é muito importante. Ele é opcional para arquivos objeto, mas é obrigatório em arquivos executáveis, que são o nosso foco de estudo. Ao contrário dos outros cabeçalhos que vimos até agora, o tamanho deste cabeçalho não é fixo, mas sim definido pelo campo _SizeOfOptionalHeader_ do cabeçalho COFF, que vimos anteriormente. Sua estrutura para arquivos PE de 64-bits, também chamados de PE32+ (ou de PE64 de forma não oficial), é a seguinte:

```c
#define MAX_DIRECTORIES 16

typedef struct {
	uint16_t Magic;
	uint8_t MajorLinkerVersion;
	uint8_t MinorLinkerVersion;
	uint32_t SizeOfCode;
	uint32_t SizeOfInitializedData;
	uint32_t SizeOfUninitializedData;
	uint32_t AddressOfEntryPoint;
	uint32_t BaseOfCode;
	uint64_t ImageBase;
	uint32_t SectionAlignment;
	uint32_t FileAlignment;
	uint16_t MajorOperatingSystemVersion;
	uint16_t MinorOperatingSystemVersion;
	uint16_t MajorImageVersion;
	uint16_t MinorImageVersion;
	uint16_t MajorSubsystemVersion;
	uint16_t MinorSubsystemVersion;
	uint32_t Win32VersionValue;
	uint32_t SizeOfImage;
	uint32_t SizeOfHeaders;
	uint32_t CheckSum;
	uint16_t Subsystem;
	uint16_t DllCharacteristics;
	uint64_t SizeOfStackReserve;
	uint64_t SizeOfStackCommit;
	uint64_t SizeOfHeapReserve;
	uint64_t SizeOfHeapCommit;
	uint32_t LoaderFlags;
	uint32_t NumberOfRvaAndSizes;
    IMAGE_DATA_DIRECTORY DataDirectory[MAX_DIRECTORIES];
} IMAGE_OPTIONAL_HEADER_64;
```

Vamos analisar agora os campos mais importantes para o nosso estudo:

## Magic

O primeiro campo, de 2 bytes, é um outro número mágico que identifica o tipo de executável em questão. O valor 0x20b diz que é um PE32+ (executável PE de 64-bits), enquanto o valor 0x10b significa que o executável é um PE32 (executável PE de 32-bits).

{% hint style="info" %}
A Microsoft chama os executáveis de PE de 64-bits de PE32+ e não de PE64 como alguns programas fazem. Já os de 32-bits são chamados de PE32 mesmo.
{% endhint %}

## AddressOfEntryPoint

Este é talvez o campo mais importante do cabeçalho opcional. Nele está contido o endereço do ponto de entrada (_entrypoint_), abreviado EP, que é onde o código do programa deve começar. Para arquivos executáveis, este endereço é relativo à base da imagem (campo que veremos a seguir). Para bibliotecas, ele não é necessário e pode ser zero, já que as funções de bilbioteca podem ser chamadas arbitrariamente.

## ImageBase

Imagem é como a Microsoft chama um arquivo executável (para diferenciar de um código-objeto) quando vai para a memória. Neste campo está o endereço de memória que é a base da imagem, ou seja, o endereço onde o programa será carregado em memória. É comum encontrar valores como 0x140000000 ou 0x400000 neste campo.

## SubSystem

Este campo define o tipo de subsistema necessário para rodar o programa. Valores interessantes para nós são:

* 0x002 - Windows GUI (_Graphical User Interface_) - para programas gráficos no Windows (que usam janelas, etc).
* 0x003 - Windows CUI (_Character User Interface_) - para programas de linha de comando.

## DllCharacteristics

Ao contrário do que o nome possa sugerir, este campo não é somente para DLLs. Ele está presente e é utilizado para arquivos executáveis também. Assim como o campo _Characteristics_ do cabeçalho COFF visto anteriormente, este campo é uma máscara de _bits_ com destaque para os possíveis valores:

| Bit | Nome                                        |
| --- | ------------------------------------------- |
| 5   | IMAGE\_DLLCHARACTERISTICS\_DYNAMIC\_BASE    |
| 7   | IMAGE\_DLLCHARACTERISTICS\_NX\_COMPAT       |

O estado _bit_ 5 nos diz se a randomização de endereços de memória, também conhecida por ASLR (_Address Space Layout Randomization_), está ativada para este binário, enquanto o estado do _bit_ 7 diz respeito à DEP (_Data Execution Prevention_), também conhecido pela sigla NX (_No eXecute_). O estudo aprofundado destes recursos foge do escopo deste livro, mas é importante que saibamos que podemos desabilitar tais recursos para um binário específico simplesmente desligando estes _bits_ se quisermos.

O último campo importante para nós é o DataDirectory, que veremos a seguir.