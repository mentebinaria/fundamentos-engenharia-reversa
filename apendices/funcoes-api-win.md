# Funções da API do Windows

Através de várias bibliotecas como kernel32.dll, user32.dll, advapi32.dll e ntdll.dll, só para citar algumas, a Windows API provê inúmeras funções para os programas em _usermode_ chamarem. Criar uma lista com todas essas funções seria insano, mas destaco aqui algumas comumente encontradas em programas para Windows. Optei também por colocar o protótipo de algumas delas, para rápida referência, mas todas as informações sobre estas funções podem ser encontradas na documentação oficial. Basta buscar por um nome de função em seu buscador preferido que certamente o site da Microsoft com a documentação da função estará entre os primeiros.

Mesmo que você não sabia tudo sobre a função funciona, essa lista te ajudará a saber **onde** colocar _breakpoints_, de acordo com o seu caso. Por exemplo, caso suspeite que um programa está abrindo um arquivo, pode colocar um _breakpoint_ nas funções `CreateFileA` e `CreateFileW`.

Vamos agora à lista de funções, separadas por categoria.

## Anti-debugging

Utilizadas em técnicas básicas para evitar a depuração de aplicações.

```c
BOOL IsDebuggerPresent();
```

```c
BOOL CheckRemoteDebuggerPresent(
  [in]      HANDLE hProcess,
  [in, out] PBOOL  pbDebuggerPresent
);
```

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

Ver também: `MessageBoxEx`.

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

## Criptografia

Implementam algoritmos de criptografia como algoritmos de hash e criptografia simétrica e assimétrica.

```c
BOOL CryptEncrypt(
  [in]      HCRYPTKEY  hKey,
  [in]      HCRYPTHASH hHash,
  [in]      BOOL       Final,
  [in]      DWORD      dwFlags,
  [in, out] BYTE       *pbData,
  [in, out] DWORD      *pdwDataLen,
  [in]      DWORD      dwBufLen
);
```

Ver também: `CryptDecrypt`, `CryptGenKey`, e `CryptImportKey`.

## Data e Hora

Configuram e consultam data e hora do sistema.

```c
void GetLocalTime(
  [out] LPSYSTEMTIME lpSystemTime
);
```

Ver também: `SetSystemTime` e `SetLocalTime`.

## Discos e Volumes

Pegam informações sobre num disco, pen drive, drives de rede mapeados, etc e também sobre seus volumes \(partições\).

```c
DWORD GetLogicalDrives();
```

Ver também: `GetDiskFreeSpace`, `GetDriveType` e `GetVolumeInformationA`.

## Entrada e Saída \(I/O\)

Manipulam arquivos e outros objetos.

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

Ver também: `CopyFile`, `CreateFileMapping`, `DeleteFile`, `GetFullPathName`, `GetTempPath`, `LoadLibrary`, `MoveFile`, `OpenFile`, `OpenFileMapping` e `OpenMutex`.

## Internet

Funções que falam com a internet, normalmente via HTTP.

```c
HINTERNET InternetOpenUrlA(
  [in] HINTERNET hInternet,
  [in] LPCSTR    lpszUrl,
  [in] LPCSTR    lpszHeaders,
  [in] DWORD     dwHeadersLength,
  [in] DWORD     dwFlags,
  [in] DWORD_PTR dwContext
);
```

Ver também: `HttpSendRequestA`, `InternetOpenA`, `InternetOpenW`, `InternetOpenUrlW` e `InternetReadFile`.

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

## Memória

Gerenciam memória, alocando ou deselocando, configurando permissões, dentre outras operações.

```c
LPVOID VirtualAlloc(
  [in, optional] LPVOID lpAddress,
  [in]           SIZE_T dwSize,
  [in]           DWORD  flAllocationType,
  [in]           DWORD  flProtect
);
```

Ver também: `VirtualAllocEx`, `VirtualFree`, `VirtualLock`, `VirtualProtect` e `VirtualQuery`.

## Processos e _Threads_

Manipulam processos e _threads_. Podem enumerar, alterar o estado, executar e mais.

```c
BOOL CreateProcessA(
  [in, optional]      LPCSTR                lpApplicationName,
  [in, out, optional] LPSTR                 lpCommandLine,
  [in, optional]      LPSECURITY_ATTRIBUTES lpProcessAttributes,
  [in, optional]      LPSECURITY_ATTRIBUTES lpThreadAttributes,
  [in]                BOOL                  bInheritHandles,
  [in]                DWORD                 dwCreationFlags,
  [in, optional]      LPVOID                lpEnvironment,
  [in, optional]      LPCSTR                lpCurrentDirectory,
  [in]                LPSTARTUPINFOA        lpStartupInfo,
  [out]               LPPROCESS_INFORMATION lpProcessInformation
);
```

Ver também: `CreateRemoteThread`, `CreateThread`, `CreateToolhelp32Snapshot`, `ExitProcess`, `ExitThread`, `Heap32First`, `Heap32ListFirst`, `Heap32ListNext`, `Heap32Next`, `Module32First`, `Module32Next`, `OpenProcess`, `OpenProcessToken`, `OpenThreadToken`, `Process32First`, `Process32Next`, `ShellExecute`, `TerminateProcess`, `Toolhelp32ReadProcessMemory`, `WriteProcessMemory`, `ZwQueryInformationProcess` e `ZwSetInformationThread`.

## Registro

Criam e alteram chaves e valores no registro.

```c
LSTATUS RegSetValueA(
  [in]           HKEY   hKey,
  [in, optional] LPCSTR lpSubKey,
  [in]           DWORD  dwType,
  [in]           LPCSTR lpData,
  [in]           DWORD  cbData
);
```

Ver também: `RegCloseKey`, `RegEnumKeyExA`, `RegOpenKey`, `RegOpenKeyEx`, `RegQueryValueA`, `RegSetValueW`, `RegSetValueExA` e `RegSetValueExW`.

## Strings

Manipulam cadeias de texto.

```c
int lstrcmpA(
  [in] LPCSTR lpString1,
  [in] LPCSTR lpString2
);
```

Ver também: `lstrcat`, `lstrcpy` e `lstrlen`.