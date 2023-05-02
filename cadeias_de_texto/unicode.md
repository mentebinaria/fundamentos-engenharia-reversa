# Unicode

A esta altura você já pode imaginar a dificuldade que programadores enfrentam em trabalhar com diferentes codificações de texto, mas existe um esforço chamado de Unicode mantido pelo Unicode Consortium que compreende várias codificações, que estudaremos a seguir. Essas _strings_  são comumente chamadas de _**wide strings**_ (largas, numa tradução livre).

## UTF-8

O padrão UTF (_Unicode Transformation Format_) de 8 _bits_ foi desenhado originalmente por Ken Thompson (sim, o criador do Unix!) e Rob Pike para abranger todos os caracteres possíveis nos vários idiomas deste planeta.

Os primeiros 128 caracteres da tabela UTF-8 são exatamente os mesmos valores da tabela ASCII padrão e somente necessitam de 1 _byte_ para serem representados. Os próximos caracteres utilizam **2** _**bytes**_ e compreendem não só o alfabeto latino (como na ASCII estendida com codificação ISO-8859-1) mas também os caracteres gregos, árabes, hebraicos, dentre outros. Já para representar os caracteres de idiomas como o chinês e japonês, **3** _**bytes**_ são necessários. Por fim, há os caracteres de antigos manuscritos, símbolos matemáticos e até _emojis_ (🤗) que utilizam **4** _**bytes**_.

Concluímos que os caracteres UTF-8 **variam** de 1 a 4 bytes. Sendo assim, como ficaria o texto "papobinário" numa sequência de _bytes_? Podemos ver com os comandos **echo** e **hd** no Linux:

```
$ echo -n "papobinário" | hd
00000000  70 61 70 6f 62 69 6e c3  a1 72 69 6f              |papobin..rio|
```

Como dito antes, os caracteres da tabela ASCII são os mesmos, mas o caractere 'á' utiliza 2 bytes (no caso, 0xc3 e 0xa1) para ser representado. Esta é uma _string_ UTF-8 válida. Dizemos que seu tamanho é 11, já que ela contém 11 caracteres, mas em _bytes_ seu tamanho é 12.

Você pode confirmar que esta é uma _string_ UTF-8 utilizado o comando **file** (presente no Linux e macOS). Veja a diferença:

```
$ echo -n "papobinario" | file -
/dev/stdin: ASCII text, with no line terminators

$ echo -n "papobinário" | file -
/dev/stdin: UTF-8 Unicode text, with no line terminators
```

Como os _shells_ atuais utilizam UTF-8, ao utilizar um caractere não presente na tabela ASCII padrão, uma _string_ UTF-8 é gerada. O traço após o nome do comando **file** o fez ler da entrada padrão (**stdin**). Para saber mais sobre como o comando **file** funciona, assista ao vídeo Identificando arquivos com o comando file, disponível no canal Papo Binário no YouTube.

{% embed url="https://www.youtube.com/watch?v=D7_zPEt5vGs" %}

## UTF-16

Também conhecido por UCS-2, este tipo de codificação é frequentemente encontrado em programas compilados para Windows, incluindo os escritos em .NET. É de extrema importância que você o conheça bem.

Representados em UTF-16, os caracteres equivalentes na tabela ASCII possuem **2 bytes** de tamanho onde o primeiro _byte_ é o mesmo da tabela ASCII e o segundo é um zero. Por exemplo, para se escrever "A" em UTF-16, faríamos: 0x41 0x00. Vamos entender melhor com o comando **strings** do Linux, a seguir.

Primeiro vamos exibir o texto em ASCII "papo" mas ao invés de imprimir na tela, vamos passar a saída para o programa **strings**:

```
$ echo -ne "\x70\x61\x70\x6f" | strings
papo
```

Até aí, nenhuma novidade. O **strings** busca justamente _strings_ naquilo que é passado para ele. Mas vamos agora tentar o mesmo texto escrito em UTF-16, em que cada caractere possui dois _bytes_ e seu equivalente em ASCII e um zerado em sequência:

```
$ echo -ne "\x70\x00\x61\x00\x70\x00\x6f\x00" | strings
$
```

