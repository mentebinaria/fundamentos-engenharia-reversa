# 💼 O formato PE

Como explicado no capítulo anterior, a maioria dos tipos de arquivo que trabalhamos possui uma especificação. Com os arquivos executáveis no Windows não é diferente: eles seguem a especificação do formato PE (_Portable Executable_) que conheceremos agora.

O formato PE é o formato de arquivo executável atualmente utilizado para muitos programas no Windows, isso inclui os famosos arquivos EXE mas também arquivos DLL, OCX, CPL e SYS. Seu nome deriva do fato de o formato não estar preso à uma arquitetura de _hardware_ específica.

Os programas que criam estes programas, chamados compiladores precisam respeitar tal formato e o programa que os interpreta, carrega e inicia sua execução (chamado de _loader_) precisa entendê-lo também.

> A documentação completa do formato PE é mantida pela própria Microsoft e está disponível em [https://learn.microsoft.com/en-us/windows/win32/debug/pe-format](https://learn.microsoft.com/en-us/windows/win32/debug/pe-format).

Assim como o formato GIF e outras especificações de formato de arquivo, o formato PE possui cabeçalhos, que possuem campos e valores possíveis. Outro conceito importante é o de seções.

De forma geral, um arquivo PE pode ser dividido em duas partes: cabeçalhos e seções. Conheceremos agora os cabeçalhos mais importantes para este primeiro contato com a engenharia reversa e, em seguida, as seções de um arquivo PE.
