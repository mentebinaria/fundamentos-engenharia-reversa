# Unicode

À esta altura você já pode imaginar a dificuldade que programadores enfrentam em trabalhar com diferentes codificações de texto, mas existe um esforço chamado de Unicode mantido pelo Unicode Consortium que compreende várias codificações, que estudaremos a seguir. As _strings_ neste formato são comumente chamadas de _**wide strings**_ (largas, numa tradução livre).

## UTF-8

O padrão UTF (_Unicode Transformation Format_) de 8 _bits_ foi desenhado originalmente por Ken Thompson (o criador do Unix!) e Rob Pike (o criador da linguagem Go) para abranger todos os caracteres possíveis nos vários idiomas deste planeta.

Os primeiros 128 caracteres da tabela UTF-8 possuem exatamente os mesmos valores da tabela ASCII padrão e somente necessitam de 1 _byte_ para serem representados. Chamamos estes números de _codepoints_. Os próximos caracteres utilizam **2** _**bytes**_ e compreendem não só o alfabeto latino (como na ASCII estendida com codificação ISO-8859-1) mas também os caracteres gregos, árabes, hebraicos, dentre outros. Já para representar os caracteres de idiomas como o chinês e japonês, **3** _**bytes**_ são necessários. Por fim, há os caracteres de antigos manuscritos, símbolos matemáticos e até _emojis,_ que utilizam **4** _**bytes**_.

Concluímos que os caracteres UTF-8 **variam** de 1 a 4 bytes. Sendo assim, como ficaria o texto "mentebinária" numa sequência de _bytes_? Podemos ver novamente com o Python, mas dessa vez ao invés declarar um objeto do tipo `bytes` com aquele prefixo **`b`**, vamos converter um tipo `str` para `bytes` utilizando a função `encode()`. Isso é necessário porque queremos ver uma string UTF-8 e não ASCII:

```python
>>> 'mentebinária'.encode('utf-8').hex(' ')
'6d 65 6e 74 65 62 69 6e c3 a1 72 69 61'
```

Como dito antes, os codepoints da tabela ASCII são os mesmos em UTF-8, mas o caractere 'á' (que não existe em ASCII puro) utiliza 2 bytes (no caso, C3 A1) para ser representado. Esta é uma _string_ UTF-8 válida. Dizemos que seu tamanho é 11 porque ela contém 11 caracteres, mas em _bytes_ seu tamanho é 12.

## UTF-16

Também conhecido por UCS-2, este tipo de codificação é frequentemente encontrado em programas compilados para Windows, incluindo os escritos em .NET. É de extrema importância que você o conheça bem.

Representados em UTF-16, os caracteres da tabela ASCII possuem **2 bytes** de tamanho, mesmo que não precisem. O _byte_ adicional estará zerado. Vamos entender melhor com a ajuda do Python.

Primeiro, exibimos os _bytes_ em hexa equivalentes de cada caractere da string:

```python
>>> b'mente'.hex(' ')
'6d 65 6e 74 65'
```

Até aí nenhuma novidade, mas vejamos como essa string seria codificada em UTF-16:

```python
>>> 'mente'.encode('utf-16').hex(' ')
'ff fe 6d 00 65 00 6e 00 74 00 65 00'
```

A primeira dupla de _bytes_ é FF FE, mas de onde ela veio? Esta é a _Byte Order Mark_ (BOM) ou Marca de Ordem de _Byte_, em português e define a ordem (ou _endianness_) dos bytes nos _codepoints_. Se for FF FE como neste caso, os _bytes_ estão em _little-endian_, o que significa que o _byte_ menos significativo está à esquerda. Em outras palavras, o número 0x0006d será representado como 6D 00. Se o bom fosse FE FF, então esse número seria representado como 00 6D.

Também é possível utilizar a codificação UTF-16-LE que já utiliza _little-endian_ por padrão, sem precisar da BOM:

