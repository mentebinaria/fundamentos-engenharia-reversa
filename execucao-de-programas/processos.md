# Processos

Processo é um objeto que representa uma instância de um executável rodando. No Windows, processos não rodam. Quem roda mesmo são as _threads_ de um processo.

Um processo possui um PID \(**P**rocess **ID**entificator\), uma tabela de _handles_ abertos \(será explicado no capítulo sobre a Windows API\), um espaço de endereçamento virtual, e outras informações associadas a ele.

Para ver os processos ativos no seu sistema Windows neste momento, você pode usar o Gerenciador de Tarefas (experimente apertar Ctrl+Shift+ESC) ou o comando **tasklist**:

```
C:\>tasklist

Image Name                     PID Session Name        Session#    Mem Usage
========================= ======== ================ =========== ============
System Idle Process              0 Services                   0          8 K
System                           4 Services                   0      1,888 K
Secure System                  188 Services                   0    273,300 K
Registry                       232 Services                   0     37,224 K
smss.exe                      1020 Services                   0      1,632 K
csrss.exe                     1292 Services                   0      7,452 K
wininit.exe                   1396 Services                   0      9,364 K
services.exe                  1472 Services                   0     12,892 K
LsaIso.exe                    1492 Services                   0      4,676 K
lsass.exe                     1500 Services                   0     41,256 K
svchost.exe                   1724 Services                   0     44,368 K
WUDFHost.exe                  1756 Services                   0      8,504 K
fontdrvhost.exe               1776 Services                   0      5,816 K
svchost.exe                   1888 Services                   0     20,828 K
svchost.exe                   1956 Services                   0     15,724 K
svchost.exe                   1320 Services                   0      6,924 K
svchost.exe                   1184 Services                   0     15,944 K
svchost.exe                   2108 Services                   0     13,308 K
svchost.exe                   2116 Services                   0     15,300 K
-- suprimido --
```

Na saída do comando **tasklist**, a coluna **Image Name** mostra o nome do arquivo executável \(o arquivo no disco\) associado ao processo. Perceba que há vários processos do `svchost.exe` por exemplo. É normal.

Há muito mais para falar sobre processos, mas para nosso objetivo aqui, saber que eles representam um programa em execução é suficiente. Agora vamos entender como os programas fazem uso da API que o Windows oferece para que ações significativas ocorram no sistema.
