# 🖼 Windows API

Uma API (_Application Programming Interface_) é uma interface para uma aplicação "falar" com outra. No caso da Windows API, esta consiste num conjunto de funções expostas para serem usadas por aplicativos rodando em _user mode_.

Para o escopo deste livro, vamos cobrir uma pequena parte da Windows API, pois o assunto é extenso.

Considere o seguinte programa em C:

```c
#include <windows.h>

int main(void) {
    MessageBox(NULL, "Mundo", "Olá", MB_OK);
}
```

A função _MessageBox()_ está definida em _windows.h_. Quando compilado, o código acima gera um executável dependente da `USER32.DLL` (além de outras bibliotecas, dependendo de certas opções de compilação), que provê a versão compilada de tal função. A documentação desta e de outras funções da Windows API está disponível no site da Microsoft. Copiamos seu protótipo abaixo para explicar seus parâmetros:

```c
int MessageBox(
  [in, optional] HWND    hWnd,
  [in, optional] LPCTSTR lpText,
  [in, optional] LPCTSTR lpCaption,
  [in]           UINT    uType
);
```

A Microsoft criou definições de anotações e novos tipos na linguagem C que precisam ser explicadas para o entendimento dos protótipos das funções de sua API. Para entender o protótipo da função _MessageBox_, é preciso conhecer o significado dos seguintes termos:

|              |                                                          |
| ------------ | -------------------------------------------------------- |
| \[in\]       | Define que o parâmetro é de entrada                      |
| \[optional\] | O parâmetro é opcional (pode ser NULL, ou 0 normalmente) |
| HWND         | Um _handle_ (identificador) da janela                    |
| LPCTSTR      | **L**ong **P**ointer to a **C**onst **T**CHAR **STR**ing |
| UINT         | _unsigned int_ ou DWORD (32-bits ou 4 _bytes_)           |

{% hint style="info" %}
Um _handle_ é um número que identifica um objeto (arquivo, chave de registro, diretório, etc) aberto usado por um processo. É um conceito similar ao _file descriptor_ em ambiente Unix/Linux. _Handles_ só são acessíveis diretamente em _kernel mode_, por isso os programas interagem com eles através de funções da API do Windows. Por exemplo, a função CreateFile() retorna um _handle_ válido em caso de execução com sucesso. A partir daí, toda leitura e escrita neste arquivo deve ser feita a partir do _handle_. Por fim, a função CloseHandle() o fecha o _handle_ quando ele não é mais necessário.
{% endhint %}

Agora vamos explicar os parâmetros da função _MessageBox_:

## MessageBox

### hWnd

É um parâmetro de entrada, ou seja, é uma informação que a função precisa (e não quem chamou). Neste caso, é um _handle_ que identifica qual janela é dona da caixa de mensagem. Isso serve para atrelar uma mensagem a uma certa janela (e impedi-la de ser fechada antes da caixa de mensagem, por exemplo). Como é opcional, este parâmetro pode ser NULL, o que faz com que a caixa de mensagem não possua uma janela dona.

### lpText

Um ponteiro para um texto (uma _string_) que será exibido na caixa de mensagem. Se for NULL, a mensagem não terá um conteúdo, mas ainda assim aparecerá.

### lpCaption

Um ponteiro para o texto que será o título da caixa de mensagem. Se for NULL, a caixa de mensagem terá o título padrão "Error" (pode rir).

### uType

Configura o tipo de caixa de mensagem. É um número inteiro que pode ser definido por macros para cada _flag_ definida na documentação da função. Se passada a macro MB\_OKCANCEL (0x00000001L), por exemplo, faz com que a caixa de mensagem tenha dois botões: OK e Cancelar. Se passada a macro MB\_ICONEXCLAMATION (0x00000030L), a janela terá um ícone de exclamação. Se quiséssemos combinar as duas características, precisaríamos passar as duas _flags_ utilizando uma operação OU entre elas, assim:

```c
MessageBox(NULL, "Mundo", "Olá", MB_OKCANCEL | MB_ICONEXCLAMATION);
```

Como macros e cálculos assim são resolvidos numa etapa conhecida por pré-compilação, o resultado da operação OU entre 1 e 0x30 será substituído neste código, antes de ser compilado, ficando assim:

```c
MessageBox(NULL, "Mundo", "Olá"), 0x31);
```

{% hint style="warning" %}
Dizer que um parâmetro é opcional não quer dizer que você não precise passá-lo ao chamar a função, mas sim que ele pode ser **NULL**, ou **0**, dependendo do que a documentação da função diz. Como o Visual Studio é um compilador de C++, você também pode usar **nullptr**, que também está disponível em C a partir da C23.
{% endhint %}

Veremos agora algumas funções da Windows API para funções básicas, mas você encontrará informações sobre outras rotinas no apêdice Funções da API do Windows.
