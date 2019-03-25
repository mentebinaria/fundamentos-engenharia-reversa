# Bibliotecas

As bibliotecas, ou DLL's no Windows, são também arquivos PE, mas sua intenção é ter suas funções utilizadas \(importadas e chamadas\) por arquivos executáveis. Elas também importam funções de outras bibliotecas, mas além disso, **exportam** funções para serem utilizadas.

Novamente, é possível utilizar o DIE para ver as funções importadas e exportadas por uma DLL, mas no exemplo a seguir, utilizamos novamente o **dumpbin** contra a biblioteca _SHELL32.DLL_, nativa do Windows:

```text
C:\>dumpbin /exports %windir%\system32\shell32.dll | findstr /i shellabout
        428  119 0027CC59 ShellAboutA
        429  11A 000CA129 ShellAboutW
```

Utilizamos o comando **findstr** do Windows para filtrar a saída por funções que criam caixas de mensagem. Este comando é como o **grep** no Linux. A sua opção **/i** faz com que o filtro de texto ignore o case \(ou seja, funciona tanto com letras maiúsculas quanto com minúsculas\).

Para chamar uma função desta DLL, teríamos que criar um executável que a **importe**. No entanto, o próprio Windows já oferece um utilitário chamado _rundll32.exe_, capaz de chamar funções de uma biblioteca. A maneira via linha de comando é como a seguir:

```text
C:\>rundll32.exe <DLL>,<Função> <Parâmetros>
```

 Como a função _ShellAboutA\(\)_ recebe um texto ASCII para ser exibido na tela "Sobre" do Windows, podemos testá-la da seguinte forma:

![](../.gitbook/assets/shellabouta.png)

{% hint style="danger" %}
Utilizar o _rundll32.exe_ para chamar funções de biblioteca não é a maneira mais adequada de fazê-lo e não funciona com todas as funções, principalmente as que precisam de parâmetros que não são do tipo _string_.  Somente o utilizamos aqui para fins de compreensão do conteúdo.
{% endhint %}

Em breve também aprenderemos como _debugar_ uma DLL, mas por hora o conhecimento reunido aqui é suficiente.

