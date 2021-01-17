# Funções da API do Windows

Segue uma lista de funções interessantes para colocarmos _breakpoints_ quando revertendo binários em Windows. Em **quais** funções colocaremos os _breakpoints_ vai depender de **onde** queremos que o programa sob o controle do _debugger_ pare afim de observarmos o comportamento de determinada ação. A seguir veremos alguns nomes de funções bem comuns em programas.

## Mensagens / Caixas de diálogo

| Função | Biblioteca |
| :--- | :--- |
| MessageBoxA / MessageBoxW | USER32.DLL |
| MessageBoxExA / MessageBoxExW | USER32.DLL |

## Caixas de texto

| Função | Biblioteca |
| :--- | :--- |
| GetDlgItemTextA / GetDlgItemTextW | USER32.DLL |

## Janelas

### EnableWindow

### EnableMenuItem

### EnableWindow

## Entrada e saída \(I/O\)

### CreateFileA

### CreateFileW

### OpenFile

### OpenFileMappingA

### OpenFileMappingW

### OpenMutexA

### OpenMutexW

### LoadLibraryA

### LoadLibraryExA

### LoadLibraryW

### LoadLibraryExW

### CreateFileMappingA

### CopyFileA

### CopyFileW

### CopyFileExA

### CopyFileExW

### MoveFileA

### MoveFileW

### MoveFileExA

### MoveFileExW

### DeleteFileA

### DeleteFileW

### LoadCursorFromFileA

### GetPrivateProfileStringA

### GetPrivateProfileIntA

## Registro

### RegOpenKeyA

### RegOpenKeyExA

### RegCloseKey

### RegQueryValueA

### RegEnumKeyExA

### RegSetValueA

### RegSetValueW

### RegSetValueExA

### RegSetValueExW

## Data e hora

### SetSystemTime

### GetLocalTime

### SetLocalTime

## Processos e _threads_

### CreateToolhelp32Snapshot

### Process32First

### Process32Next

### Process32FirstW

### Module32First

### Module32Next

### Module32FirstW

### Module32NextW

### Toolhelp32ReadProcessMemory

### Heap32ListFirst

### Heap32ListNext

### Heap32First

### Heap32Next

### OpenProcess

### TerminateProcess

### ExitProcess

### ExitThread

### OpenProcessToken

ADVAPI32.DLL

### OpenThreadToken

ADVAPI32.DLL

### ZwQueryInformationProcess

NTDLL.DLL

### ZwSetInformationThread

NTDLL.DLL

### WriteProcessMemory

### CreateThread

### CreateRemoteThread

### CreateProcessA

## Anti-debugging

### IsDebuggerPresent

### CheckRemoteDebuggerPresent

### NtSetInformationThread

NTDLL.DLL

## Strings

### lstrcatA

### lstrcmpA

### lstrcpyA

### lstrlenA

## Disco

### GetDiskFreeSpaceA

### GetDriveTypeA

