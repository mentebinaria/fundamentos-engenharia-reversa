# Seções

As seções são divisões num binário PE. Uma analogia que torna o conceito de seções simples de entender é a de comparar o binário PE com uma cômoda: as seções seriam suas gavetas. Cada gaveta da cômoda, em teoria, guarda um conteúdo diferente. Assim é com as seções.

Elas são necessárias porque diferentes tipos de conteúdos exigem diferentes tratamentos quando carregados em memória pelo sistema operacional.

Podemos então dizer que um binário PE é completamente definido por cabeçalhos e seções (com seu conteúdo), como na seguinte ilustração:

![Versão simplificada do arquivo PE](../.gitbook/assets/cabsec_fig3.png)

Como dito, a principal separação que existe entre as seções é em relação a seu conteúdo, que distinguimos entre **código** ou **dados**. Apesar de terem seus nomes ignorados pelo _loader_ do Windows, convencionam-se alguns nomes de seção normalmente encontrados em executáveis:

## .text

Também nomeada **CODE** em programas compilados em Delphi, esta seção contém o código executável do programa. Em seu cabeçalho normalmente encontramos as permissões de leitura e execução em memória.

## .data

Também chamada de DATA em programas criados com Delphi, esta seção contém dados inicializados com permissão de leitura e escrita. Estes dados podem ser, por exemplo, uma _string_ declarada e já inicializada. Considere o programa abaixo:

```c
#include <stdio.h>

int main(void) {
    char s[] = "texto grande para forçar o compilador a utilizar a seção de dados";
    s[0] = 'T';
    puts(s);
}
```

A variável local **s** é um _array_ de _char_ e pode ser alterada a qualquer momento dentro da função _main()_. De fato, logo após a sua declaração ela é alterada e logo depois acessada/lida pela função `puts()`. Em sua configuração padrão, o compilador coloca essa _string_ numa seção de dados inicializados com permissão tanto para leitura quanto para escrita.

> Apesar de fazer sentido, os compiladores não precisam respeitar tal lógica. O conteúdo da variável **s** no exemplo apresentado pode ser armazenado na seção **.rdata** (ou mesmo na **.text**) e ser manipulado na pilha de memória para sofrer alterações. Não há uma imposição por parte do formato e cada compilador escolhe como fazer.

## .rdata

Seção que contém dados inicializados com permissão somente para leitura. Um bom exemplo seria com o programa abaixo:

```c
int main(void) {
    const char s[] = "texto grande para o compilador utilizar a seção de dados";
    puts(s);
}
```

Neste caso declaramos a variável **s** como **const**, o que instrui o compilador a armazená-la numa região de memória somente para leitura, casando perfeitamente com a descrição da seção **.rdata**.

## .idata

Seção para abrigar as tabelas de _imports_, comum em todos os binários que importam funções de outras bibliotecas. Possui permissão tanto para leitura quanto para escrita. Entenderemos o motivo dessas permissões em breve.

## Alinhamento de Seções

O sistema operacional divide a memória RAM em páginas, normalmente de 4 _kilobytes_ (ou 4096 _bytes_) nas versões atuais do Windows. Nestas páginas de memória o sistema configura as permissões de leitura, escrita e execução. Os arquivos executáveis precisam ser carregados na memória e cada seção pode requerer permissões diferentes. Com isso em mente, considere a seguinte situação hipotética:

* Um executável tem seus cabeçalhos ocupando 2 KB.
* Sua seção .text tem 6 KB de tamanho e requer leitura e execução.
* Sua seção .data tem 5 KB de tamanho e requer leitura e escrita.
* O tamanho final do executável em disco é 13 KB.

Para mapear este executável em memória e rodá-lo, o SO precisa copiar o conteúdo de suas seções em páginas de memória e configurar suas permissões de acordo. Analise agora a figura abaixo:

![Mapeamento de seções em memória](<../.gitbook/assets/alinhamento.png>)

Perceba na figura que a seção .text já ocuparia duas páginas que precisariam ter permissões de leitura e execução. No que sobrou da segunda página, o SO não pode mapear a .data pois esta, apesar de compartilhar a permissão de leitura, exige escrita ao invés de execução. Logo, ele precisa mapeá-la numa próxima página.

Como consequência, o tamanho total de cada seção em memória é maior que seu tamanho em disco, devido ao que chamamos de **alinhamento de seção**. No cabeçalho opcional existe o campo **SectionAlignment**, que pulei propositalmente. Este campo define qual fator de alinhamento deve ser utilizado para todas as seções do binário quando mapeadas em memória. O padrão é o valor do tamanho da página de memória do sistema.

Como bônus por ter chegado até aqui, segue um código que, depois de compilado e executado, vai te dizer qual o tamanho da página de memória na sua versão do Windows.

```c
#include <stdio.h>
#include <windows.h>

int main(void) {
    SYSTEM_INFO info;

    GetNativeSystemInfo(&info);    
    printf("dwPageSize: %u\n", info.dwPageSize);
}
```

Já sabemos como é a estrutura de um arquivo PE, mas precisamos voltar um pouquinho no assunto de diretórios para falar da _Import Table_, que veremos agora.
