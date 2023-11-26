# Manipulação de Arquivos

É muito comum softwares trabalharem com arquivos. O mesmo vale para malware. Considero importante, do ponto de vista de engenharia reversa, saber como as funções do Windows que trabalham com arquivos são chamadas.

## CreateFile

Vamos começar pela função `CreateFile`, que tanto cria quando abre arquivos e outros objetos no Windows. O protótipo da versão Unicode dessa função é o seguinte:

```c
HANDLE CreateFileW(
  [in]           LPCWSTR               lpFileName,
  [in]           DWORD                 dwDesiredAccess,
  [in]           DWORD                 dwShareMode,
  [in, optional] LPSECURITY_ATTRIBUTES lpSecurityAttributes,
  [in]           DWORD                 dwCreationDisposition,
  [in]           DWORD                 dwFlagsAndAttributes,
  [in, optional] HANDLE                hTemplateFile
);
```

Agora vamos aos parâmetros:

### lpFileName \[entrada]

O caminho do arquivo que será aberto para escrita ou leitura. Se somente um nome for especificado, o diretório de onde o programa é chamado será considerado. Este parâmetro é do tipo LPCSTR na versão ASCII da função e do tipo LPCSWSTR na versão UNICODE.

### dwDesiredAccess \[entrada]

Este é um campo numérico que designa o tipo de acesso desejado ao arquivo. Os valores possíveis são:

```c
#define GENERIC_READ    (0x80000000L)
#define GENERIC_WRITE   (0x40000000L)
#define GENERIC_EXECUTE (0x20000000L)
#define GENERIC_ALL     (0x10000000L)
```

Também é possível combinar tais valores. Por exemplo, `GENERIC_READ | GENERIC_WRITE` para abrir um arquivo com acesso de leitura e escrita.

### dwShareMode \[entrada]

O modo de compartilhamento deste arquivo com outros processos. Os valores possíveis são:

```c
#define FILE_SHARE_READ                 0x00000001  
#define FILE_SHARE_WRITE                0x00000002  
#define FILE_SHARE_DELETE               0x00000004 
```

No entanto, o valor `0` é bem comum e faz com que nenhum outro processo apossa abrir o arquivo simultâneamente.

### lpSecurityAttributes \[entrada, opcional]

Um ponteiro para uma estrutura especial do tipo `SECURITY_ATTRIBUTES`. Em geral, usamos `NULL`.

### dwCreationDisposition \[entrada]

Ações para tomar em relação à criação do arquivo, pode ser:

```c
#define CREATE_NEW          1
#define CREATE_ALWAYS       2
#define OPEN_EXISTING       3
#define OPEN_ALWAYS         4
#define TRUNCATE_EXISTING   5
```

### dwFlagsAndAttributes \[entrada]

Atributos e flags especiais para os arquivos. O mais comum é passar somente `FILE_ATTRIBUTE_NORMAL`, mas a documentação oficial prevê muitos outros possíveis valores.

### hTemplateFile \[entrada, opcional]

Um handle válido para um arquivo modelo, para ter os atributos copiados. Normalmente é `NULL`.

Colocando tudo junto, podemos criar um arquivo usando a API do Windows assim:

```c
HANDLE hFile = CreateFile(L"log.txt",
	GENERIC_WRITE,
	0,
	nullptr,
	CREATE_ALWAYS,
	FILE_ATTRIBUTE_NORMAL,
	nullptr);
```

Logo após a chamada à `CreateFileA`, é comum encontrar uma comparação para saber se o objeto foi aberto com sucesso. Como esta função retorna um handle para o arquivo ou o valor `INVALID_HANDLE_VALUE` (0xffffffff) em caso de falha, podemos fazer na sequência:

```c
if (hFile == INVALID_HANDLE_VALUE) {
	return EXIT_FAILURE;
}
```

Por fim, é importante fechar o handle obtido para o arquivo. Isso é feito com a função `CloseHandle`:

```c
CloseHandle(hFile);
```

O código que construímos só abre o arquivo, criando-o sempre, e depois o fecha. Nada é escrito nele. Vamos agora escrever algum texto antes de fechar, mas para isso precisamos de mais uma função.

## WriteFile

Essa função escreve dados num objeto. Seu protótipo é o seguinte:

```c
BOOL WriteFile(
  [in]                HANDLE       hFile,
  [in]                LPCVOID      lpBuffer,
  [in]                DWORD        nNumberOfBytesToWrite,
  [out, optional]     LPDWORD      lpNumberOfBytesWritten,
  [in, out, optional] LPOVERLAPPED lpOverlapped
);
```

`hFile` é o handle de um arquivo previamente aberto com a `CreateFile`. O próximo parâmetro, `lpBuffer`, é um ponteiro para os dados que pretendemos escrever no arquivo. A quantidade de bytes a serem escritos é informada pelo parâmetro `nNumberOfBytesToWrite` e a quantidade de bytes que a função conseguiu escrever é colocada num parâmetro de saída opcional `lpNumberOfBytesWritten`. Por fim, o parâmetro `lpOverlapped` é um ponteiro para uma estrutura do tipo `OVERLAPPED` utilizada em casos especials, mas normalmente é `NULL`.

A `WriteFile` retorna `TRUE` se a escrita ocorreu, ou `FALSE` em caso de falha.

Com tais definições, podemos completar nosso programa para fazê-lo escrever um texto no arquivo antes de fechar o arquivo com a `CloseHandle`. O código final fica assim:

```cpp
#include <Windows.h>

int main() {
	HANDLE hFile = CreateFile(L"log.txt",
		GENERIC_WRITE,
		0,
		nullptr,
		CREATE_ALWAYS,
		FILE_ATTRIBUTE_NORMAL,
		nullptr);

	if (hFile == INVALID_HANDLE_VALUE) {
		return EXIT_FAILURE; // expande para 1
	}

	LPCSTR texto = "Programando usando a API do Windows";
	size_t tam = lstrlenA(texto);

	if (WriteFile(hFile, texto, tam, nullptr, nullptr) == FALSE) {
		return EXIT_FAILURE;
	}

	CloseHandle(hFile);
}
```

Ao compilar e rodar este código que produzimos, o programa deve criar o arquivo `log.txt` e escrever o texto "Programando usando a API do Windows" nele.
