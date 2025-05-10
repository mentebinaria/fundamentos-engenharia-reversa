# Caixas de Mensagens

## Um "Hello, World" Conceituado

Vamos programar um pouco. Neste momento é importante, se ainda não o fez, que você instale o Visual Studio Community.

Abra o Visual Studio e crie um novo projeto do tipo Console App, conforme a imagem abaixo mostra:

![Tela de criação de projeto do Visual Studio Community 2019](../.gitbook/assets/vs\_console\_cpp\_app.png)

Nomeie o projeto como "Mensagem" (sem aspas) e após criá-lo, substitua o conteúdo do arquivo Mensagens.cpp que o Visual Studio criará automaticamente por este:

```cpp
#include <Windows.h>

int main() {
	MessageBox(nullptr,
		L"Estou estudando a Windows API\n\nGostei disso! :)",
		L"Mente Binária",
		MB_OK | MB_ICONINFORMATION);
}
```

Tecle `F5` para rodar o programa e você deve ver uma janela como esta:

![](../.gitbook/assets/msgboxw.png)

Há vários conceitos neste código. Vamos dedicar um tempo a eles. Acompanhe:

* Na **linha 1**, como o Windows utiliza sistemas de arquivos que não são sensíveis ao caso, ou seja, não diferenciam letras maiúsculas de minúsculas, tanto faz escrever `Windows.h`, `windows.h`, `WINDOWS.H` ou mesmo `WiNdOwS.H`. Vai funcionar.
* Na **linha 4** chamei a função `MessageBox`, mas ela na verdade não existe: é uma macro que será substituída pelo pré-processador pelas funções `MessageBoxW` (mais comum) ou `MessageBoxA` (caso a macro `UNICODE` não esteja definida).
* Ainda na **linha 4** introduzi um conceito novo, de `nullptr` ao invés de `NULL`, aproveitando que o compilador utilizado é de C++. Acho melhor de digitar.
* Nas **linhas 5 e 6** (sim, não há o menor problema em colocar os outros parâmetros da função em outras linhas para facilitar a leitura) eu passo para a função o texto e o título, respectivamente. Impossível não notar o `L` colado com as aspas duplas que abrem uma string em C não é mesmo? Ele serve para transformar a string subsequente em uma **wide** string (Unicode), que já estudamos. Este `L` é necessário porque a função `MessageBox` vai expandir, por padrão, para a `MessageBoxW` (perceba o `W` no final) que é a versão da `MessageBox` que trabalha com _strings_ Unicode. Também usamos o caractere de nova linha duas vezes para dividir a mensagem em três linhas, sendo a segunda uma linha vazia.
* Na **linha 7** eu utilizo uma combinação de duas flags: `MB_OK` e `MB_ICONINFORMATION`. Esta última configura este ícone de um "i" numa bolinha azul.

## Lendo o Retorno da Função

Agora vamos criar um programa um pouco maior afim de estudar mais conceitos da API do Windows. Compila aí:

```cpp
#include <Windows.h>

int main() {
	LPCWSTR titulo = L"Mente Binária";
	
	int ret = MessageBox(nullptr,
		L"Você já se registrou em https://menteb.in?",
		titulo,
		MB_YESNO | MB_ICONQUESTION);

	if (ret == IDYES) {
		MessageBox(nullptr, L"Aê! Isso é ser inteligente!", titulo, MB_OK);
	} else if (ret == IDNO) {
		MessageBox(nullptr, L"Tá esperando o que então? Vai lá!", titulo, MB_OK); 
	}
}
```

Vamos analisar os conceitos novos aqui, como fizemos com o programa anterior:

* Na **linha 5** declaro uma variável do tipo `LPCWSTR`. A diferença de `LPCSTR`, que já estudamos, é este "W", de _wide_, para definir uma string Unicode.
* A **linha 7** declara uma variável `ret` do tipo `int` e já a inicializa com o retorno da chamada à `MessageBox`.
* Nas **linhas 12** e **15** comparo o conteúdo da variável `res`, que detém o retorno da chamada à `MessageBox`. Se for igual a `IDYES`, novamente uma macro, mostra uma determinada mensagem. Se for igual a `IDNO`, mostra outra.

{% hint style="success" %}
Em relação às _strings_, há três maneiras de se programar com a Windows API: ASCII (`CHAR`), UNICODE (`WCHAR`) ou em compatibilidade (`TCHAR`), que expandirá para `CHAR` ou `WCHAR`, caso a macro `UNICODE` esteja definida. Atualmente, é recomendado utilizar `WCHAR` e textos `L"assim"`.
{% endhint %}

A tabela abaixo ajuda na compreensão:

| Tipo    | Expansão                                           |
| ------- | -------------------------------------------------- |
| LPSTR   | char\*                                             |
| LPCSTR  | const char\*                                       |
| LPWSTR  | wchar\_t\*                                         |
| LPCWSTR | const wchar\_t\*                                   |
| LPTSTR  | char or wchar\_t dependendo da UNICODE             |
| LPCTSTR | const char or const wchar\_t dependendo da UNICODE |

Vamos fazer algo um pouco mais significativo agora. Vamos pedir ao _kernel_ do Windows que crie um arquivo para nós.