```python
>>> 'mente'.encode('utf-16-le').hex(' ')
'6d 00 65 00 6e 00 74 00 65 00'
```

A codificação UTF-16-LE (lembre-se: sem BOM) é a utilizada pelo Visual Studio no Windows quando tipos `WCHAR` são usados, como nos argumentos das funções `MessageBoxW()` e `CreateFileW()`. Também é a codificação padrão para programas em .NET. Isto é importante de saber pois se você precisar alterar uma string UTF-16-LE durante a engenharia reversa, vai ter que respeitar essas regras.

Além da UTF-16-LE, temos a UTF-16-BE (_Big Endian_), onde os _bytes_ estão em _big-endian_, ou seja, na ordem direta, com o _byte_ mais significativo à esquerda:

```python
>>> 'mente'.encode('utf-16-be').hex(' ')
'00 6d 00 65 00 6e 00 74 00 65'
```

Além disso, é importante ressaltar que em strings UTF-16 também há a possibilidade de caracteres de quatro _bytes_. Por exemplo, um _emoji_:

```python
>>> '💚'.encode('utf-16-le').hex(' ')
'3d d8 9a dc'
```

### Codepoints da ISO-8859-1 na UTF-16

Os números (_codepoints_) utilizados pela ISO-8859-1 para seus caracteres são também os números utilizados em strings UTF-16. No Windows, como já falado, o padrão é o UTF-16-LE. Para entender como isso funciona, observe primeiro os _bytes_ da string "binária" na codificação ISO-8859-1:

```python
>>> 'binária'.encode('iso-8859-1').hex(' ')
'62 69 6e e1 72 69 61'
```

Perceba que o _byte_ referente ao "á" é o E1. Até aí nenhuma novidadade. Sabemos que é uma string ASCII estendida que usa a tabela ISO-8859-1, também conhecida por Latin-1. Agora, vejamos como ela fica em UTF-16-LE:

```python
>>> 'binária'.encode('utf-16-le').hex(' ')
'62 00 69 00 6e 00 e1 00 72 00 69 00 61 00'
```

Nesse caso, "binária" é uma string UTF-16-LE sem BOM. Os _bytes_ dos caracteres em si coincidem com os da ISO-8859-1. Doido né? Mas vamos em frente!

> Perceba que o "á" em UTF-8 é C3 A1, mas em UTF-16 é E1 (precedido ou sucedido por zero), assim como na codificação ISO-8859-1.

## UTF-32

Sendo pouco utilizado, este padrão utiliza 4 _bytes_ para cada caractere. Vamos ver como fica a string "mb" em UTF-32 com BOM:

```python
>>> 'mb'.encode('utf-32').hex(' ')
'ff fe 00 00 6d 00 00 00 62 00 00 00'
```

Perceba o BOM de quatro _bytes_ ao invés de dois.

Agora em UTF-32-LE:

```python
>>> 'mb'.encode('utf-32-le').hex(' ')
'6d 00 00 00 62 00 00 00'
```

E por fim em UTF-32-BE:

```python
>>> 'mb'.encode('utf-32-be').hex(' ')
'00 00 00 6d 00 00 00 62'
```

É importante ressaltar que simplesmente dizer que uma _string_ é Unicode não diz exatamente qual codificação ela está utilizando, fato que normalmente depende do sistema operacional, da pessoa que programou, do compilador utilizado, dentre outros fatores. Por exemplo, um programa feito em C no Windows e compilado com Visual Studio tem as _wide strings_ em UTF-16-LE normalmente. Já no Linux, o tamanho do tipo _wchar\_t_ é de 32 _bits_, resultando em _strings_ UTF-32.

Há muito mais sobre codificação de texto para ser dito, mas isso foge do escopo deste livro. Se desejar se aprofundar, basta consultar a documentação oficial dos grupos que especificam estes padrões. No entanto, cabe ressaltar que para a engenharia reversa, a prática de compilar programas e buscar como as _strings_ são codificadas é a melhor escola.
