# Cabeçalhos das seções

Após o cabeçalho opcional, encontramos os cabeçalhos das seções \(estas serão explicadas no próximo capítulo\). Neste cabeçalho há um _array_ de estruturas como a seguir:

```c
typedef struct {
	uint8_t Name[SECTION_NAME_SIZE];
	uint32_t VirtualSize;
	uint32_t VirtualAddress;
	uint32_t SizeOfRawData;
	uint32_t PointerToRawData;
	uint32_t PointerToRelocations;
	uint32_t PointerToLinenumbers; // descontinuado
	uint16_t NumberOfRelocations;
	uint16_t NumberOfLinenumbers;  // descontinuado
	uint32_t Characteristics;
} IMAGE_SECTION_HEADER;
```

Cada estrutura define uma seção no executável e a quantidade de estrutura \(quantidade de elementos neste _array_\) é igual ao número de seções no executável, definido no campo _NumberOfSections_ do cabeçalho COFF. Vamos aos campos importantes:

### **Name**

Este campo define o nome da seção. Como é um _array_ de 8 elementos do tipo **uint8\_t**, este nome está limitado à 8 caracteres. A string **.text** por exemplo ocupa apenas 5 _bytes_, então os outros 3 devem estar zerados. Há suporte à UTF-8 para estes nomes.

### **VirtualSize**

O tamanho em _bytes_ da seção depois de ser mapeada \(carregada\) em memória pelo _loader_. Se este valor for maior que o valor do campo **SizeOfRawData**, os _bytes_ restantes são preenchidos com zeros.

#### **VirtualAddress**

O endereço relativo à base da imagem \(campo **ImageBase** do cabeçalho Opcional\) quando a seção é carregada em memória. Por exemplo, se para uma seção este valor é 0x1000 e o valor de **ImageBase** é 0x400000, quando carregada em memória esta seção estará no endereço 0x401000. Para chegar nesta conclusão basta somar os dois valores.

### **SizeOfRawData**

Tamanho em _bytes_ da seção no arquivo PE, ou seja, antes de ser mapeada em memória. Alguns autores também usam a expressão "tamanho em disco" ou simplesmente "tamanho da seção".

### **PointerToRawData**

O _offset_ em disco da seção no arquivo. É correto dizer que aponta para o primeiro _byte_ da seção. Por exemplo, se para dada seção este valor é 0x400 e o valor do campo **SizeOfRawData** é 0x1800, para ver somente seu conteúdo em hexadecimal com o **hexdump** poderíamos fazer:

```text
$ hd -s 0x400 -n 0x1800 arquivo.exe > secao.txt
```

### **Characteristics**

Este é um campo define algumas _flags_ para a seção, além de as permissões em memória que ela deve ter quando for mapeada pelo _loader_. Ele possui 32-bits, onde alguns significam conforme a tabela a seguir:

| Bit | Nome da flag | Descrição |
| :--- | :--- | :--- |
| 5 | IMAGE\_SCN\_CNT\_CODE | A seção contém código executável |
| 6 | IMAGE\_SCN\_CNT\_INITIALIZED\_DATA | A seção contém dados inicializados |
| 7 | IMAGE\_SCN\_CNT\_UNINITIALIZED\_ DATA | A seção contém dados não inicializados |
| 29 | IMAGE\_SCN\_MEM\_EXECUTE | Terá permissão de execução |
| 30 | IMAGE\_SCN\_MEM\_READ | Terá permissão de leitura |
| 31 | IMAGE\_SCN\_MEM\_WRITE | Terá permissão de escrita |

{% hint style="info" %}
As _flags_ que contém o texto "MEM" no nome dizem respeito às permissões que a seção terá quando mapeada em memória. De acordo com elas o SO vai _setar_ as permissões nas páginas de memória nas quais a seção é carregada.
{% endhint %}

É importante notar que campos como o **Characteristics** são o que chamamos de máscaras de _bits_. Por exemplo, a tabela anterior diz que se o _bit_ 30 deste campo está _setado_ \(seu valor é 1\), então esta seção terá permissão de leitura quando em memória. O valor de campo **Characteristics** seria então 0**1**000000000000000000000000000000 em binário, mas você provavelmente vai encontrar este valor representado em hexadecimal \(0x80000000\) nos analisadores de executáveis que for utilizar. Aliás, agora é uma boa hora para abrir o **DIE** e analisar alguns arquivos executáveis, colocando em prática tudo o que foi visto até aqui. ;-\)
