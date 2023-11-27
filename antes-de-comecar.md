# 👀 Antes de Começar

Todo escritor quer que sua mensagem seja lida e compreendida, isso não é diferente no meu caso. Então, estabeleci umas regras em meu processo de escrita para facilitar o seu processo de compreensão da disciplina de Engenharia Reversa.

## Terminologia

Utilizo do _itálico_ para neologismos, palavras em Inglês ou em outro idioma, como _crypter_ (encriptador). Em geral, prefiro não utilizar termos "aportuguesados" como baite (para _byte_) ou linkeditor (para _linker)._ Acho que isso confunde o leitor e por isso mantenho os originais em inglês, mas em itálico.

Já o **negrito**, utilizo para dar destaque ou para me referir a um nome de ferramenta, por exemplo: o comando **ipconfig**.

Algumas vezes utilizo o termo **GNU/Linux** ao invés de somente Linux. O projeto GNU é o principal projeto da FSF (_Free Software Foundation_), a fundação que criou o conceito de software livre. Suas ferramentas são parte essencial de qualquer sistema operacional baseado no kernel Linux e por isso faz bem lembrá-la.

Após a introdução, **engenharia reversa** passa a ser utilizado como forma curta de engenharia reversa de _software_.

Nas **operações bit-a-bit** (_bitwise_), utilizo os símbolos da programação para representar as operações E, OU, OU EXCLUSIVO, etc. Muitas vezes no texto adoto seus mnemônicos em inglês como em AND, OR e XOR.

## Arquitetura de Software

Cada frase deste livro, a não ser que expressado diferente, considera a arquitetura Intel x86 (IA-32), visto que esta é documentada o suficiente para começar o estudo de engenharia reversa e moderna o suficiente para criar exemplos funcionais de código e analisar programas atuais.

## Preparação do Ambiente

Este é um guia prático. Sendo assim, é recomendável que você seja capaz de reproduzir o que é sugerido neste livro em seu próprio ambiente. Você vai precisar do seguinte:

* Uma máquina (virtual ou real) com Windows 7, 10 ou 11.

Nessa máquina Windows, você deve baixar e instalar os seguintes programas:

* Detect It Easy (DIE) - https://horsicq.github.io
* HxD - https://mh-nexus.de
* Python 3 - https://www.python.org
* Visual Studio Community - https://aka.ms/vs
* x64dbg - https://x64dbg.com

## Exercícios

Este livro é recheado de trechos de código. É recomendável que você pratique escrevendo-os no ambiente específico cada vez que encontrar blocos como os mostrados abaixo.

Exemplos de código em Python como a seguir devem ser digitados no ambiente do Python.

```python
>>> 'Execute isto na console do Python!'
```

Você também encontrará códigos em linguagem C como este:

```c
#include <stdio.h>

int main(void) {
    printf("Compilar com o Visual Studio e executar!\n");
}
```

Este deve ser compilado em ambiente Windows utilizando o Visual Studio.

Tenha em mente que é **necessário** para o aprendizado que você escreva estes códigos, os execute e analise seus resultados, certo? É para o seu próprio bem. :)

Enquanto instala os programas necessários, vamos seguir para a introdução ao assunto!

