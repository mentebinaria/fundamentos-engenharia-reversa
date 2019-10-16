# C strings

Na linguagem C foi criado um padrão para saber _programaticamente_ o fim de uma _string_: ela precisa ser terminada com um ****_byte_ ****nulo. Sendo assim, a _string_ ASCII "fernando", se utilizada num programa escrito em C, fica no binário compilado \(no .exe final, ou outro formato\) da seguinte forma:

```text
66 65 72 6e 61 6e 64 6f 00
```

{% hint style="warning" %}
É importante não confundir o _nullbyte_ com o caractere de nova linha. Este pode ser o _Line Feed_ \(0x0a\), também conhecido por **\n** em sistemas Unix/Linux. Já no DOS/Windows, a nova linha é definida por dois caracteres: o _Carriage Return_ \(0x0d\) seguido do _Line Feed_, sequência conhecida pelo jargão **CrLf** em linguagens como Visual Basic.
{% endhint %}

Se a _string_ for UTF-16, então dois _bytes_ nulos serão adicionados ao fim. Se for UTF-32, quatro. :-\)

O leitor pode estar se perguntando a razão pela qual este conceito é útil ao engenheiro reverso. Bem, no caso de busca de _strings_ num binário compilado, você pode refinar a busca por sequências de _bytes_ na tabela ASCII, usando ou não uma codificação UNICODE \(já que os valores ASCII são os mesmos\) terminados com por um ou mais _nullbytes_. Por exemplo, supondo que você esteja buscado a _string_ "Erro" dentro de um programa, o primeiro passo é descobrir quais são os _bytes_ equivalentes na tabela ASCII desta _string_. Ao invés de usar a tabela, você pode usar o comando **hd** no Linux:

```bash
$ echo -n Erro | hd
00000000  45 72 72 6f                                       |Erro|
```

Alternativamente, você pode experimentar a função _bh\_str2hex\(\)_ do projeto [bashacks](https://github.com/merces/bashacks), disponível para Linux, macOS e Windows 10:

```bash
$ bh_str2hex Erro
45 72 72 6f
```

Agora sabemos que a sequência de _bytes_ a ser procurada vai depender do tipo de _string_.

* Em ASCII:

45 72 72 6f

* Em UTF-16-LE \(_Little Endian_, que é o padrão\) ou simplesmente UTF-16:

45 00 72 00 72 00 6f

Mas para sermos mais assertivos, caso não haja mais nada depois do "o" da palavra "Erro" no programa, podemos adicionar o _nullbyte_ na busca:

* Em ASCII:

45 72 72 6f 00

* Em UTF-16:

45 00 72 00 72 00 6f 00 00

Claro que os programas feitos para buscarem texto dentro de arquivos já possuem esta inteligência, no entanto, a proposta deste livro é entender a fundo como a engenharia reversa funciona e por isso não poderíamos deixar de cobrir esta valiosa informação. ;-\)

