# Seções

As seções são divisões num binário PE. Uma analogia que torna o conceito de seções simples de entender é a de comparar o binário PE com uma cômoda: as seções seriam suas gavetas. Cada gaveta da cômoda, em teoria, guarda um tipo de dado distinto, e assim é com as seções, apesar de não ser uma regra muito rígida. Elas são necessárias porque diferentes conteúdos exigem diferentes tratamentos quando carregados em memória pelo Sistema Operacional.

Podemos então dizer que um binário PE é completamente definido por cabeçalhos e seções (com seu conteúdo), como na seguinte ilustração:

![Versão simplificada do arquivo PE](../.gitbook/assets/cabsec\_fig3.png)

Como dito, a principal separação que existe entre as seções é em relação a seu conteúdo, que distinguimos entre **código** ou **dados**. Apesar de terem seus nomes ignorados pelo _loader_ do Windows, convencionam-se alguns, normalmente iniciados por um ponto. As seções padrão importantes são discutidas a seguir:

## .text

Também nomeada **CODE** em programas compilados com o Delphi, esta seção contém o código executável do programa. Em seu cabeçalho normalmente encontramos as permissões de leitura e execução.

## .data

Também chamada de CODE em programas criados com Delphi, esta seção contém dados inicializados com permissão de leitura e escrita. Estes dados podem ser, por exemplo, uma C string declarada e já inicializada. Por exemplo, considere o programa abaixo:

```c
#include <stdio.h>

int main(void) {
    char s[] = "texto grande para o compilador utilizar a seção de dados";

    s[0] = 'T';
    puts(s);
    return 0;
}
```

A variável local **s** é um _array_ de _char_ e pode ser alterada a qualquer momento dentro da função _main()_. De fato, o código na linha 6 a altera. Sendo assim, é bem possível que um compilador coloque seu conteúdo numa seção de dados inicializados com permissão tanto para leitura quanto para escrita. ;-)

{% hint style="warning" %}
Apesar de fazer sentido, os compiladores não precisam respeitar tal lógica. O conteúdo da variável **s** no exemplo apresentado pode ser armazenado na seção **.rdata** (ou mesmo na **.text**) e ser manipulado na pilha de memória para sofrer alterações. Não há uma imposição por parte do formato e cada compilador escolhe fazer do seu jeito.
{% endhint %}

## .rdata

Seção que contém dados inicializados, com permissão somente para leitura. Um bom exemplo seria com o programa abaixo:

```c
int main(void) {
    const char s[] = "texto grande para o compilador utilizar a seção de dados";

    puts(s);
    return 0;
}
```

Neste caso declaramos a variável **s** como **const**, o que instrui o compilador a armazená-la numa região de memória somente para leitura, casando perfeitamente com a descrição da seção **.rdata**. ;-)

## .idata

Seção para abrigar as tabelas de _imports_, comum em todos os binários que importam funções de outras bibliotecas. Possui permissão tanto para leitura quanto para gravação. Entenderemos o motivo em breve.

## Alinhamento de Seções

O sistema operacional divide a memória RAM em páginas, normalmente de 4 _kilobytes_ (ou 4096 _bytes_) nas versões atuais do Windows. Nestas páginas de memória o sistema configura as permissões (leitura, escrita e execução). Os arquivos executáveis precisam ser carregados na memória e cada seção pode requerer permissões diferentes. Com isso em mente, considere a seguinte situação hipotética:

* Um executável tem seus cabeçalhos ocupando 2 KB.
* Sua seção .text tem 6 KB de tamanho e requer leitura e execução.
* Sua seção .data tem 5 KB de tamanho e requer leitura e escrita.
* O tamanho final do executável em disco é 13 KB.

Para mapear este executável em memória e rodá-lo, o SO precisa copiar o conteúdo de suas seções em páginas de memória e configurar suas permissões de acordo. Analise agora a figura abaixo:

![Mapeamento de seções em memória](<../.gitbook/assets/alinhamento (1).png>)

Perceba na figura que a seção .text já ocuparia duas páginas que precisariam ter permissões de leitura e execução. No que sobrou da segunda página, o SO não pode mapear a .data pois esta, apesar de compartilhar a permissão de leitura, exige escrita ao invés de execução. Logo, ele precisa mapeá-la na próxima página.

Como consequência, o tamanho total de cada seção em memória é maior que seu tamanho em disco, devido ao que chamamos de **alinhamento de seção**. No cabeçalho opcional existe o campo **SectionAlignment**, que pulei propositalmente. Este campo define qual fator de alinhamento deve ser utilizado para todas as seções do binário quando mapeadas em memória. O padrão é o valor do tamanho da página de memória do sistema.

Como bônus por ter chegado até aqui, segue um código que, depois de compilado e executado, vai dizer qual o tamanho da página de memória na sua versão do Windows.

```c
#include <stdio.h>
#include <windows.h>

int main(void) {
    SYSTEM_INFO info;

    GetNativeSystemInfo(&info);    
    printf("dwPageSize: %u\n", info.dwPageSize);
}
```
