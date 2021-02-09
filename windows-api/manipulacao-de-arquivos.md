# Manipulação de arquivos

{% hint style="warning" %}
Em breve
{% endhint %}

```c
#include <Windows.h>
#include <stdio.h>

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



