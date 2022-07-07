# Funções da API do Windows

Segue uma lista de funções interessantes para colocarmos _breakpoints_ quando revertendo binários em Windows. Em **quais** funções colocaremos os _breakpoints_ vai depender de **onde** queremos que o programa sob o controle do _debugger_ pare afim de observarmos o comportamento de determinada ação. A seguir veremos alguns nomes de funções bem comuns em programas.

## Caixas de Mensagens

Utilizadas para exibir janelas com mensagens.

```c
int MessageBoxA(
  [in, optional] HWND    hWnd,
  [in, optional] LPCTSTR lpText,
  [in, optional] LPCTSTR lpCaption,
  [in]           UINT    uType
);
```

```c
int MessageBoxExA(
  [in, optional] HWND   hWnd,
  [in, optional] LPCSTR lpText,
  [in, optional] LPCSTR lpCaption,
  [in]           UINT   uType,
  [in]           WORD   wLanguageId
);
```

## Caixas de Texto

Usadas para ler textos de caixas de texto.

```c
UINT GetDlgItemTextA(
  [in]  HWND  hDlg,
  [in]  int   nIDDlgItem,
  [out] LPSTR lpString,
  [in]  int   cchMax
);
```

## Janelas

Habilitam ou desabilitam janelas ou itens dentro dela.

```c
BOOL EnableMenuItem(
  [in] HMENU hMenu,
  [in] UINT  uIDEnableItem,
  [in] UINT  uEnable
);
```

```c
BOOL EnableWindow(
  [in] HWND hWnd,
  [in] BOOL bEnable
);
```

## Data e Hora

```c
void GetLocalTime(
  [out] LPSYSTEMTIME lpSystemTime
);
```

Ver também: SetSystemTime e SetLocalTime.

## Entrada e saída \(I/O\)

```c
BOOL CopyFile(
  [in] LPCTSTR lpExistingFileName,
  [in] LPCTSTR lpNewFileName,
  [in] BOOL    bFailIfExists
);
```

Usada para copiar um arquivo. Tem uma segunda versão que permite mais parâmetros. Consulte a documentação. Exemplos:

```cpp
CopyFile(L"origem.txt", L"destino.txt", false);
```

```cpp
COPYFILE2_EXTENDED_PARAMETERS cf2;
// configura os parâmetros
CopyFile2(L"origem.txt", L"destino.txt", &cf2);
```

```c
HANDLE CreateFileA(
  [in]           LPCSTR                lpFileName,
  [in]           DWORD                 dwDesiredAccess,
  [in]           DWORD                 dwShareMode,
  [in, optional] LPSECURITY_ATTRIBUTES lpSecurityAttributes,
  [in]           DWORD                 dwCreationDisposition,
  [in]           DWORD                 dwFlagsAndAttributes,
  [in, optional] HANDLE                hTemplateFile
);
```

`CreateFileA` e `CreateFileW` abrem para ler e/ou para escrever e também criam e até truncam \(zeram\) arquivos no disco. Também trabalham com outros objetos como pipes, diretórios e consoles.

```c
BOOL DeleteFileA(
  [in] LPCSTR lpFileName
);
```

```c
HMODULE LoadLibraryA(
  [in] LPCSTR lpLibFileName
);
```

Consulte também a documentação das funções MoveFile, OpenFile, OpenFileMapping, OpenMutex e CreateFileMapping.

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

