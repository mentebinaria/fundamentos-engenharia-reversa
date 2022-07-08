# Manipulação de Arquivos

É muito comum softwares trabalharem com arquivos. O mesmo vale para malware. Considero importante, do ponto de vista de engenharia reversa, saber como as funções do Windows que trabalham com arquivos são chamadas. Vamos começar pela `CreateFile`.

```c
#include <stdio.h>
#include <Windows.h>

int main() {
	HANDLE hFile = CreateFileW(L"log.txt",
		GENERIC_WRITE,
		0,
		nullptr,
		CREATE_ALWAYS,
		FILE_ATTRIBUTE_NORMAL,
		nullptr);

	if (hFile == INVALID_HANDLE_VALUE) {
		printf_s("Erro ao tentar criar o arquivo.");
		return EXIT_FAILURE;
	}

	CloseHandle(hFile);
	return EXIT_SUCCESS;
}
```



