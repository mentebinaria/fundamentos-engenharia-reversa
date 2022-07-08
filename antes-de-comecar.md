# 👀 Antes de Começar

Todo escritor quer que sua mensagem seja lida e compreendida, isso não é diferente no meu caso. Então, estabeleci umas regras em meu processo de escrita para facilitar o seu processo de compreensão da disciplina de Engenharia Reversa.

## Terminologia

Utilizo do _itálico_ para neologismos, palavras em Inglês ou em outro idioma, como _crypter_ \(encriptador\). Em geral, prefiro não utilizar termos "aportuguesados" como baite \(para _byte_\) ou linkeditor \(para _linker\)._ Acho que isso confunde o leitor e por isso mantenho-os em seus originais em Inglês, mas em itálico.

Já **negrito** utilizo para dar destaque ou para me referir à um comando.

Algumas vezes utilizo o termo **GNU/Linux** ao invés de somente Linux. O projeto GNU é o principal projeto da FSF \(_Free Software Foundation_\), que criou o conceito de software livre. Suas ferramentas são parte essencial de qualquer sistema operacional baseado no kernel Linux e por isso faz bem lembrá-la de vez em quando.

Após a introdução, **engenharia reversa** \(ou simplesmente **ER**\) passa a ser utilizado como forma curta de engenharia reversa de _software_.

Nas **operações bit-a-bit** \(_bitwise_\), utilizo os símbolos da programação para representar as operações E, OU, OU EXCLUSIVO, etc. No texto, adoto seus mnemônicos em Inglês: AND, OR e XOR.

## Arquitetura de Software

Cada frase deste livro, a não ser que expressado diferente, considera a arquitetura Intel x86 \(IA-32\), visto que esta é documentada o suficiente para começar o estudo de engenharia reversa e moderna o suficiente para criar exemplos funcionais de código e analisar programas atuais.

## Exercícios

Este livro é recheado de trechos de código. É recomendável que o leitor pratique escrevendo os códigos no ambiente específico cada vez que encontrar blocos como os abaixo.

Exemplos de código em Python como a seguir devem ser digitados no ambiente do Python da sua máquina virtual \(ou real\) Linux:

```python
>>> print 'Execute isto no console do Python!'
```

Vários exemplos são no _shell_ do Linux, que é o Bash por padrão:

```bash
$ echo 'Este vai no Bash'
```

O leitor também encontrará códigos em linguagem C como este:

```c
#include <stdio.h>

int main(void) {
    printf("Compilar com o gcc e executar!\n");
}
```

Neste caso, exceto quando especificado diferente, é preciso salvar o código em C num arquivo de texto e assim compilar e executar no ambiente GNU/Linux, assim:

```bash
$ gcc -o exemplo exemplo.c
$ ./exemplo
```

Caso o código em C inclua a _windows.h_, então deve ser compilado em ambiente Windows utilizando de preferência o Visual Studio Community, disponível gratuitamente em https://aka.ms/vs.

Há ainda outros tipos de blocos, mas tenha em mente que é **necessário** para o aprendizado que você os escreva, execute e analise seus resultados.

## Preparação do ambiente

Este é um guia prático. Sendo assim, é recomendável que você seja capaz de reproduzir o que é sugerido neste livro em seu próprio ambiente. Precisaremos de:

* Uma máquina \(virtual ou real\) com Windows 7 ou 10.
* Uma máquina \(virtual ou real\) com Linux. Usamos o Ubuntu, mas qualquer outra serve desde que você saiba instalar pacotes nela.

Na máquina com Windows, os programas necessários são:

* Detect It Easy - https://horsicq.github.io
* HxD - https://mh-nexus.de
* Python 3 - https://www.python.org
* Visual Studio Community - https://aka.ms/vs
* x64dbg - https://x64dbg.com
