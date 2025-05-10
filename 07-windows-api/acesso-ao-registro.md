# Acesso ao Registro

O registro do Windows é um repositório de dados utilizado normalmente para armazenar configurações de programas instalados no sistema operacional e do próprio sistema, mas na real ele não faz distinção do que pode ser armazenado lá, já que suporta vários tipos de dados, incluindo textos, números e dados binários.

A estrutura do registro é parecida com um sistema de arquivos. As chaves são como as pastas e os valores são como os arquivos. Os dados de um valor são como o conteúdo dos arquivos.

O registro tem algumas chaves especiais em sua raiz. São elas:

```c
#define HKEY_CLASSES_ROOT                   (( HKEY ) (ULONG_PTR)((LONG)0x80000000) )
#define HKEY_CURRENT_USER                   (( HKEY ) (ULONG_PTR)((LONG)0x80000001) )
#define HKEY_LOCAL_MACHINE                  (( HKEY ) (ULONG_PTR)((LONG)0x80000002) )
#define HKEY_USERS                          (( HKEY ) (ULONG_PTR)((LONG)0x80000003) )
#define HKEY_PERFORMANCE_DATA               (( HKEY ) (ULONG_PTR)((LONG)0x80000004) )
#define HKEY_PERFORMANCE_TEXT               (( HKEY ) (ULONG_PTR)((LONG)0x80000050) )
#define HKEY_PERFORMANCE_NLSTEXT            (( HKEY ) (ULONG_PTR)((LONG)0x80000060) )
#if(WINVER >= 0x0400)
#define HKEY_CURRENT_CONFIG                 (( HKEY ) (ULONG_PTR)((LONG)0x80000005) )
#define HKEY_DYN_DATA                       (( HKEY ) (ULONG_PTR)((LONG)0x80000006) )
#define HKEY_CURRENT_USER_LOCAL_SETTINGS    (( HKEY ) (ULONG_PTR)((LONG)0x80000007) )
#endif
```

Os valores à direita representam códigos para acessar cada chave. As quatro primeiras chaves são as mais comuns. Dentro delas, é possível criar e ler subchaves e manipular seus valores. Vamos ver como fazer isso estudando a função `RegCreateKey`.

## RegCreateKey

Embora a Microsoft recomende utilizar a versão mais nova dessa função chamada `RegCreateKeyEx`, muitos programas ainda utilizam a versão mais antiga, que estudaremos agora. Eis o protótipo da versão ASCII desta função:

```c
LSTATUS RegCreateKeyW(
  [in]           HKEY    hKey,
  [in, optional] LPCWSTR lpSubKey,
  [out]          PHKEY   phkResult
);
```

Agora vamos aos parâmetros:

### hKey

Uma das chaves raíz, por exemplo: `HKEY_CURRENT_USER` ou `HKEY_LOCAL_MACHINE` (para essa o usuário rodando o programa precisa ter privilégios administrativos).

### lpSubKey

