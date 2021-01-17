# 🖼 Windows API

Uma API \(_Application Programming Interface_\) é uma interface para programar uma aplicação. No caso da Windows API, esta consiste num conjunto de funções expostas para serem usadas por aplicativos rodando em _user mode_.

Para o escopo deste livro, vamos cobrir uma minúscula parte da Win32 API \(outro nome para a Windows API\), com foco nas funções disponíveis basicamente em duas bibliotecas diferentes: a USER32.DLL e a KERNEL32.DLL.

Considere o seguinte programa em C:

```c
#include <windows.h>

int main(void) {
    MessageBox(NULL, "Cash", "Johnny", MB_OK);
    return 0;
}
```

A função _MessageBox\(\)_ está definida em _windows.h_. Quando compilado, o código acima gera um executável dependente da USER32.DLL \(além de outras bibliotecas\), que provê a versão compilada de tal função. A documentação desta e de outras funções da Win32 está disponível no [site da Microsoft](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-messagebox). Copiamos seu protótipo abaixo para explicar seus parâmetros:

```c
int WINAPI MessageBox(
    _In_opt_ HWND    hWnd,
    _In_opt_ LPCTSTR lpText,
    _In_opt_ LPCTSTR lpCaption,
    _In_     UINT    uType
);
```

A Microsoft utiliza em várias de suas documentações de funções em C uma espécie de linguagem de anotação alheia à linguagem C padrão, que ela criou chamou de [SAL _\(Source-code Annotation Language\)_](https://docs.microsoft.com/en-us/cpp/code-quality/understanding-sal). Além disso, ela criou definições de novos tipos na linguagem que precisam ser explicadas para o entendimento dos protótipos das funções de sua API. Para entender o protótipo da função _MessageBox_, é preciso conhecer o significado dos seguintes termos:

|  |  |
| :--- | :--- |
| WINAPI | Define que a convenção de chamada da função é a \_\_stdcall |
| \_In\_ | Define que o parâmetro é de entrada |
| \_Out\_ | Define que o parâmetro é de saída \(a função vai escrever nele\) |
| \_opt\_ | O parâmetro é opcional \(pode ser NULL, ou 0 normalmente\) |
| HANDLE | Um número identificador de um objeto no ambiente Windows |
| HWND | Um _handle_ \(identificador\) da janela |
| LPCTSTR | **L**ong **P**ointer to a **C**onst **T**CHAR **STR**ing |
| UINT | _unsigned int_ ou DWORD \(32-bits\) |

{% hint style="info" %}
Um _handle_ é um número que identifica um objeto \(arquivo, chave de registro, diretório, etc\) aberto usado por um processo. É um conceito similar ao _file descriptor_ em ambiente Unix/Linux. _Handles_ só são acessíveis diretamente em _kernel mode_, por isso os programas interagem com eles através de funções da API do Windows. Por exemplo, a função CreateFile\(\) retorna um handle em caso de sucesso, enquanto a função CloseHandle\(\) o fecha.
{% endhint %}

Agora vamos explicar os parâmetros da função _MessageBox_:

## MessageBox

### hWnd \[entrada, opcional\]

Um _handle_ que identifica qual janela é dona da caixa de mensagem. Isso serve para atrelar uma mensagem a uma certa janela \(e impedi-la de ser fechada antes da caixa de mensagem, por exemplo\). Como é opcional, este parâmetro pode ser NULL, o que faz com que a caixa de mensagem não possua uma janela dona.

### lpText \[entrada, opcional\]

Um ponteiro para um texto \(uma _string_\) que será exibido na caixa de mensagem. Se for NULL, a mensagem não terá um conteúdo, mas ainda assim aparecerá.

### lpCaption \[entrada, opcional\]

Um ponteiro para o texto que será o título da caixa de mensagem. Se for NULL a caixa de mensagem não terá um título, mas ainda assim aparecerá.

### **uType \[entrada\]**

Configura o tipo de caixa de mensagem. É um número inteiro que pode ser definido por macros para cada _flag_, definida na [documentação da função](https://msdn.microsoft.com/pt-br/library/windows/desktop/ms645505%28v=vs.85%29.aspx). Se passada a macro MB\_OKCANCEL \(0x00000001L\), por exemplo, faz com que a caixa de mensagem tenha dois botões: OK e Cancelar. Se passada a macro MB\_ICONEXCLAMATION \(0x00000030L\), a janela terá um ícone de exclamação. Se quiséssemos combinar as duas características, precisaríamos passar as duas _flags_ utilizando uma operação OU entre elas, assim:

```c
MessageBox(NULL, "Cash", "Johnny", MB_OKCANCEL | MB_ICONEXCLAMATION);
```

Como macros e cálculos assim são resolvidos em C numa etapa conhecida por pré-compilação, o resultado da operação OU entre 1 e 0x30 será substituído neste código, antes de ser compilado, ficando assim:

```c
MessageBox(0, "Cash", "Johnny", 0x31);
```

{% hint style="warning" %}
Dizer que um parâmetro é opcional não quer dizer que você não precise passá-lo ao chamar a função, mas sim que ele pode ser NULL, ou zero, dependendo do que a documentação da função diz.
{% endhint %}

Funções importantes da Win32 incluem CreateFile, DeleteFile, RegOpenKey, RegCreateKey, dentre outras. É altamente recomendado que o leitor crie programas de exemplo utilizando-as para atestar o funcionamento delas. Você encontrará algumas dessas funções separadas por categoria no apêndice [Funções da API do Windows](apendices/funcoes-api-win.md).

