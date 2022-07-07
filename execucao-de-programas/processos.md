# Processos

Processo é um objeto que representa uma instância de um executável rodando. No Windows, processos não rodam. Quem roda mesmo são as _threads_ de um processo.

Um processo possui um PID \(Process IDentificator\), uma tabela de _handles_ abertos \(será explicado no capítulo sobre a Windows API\), um espaço de endereçamento virtual, e outras informações associadas a ele.

Para ver os processos ativos no seu sistema Windows neste momento, você pode usar o Gerenciador de Tarefas \(aba "Detalhes"\) ou o comando `tasklist`:

![Sa&#xED;da do comando tasklist do Windows](../.gitbook/assets/tasklist.png)

Na imagem anterior, a coluna **Image Name** define o nome do arquivo executável \(o arquivo no disco\) associado ao processo. Perceba que há vários processos do `svchost.exe` por exemplo. É normal.
