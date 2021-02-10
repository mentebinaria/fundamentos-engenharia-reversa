# Funções da API do Windows

Segue uma lista de funções interessantes para colocarmos _breakpoints_ quando revertendo binários em Windows. Em **quais** funções colocaremos os _breakpoints_ vai depender de **onde** queremos que o programa sob o controle do _debugger_ pare afim de observarmos o comportamento de determinada ação. A seguir veremos alguns nomes de funções bem comuns em programas.

## Mensagens

À esta altura, essas funções dispensam apresentações, não é mesmo? :-\)

MessageBox

MessageBoxEx

## Caixas de texto

Usadas para ler textos de caixas de texto.

GetDlgItemText

## Janelas

Habilitam ou desabilitam janelas ou itens dentro dela.

EnableMenuItem

EnableWindow

## Data e hora

SetSystemTime

GetLocalTime

SetLocalTime

## Entrada e saída \(I/O\)

### CopyFile

Usada para copiar um arquivo. Tem uma segunda versão que permite mais parâmetros. Consulte a documentação. ;-\)

{% tabs %}
{% tab title="CopyeFile" %}
```cpp
CopyFile(L"origem.txt", L"destino.txt", false);
```
{% endtab %}

{% tab title="CopyFile2" %}
```cpp
COPYFILE2_EXTENDED_PARAMETERS cf2;
// configura os parâmetros
CopyFile2(L"origem.txt", L"destino.txt", &cf2);
```
{% endtab %}
{% endtabs %}

### CreateFile

Abre para ler e/ou para escrever e também cria e até trunca \(zera\) arquivos no disco. Também trabalha outros objetos como pipes, diretórios, consoles e mais. Segue um exemplo de abertura de um arquivo que vai falhar caso o arquivo não exista:

```cpp
	HANDLE hFile = CreateFileW(L"arquivo.txt",
		GENERIC_READ,
		0,
		nullptr,
		OPEN_EXISTING,
		FILE_ATTRIBUTE_NORMAL,
		nullptr);
```

Existe também uma CreateFile2.

DeleteFile

LoadLibrary

MoveFile

OpenFile

OpenFileMapping

OpenMutex

CreateFileMapping

## Registro

RegOpenKey

RegOpenKeyEx

RegCloseKey

RegQueryValueA

RegEnumKeyExA

RegSetValue

RegSetValueExA

RegSetValueExW

## Processos e _threads_

CreateToolhelp32Snapshot

Process32First

Process32Next

Module32First

Module32Next

Toolhelp32ReadProcessMemory

Heap32ListFirst

Heap32ListNext

Heap32First

Heap32Next

OpenProcess

TerminateProcess

ExitProcess

ExitThread

OpenProcessToken - ADVAPI32.DLL

OpenThreadToken - ADVAPI32.DLL

ZwQueryInformationProcess - NTDLL.DLL

ZwSetInformationThread - NTDLL.DLL

WriteProcessMemory

CreateThread

CreateRemoteThread

CreateProcess

ShellExecute

## Anti-debugging

IsDebuggerPresent

CheckRemoteDebuggerPresent

NtSetInformationThread - NTDLL.DLL

## Strings

lstrcat

lstrcmp

lstrcpy

lstrlen

## Disco

GetDiskFreeSpace

GetDriveType

## Rede

InternetOpenUrl...

## Criptografia

CryptDecrypt...

## Alocação de memória

VirtualAlloc, HeapAlloc...

