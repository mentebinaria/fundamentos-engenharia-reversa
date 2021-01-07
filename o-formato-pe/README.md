# O formato PE

Como explicado no capítulo anterior, a maioria dos tipos de arquivo que trabalhamos possuem uma especificação. Com os arquivos executáveis no Windows não é diferente: eles seguem a especificação do formato PE \(_Portable Executable_\) que conheceremos agora.

O formato PE é o formato de arquivo executável atualmente utilizado para os programas no Windows, isso inclui os famosos arquivos EXE mas também DLL, OCX, CPL e SYS. Seu nome deriva do fato de que o formato não está preso a uma arquitetura de _hardware_ específica.

Os programas que criam estes programas, chamados compiladores precisam respeitar tal formato e o programa que os interpreta, carrega e inicia sua execução \(chamado de _loader_\) precisa entendê-lo também.

{% hint style="success" %}
A documentação completa do formato PE é mantida pela própria Microsoft e está disponível [online](https://docs.microsoft.com/en-us/windows/win32/debug/pe-format). Uma versão em formato do Word esteve disponível por um tempo e a salvamos [aqui](https://www.mentebinaria.com.br/files/file/18-microsoft-portable-executable-and-common-object-file-format-specification/).
{% endhint %}

Assim como o formato GIF e outras especificações de formato de arquivo, o formato PE possui cabeçalhos, que possuem campos e valores possíveis. Outro conceito importante é o de seções.

A estrutura geral de um arquivo PE é apresentada na imagem abaixo:

![Estrutura de um arquivo PE](../.gitbook/assets/arquivo_pe.png)

Conheceremos agora os cabeçalhos mais importantes para este primeiro contato com a engenharia reversa e, em seguida, as seções de um arquivo PE.