A subchave desejada, por exemplo, se o parâmetro `hKey` `HKEY_LOCAL_MACHINE` e `lpSubKey` é `Software\Microsoft\Windows\`, o caminho completo utilizado pela função será `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\`.

> Alguns textos abreviam essas chaves raíz com as letras iniciais de seu nome. Por exemplo, `HKCU` para `HKEY_CURRENT_USER`, `HKCR` para `HKEY_CLASSES_ROOT` e `HKLM` para `HKEY_LOCAL_MACHINE`. Tais abreviações são válidas para acesso ao registro através de programas como o Registry Editor (regedit.exe), mas não são válidas para uso com a função.

### phkResult

Um ponteiro para uma váriável do tipo `HKEY`, previamente alocada, pois é aqui que a função vai escrever o handle da chave criada ou aberta por ela. Perceba que este é um parâmetro de saída, ou seja, quem chamou a função receberá algo nesta variável que pode ser útil após a chamada de função.

Colocando tudo junto, se quisermos criar a sub-chave `HKCU\Software\Mente Binária`, basta fazer:

```c
HKEY hChave;
RegCreateKey(HKEY_CURRENT_USER, L"Software\\Mente Binária", &hChave);
RegCloseKey(hKey);
```

Perceba que, assim como um handle para arquivo, o handle para chave também precisa ser fechado depois de seu uso.

## RegSetKeyValue

Como o nome sugere, essa função configura um valor em uma chave. Seu protótipo é:

```c
LSTATUS RegSetKeyValueW(
  [in]           HKEY    hKey,
  [in, optional] LPCWSTR lpSubKey,
  [in, optional] LPCWSTR lpValueName,
  [in]           DWORD   dwType,
  [in, optional] LPCVOID lpData,
  [in]           DWORD   cbData
);
```

Já sabemos o que são os parâmetros `hKey` e `lpSubKey`. Nos restam então os seguintes:

### lpValueName

Um ponteiro para uma string contendo o nome do valor. Caso seja `NULL` ou aponte para uma string vazia, o valor padrão da chave é considerado.

### dwType

O tipo do valor. Pode ser um dos seguintes:

```c
#define REG_NONE                       ( 0ul ) // Nenhum tipo
#define REG_SZ                         ( 1ul ) // String UNICODE terminada em null
#define REG_EXPAND_SZ                  ( 2ul ) // String UNICODE terminada em null
                                               // (com suporte à variáveis de ambiente)
#define REG_BINARY                     ( 3ul ) // Dados binários
#define REG_DWORD                      ( 4ul ) // Número de 32-bits em little endian
#define REG_DWORD_LITTLE_ENDIAN        ( 4ul ) // Número de 32-bits (o mesmo que REG_DWORD)
#define REG_DWORD_BIG_ENDIAN           ( 5ul ) // Número de 32-bits em big endian
#define REG_LINK                       ( 6ul ) // Um link (atalho) UNICODE
#define REG_MULTI_SZ                   ( 7ul ) // Várias strings UNICODE
#define REG_RESOURCE_LIST              ( 8ul ) // Lista de recursos num mapa de recursos
#define REG_FULL_RESOURCE_DESCRIPTOR   ( 9ul ) // Lista de recursos na descrição do hardware
#define REG_RESOURCE_REQUIREMENTS_LIST ( 10ul )
#define REG_QWORD                      ( 11ul ) // Número de 64-bits em little endian
#define REG_QWORD_LITTLE_ENDIAN        ( 11ul ) // Número de 64-bits (o mesmo que REG_QWORD)
```

### lpData

Os dados do valor, que deve ter seu tipo compatível com o tipo configurado no parâmetro `dwType`.

### cbData

O tamanho dos dados do valor.

O código abaixo cria uma chave `HKCU\Software\Mente Binária`, configura um valor "Habilitado" do tipo `REG_DWORD` com o dado `1` e um valor "Website" do tipo `REG_SZ` com o dado textual "https://menteb.in":

```cpp
#include <Windows.h>

int main() {
    HKEY hChave;

    RegCreateKey(HKEY_CURRENT_USER, L"Software\\Mente Binária", &hChave);

    LPCWSTR website = L"https://menteb.in";
    DWORD tamanho = (lstrlen(website) + 1) * sizeof(WCHAR);  // +1 para incluir o terminador nulo

    RegSetKeyValueW(hChave, nullptr, L"Website", REG_SZ, website, (DWORD)tamanho);

    DWORD habilitado = 1;
    RegSetKeyValueW(hChave, nullptr, L"Habilitado", REG_DWORD, &habilitado, sizeof(habilitado));

    RegCloseKey(hChave);

    return EXIT_SUCCESS;
}
```

Com este programa finalizamos esta breve introdução à Windows API. Existem, é claro, centenas de outras funções disponíveis para uso, mas é preciso saber programar em C/C++ para utilizá-las em seus programas. Vamos agora ver como os programas ficam depois que os compilamos como você fez aqui. Para isso, vamos iniciar nossos estudos de Assembly.