Nada é retornado porque o comando **strings** só busca por padrão _strings_ ASCII e a _string_ que passamos é uma UTF-16. Por sorte este comando possui a opção **-e** em sua versão para Linux que permite especificar a codificação:

```
$ echo -ne "\x70\x00\x61\x00\x70\x00\x6f\x00" | strings -e l
papo
```

Agora sim vemos o texto. O motivo está no manual do comando **strings**. Veja:

```
$ man strings
```

```
-e encoding
--encoding=encoding
Select the character encoding of the strings that are to be found.  Possible values for encoding are:

s = single-7-bit-byte characters (ASCII, ISO 8859, etc., default)
S = single-8-bit-byte characters
b = 16-bit bigendian
l = 16-bit littleendian
B = 32-bit bigendian
L = 32-bit littleendian.

Useful for finding wide character strings. (l and b apply to, for example,
Unicode UTF-16/UCS-2 encodings).
```

Perceba que há uma opção para ASCII estendido também (-S):

```
$ echo binário | strings
$

$ echo binário | strings -e S
binário
```

Ainda sobre UTF-16, é importante observar que no Windows o ASCII estendido com codificação ISO-8859-1 é _encodado_ em UTF-16. Por exemplo, se criarmos um programa em .NET que contenha a _string_ "Papo Binário", ela vai para o executável deste jeito:

```
42 00 69 00 6e 00 e1 00 72 00 69 00 6f 00
```

O "á" é o _byte_ 0xe1 (Tabela ISO-8859-1/Latin-1) seguido de um _nullbyte_ (_byte_ nulo) 0x00. Você vai entender melhor a importância destes conceitos quando buscarmos por texto em programas utilizando _debuggers_ e outras ferramentas.

{% hint style="info" %}
Notou que o comando **strings** tem opções de _endianess_ para caracteres de 16 e 32 _bits_? Acontece que estas codificações suportam tanto _little endian_ (padrão), no qual o _byte_ ASCII do caractere é **seguido** de um _nullbyte_ quanto o _big endian_, no qual ele é **precedido** por um _nullbyte_. Veja:

```bash
$ echo -ne "\x00\x70\x00\x61\x00\x70\x00\x6f" | strings -e b
papo
```

Uma boa leitura adicional é o artigo Viewing strings in executables disponível em https://blog.didierstevens.com.
{% endhint %}

## UTF-32

Raramente utilizado no Windows, porém existente em alguns programas para Linux e Unix, este padrão utiliza 4 _bytes_ para cada caractere. Vamos já analisar a _string_ "papo" em UTF-32 com a opção -L do comando **strings**:

```bash
$ echo -ne "\x70\x00\x00\x00\x61\x00\x00\x00\x70\x00\x00\x00\x6f\x00\x00\x00" | strings -e L
papo
```

É importante ressaltar que simplesmente dizer que uma _string_ é Unicode não diz exatamente qual codificação ela está utilizando, fato que normalmente depende do sistema operacional, da pessoa que programou, do compilador, etc. Por exemplo, um programa feito em C no Windows e compilado com Visual Studio tem as _wide strings_ em UTF-16 normalmente. Já no Linux, o tamanho do tipo _wchar\_t_ é 32 _bits_, resultando em _strings_ UTF-32. Escreva o seguinte programa em C para entender:

```c
#include <wchar.h>

int main(void) {
  wchar_t *s = L"papo";
  wprintf(L"%S\n", s);

  return 0;
}
```

Salve-o no Linux como _wide.c_ e compile utilizando o gcc:

```bash
$ gcc -o wide wide.c
```

Agora vamos buscar as _strings_ dentro deste binário compilado.

Foi dito que o no Linux as _wide strings_ são UTF-32, então a opção correta para utilizarmos com o comando **strings** é a "-L":

```
$ strings -e L wide
papo
```

O mesmo programa compilado em Windows resultaria em _strings_ UTF-16 ao invés de UTF-32, portanto, fique alerta.

Há muito mais sobre codificação de texto para ser dito, mas isso foge ao escopo deste livro. Se o leitor desejar se aprofundar, basta consultar a documentação oficial dos grupos que especificam tais padrões. No entanto, cabe ressaltar que a prática (compilar programas e buscar como as _strings_ são codificadas) é a melhor escola.
