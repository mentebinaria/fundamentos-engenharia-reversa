# C Strings

Na linguagem C, foi criado um padrão para saber _programaticamente_ o fim de uma _string_: ela precisa ser terminada com um _**byte**_**&#x20;nulo**, também chamado de _nullbyte_. Este nada mais é que um _byte_ zerado. Sendo assim, a _string_ ASCII "fernando", se utilizada num programa escrito em C, fica no binário compilado (no .exe gerado pelo processo de compilação, por exemplo) da seguinte forma:

```
66 65 72 6e 61 6e 64 6f 00
```

{% hint style="warning" %}
É importante não confundir o _nullbyte_ com o caractere de nova linha. Este pode ser o _Line Feed_ (0x0a), também conhecido por _newline_. Já no DOS/Windows, a nova linha é definida por um par de caracteres: _Carriage Return_ (0x0d) seguido de _Line Feed_, sequência conhecida por **CrLf** em linguagens como Visual Basic.
{% endhint %}

Se a _string_ for UTF-16, então dois _bytes_ nulos serão adicionados ao fim. Se for UTF-32, quatro.

Talvez você se pergunte qual a razão pela qual este conceito é útil em engenhria reversa. Bem, no caso de busca de _strings_ num binário compilado, você pode refinar a busca por sequências de _bytes_ na tabela ASCII, usando ou não uma codificação do padrão Unicode (já que os valores ASCII são os mesmos) terminados com por um ou mais _nullbytes_. Por exemplo, supondo que você esteja buscado a _string_ "Erro" dentro de um programa, o primeiro passo é descobrir quais são os _bytes_ equivalentes na tabela ASCII desta _string_. Ao invés de usar a tabela, você pode usar o Python:

```python
>>> 'Erro'.encode().hex(' ')
'45 72 72 6f'
```

Agora sabemos que a sequência de _bytes_ a ser procurada vai depender do tipo de _string._ Se for UTF-16-LE (_Little Endian_, que é o padrão no Windows), podemos montar a _string_ sem nem precisar do Python, bastando para isso colocar os zeros depois de cada _byte_:

45 00 72 00 72 00 6f 00

Mas para sermos mais assertivos, caso não haja mais nada depois do "o" da palavra "Erro" no programa, podemos adicionar o _nullbyte_ na busca:

* Em ASCII:

45 72 72 6f 00

* Em UTF-16-LE:

45 00 72 00 72 00 6f 00 00 00

Claro que os programas feitos para buscarem texto dentro de arquivos já possuem esta inteligência, no entanto, a proposta deste livro é entender a fundo como a engenharia reversa funciona e por isso não poderíamos deixar de cobrir este importante assunto.
