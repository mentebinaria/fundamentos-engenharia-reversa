# Arquivos

Não há dúvida de que o leitor já se deparou com diversos arquivos, mas será que já pensou numa definição para eles? Defino arquivo como uma sequência de _bytes_ armazenada numa mídia digital somados a uma entrada, um registro, no sistema de arquivos \(_filesystem_\) que o referencie. Por exemplo, vamos no Linux criar um arquivo cujo seu conteúdo é somente a _string_ ASCII "mentebinaria.com.br" \(sem aspas\):

```text
$ echo -n 'mentebinaria.com.br' > arquivo
```

Se nosso estudo sobre _strings_ estiver correto, este arquivo deve possuir 19 _bytes_ de tamanho. Chequemos:

```text
$ wc -c arquivo
19 arquivo
```

O comando **wc** conta caracteres \(opção -m\), palavras \(-w\), linhas \(-l\) ou _bytes_ \(-c\) de um arquivo.

E isto, nos sistemas de arquivos modernos, é tudo que contém um arquivo: seu conteúdo. Qualquer outra informação sobre ele, inclusive seu nome fica numa referência neste _filesystem_. A maneira mais rápida de checar tudo isso no Linux é com o comando **stat**:

```text
$ stat arquivo
  File: ‘arquivo’
  Size: 19            Blocks: 8          IO Block: 4096   regular file
Device: 801h/2049d    Inode: 532417      Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/    user)   Gid: ( 1000/    user)
Access: 2017-09-08 17:21:32.745159845 -0400
Modify: 2017-09-08 17:21:24.097159494 -0400
Change: 2017-09-08 17:21:24.097159494 -0400
 Birth: -
```

A referência é justamente o que o Linux chama de _inode_, mas este assunto foge ao escopo de nosso livro. Se quiser entender melhor sobre este e outros detalhes de implementação do Linux, recomendo o livro [Descobrindo o Linux](https://novatec.com.br/livros/linux-3ed/), do João Eriberto Mota Filho.

A pergunta mais interessante para nós é, no entanto, em relação ao **tipo** de arquivo. Criamos eles sem extensão e aqui e é bom lembrar que uma extensão de arquivo nada mais é que parte de seu nome e esta não mantém nenhuma relação com seu tipo real. A única forma de saber um tipo de arquivo é **inferindo-o** através de seu conteúdo. Vamos olhar seu conteúdo com um visualizador hexadecimal, o **hexdump**, invocado por seu atalho mais comum, o **hd**:

```text
$ hd arquivo
00000000  6d 65 6e 74 65 62 69 6e  61 72 69 61 2e 63 6f 6d  |mentebinaria.com|
00000010  2e 62 72                                          |.br|
00000013
```

O **hd** também nos entrega o tamanho do arquivo em hexa, que neste caso é 0x13 ou 19 em decimal. Então, sabendo que todos os _bytes_ deste arquivo estão na faixa numérica da tabela ASCII, podemos dizer então que este é, de fato, um arquivo de texto. :-\)

Uma maneira mais rápida de identificar um tipo de arquivo é trabalhando com o comando **file**, que se utiliza da **libmagic**, uma biblioteca que tem catalogadas várias sequências de _bytes_ que representam tipos de arquivos diferentes:

```text
$ file arquivo
arquivo: ASCII text, with no line terminators

$ file -i arquivo
arquivo: text/plain; charset=us-ascii
```

Como o leitor pode notar, a opção **-i** o **file** também nos dá o tipo MIME \(do inglês _Multipurpose Internet Mail Extensions_\), que é o tipo de arquivo utilizado na Internet para _downloads_, e-mail, etc.

