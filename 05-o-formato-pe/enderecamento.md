# Endereçamento

## Memória virtual

Como poderiam dois executáveis com o mesmo **ImageBase** rodarem ao mesmo tempo se ambos são carregados no mesmo endereço de memória? Bem, a verdade é que não são. Existe um esquema chamado de **memória virtual** que consiste num mapeamento da memória RAM real física para uma memória virtual para cada processo no sistema, o que dá aos processos a ilusão de que estão sozinhos num ambiente monotarefa como era antigamente \(vide MS-DOS e outros sistemas antigos\). Essa memória virtual também pode ser mapeada para um arquivo em disco, como o _pagefile.sys_. O desenho a seguir ilustra o mecanismo de mapeamento:

![Memória Virtual](../.gitbook/assets/memoria_virtual.png)

Conforme explicado no capítulo sobre as Seções dos arquivos PE, a memória é dividida em páginas, tanto a virtual quanto a física. No desenho, os dois processos possuem páginas mapeadas pelo _kernel_ \(pelo gerenciador de memória, que é parte deste\) em memória física e em disco \(sem uso no momento\). Perceba que as páginas de memória não precisam ser contíguas \(uma imediatamente após a outra\) no _layout_ de memória física, nem no da virtual. Além disso, dois processos diferentes podem ter regiões virtuais mapeadas para a mesma região da memória física, o que chamamos de páginas compartilhadas.

Em resumo, o sistema gerencia uma tabela que relaciona endereço físico de memória \(real\) com endereço virtual, para cada processo. Todos os "acham" que estão sozinhos no sistema, mas na verdade estão juntos sob controle do _kernel_.

## Endereço Virtual

O endereço virtual, em inglês _Virtual Address_, ou simplesmente VA, é justamente a localização virtual em memória de um dado ou instrução. Por exemplo, quando alguém fazendo engenharia reversa num programa diz que no endereço 0x401000 existe uma função que merece atenção, quer dizer que ela está no VA 0x401000 do binário quando carregado em memória. Para ver a mesma função, você precisa carregar o binário em memória \(normalmente feito com um _debugger_, como veremos num capítulo futuro\) e verificar o conteúdo de tal endereço.

## Endereço Virtual Relativo

Em inglês, _Relative Virtual Address_, é um VA que, ao invés de ser absoluto, é relativo à alguma base. Por exemplo, o valor do campo _entrypoint_ no cabeçalho Opcional é um RVA relativo à base da imagem \(campo _ImageBase_ no mesmo cabeçalho\). Com isso em mente, avalie seu valor na saída do comando **dumpbin** a seguir:

```text
Dump of file c:\windows\system32\calc.exe

PE signature found

File Type: EXECUTABLE IMAGE

FILE HEADER VALUES
            8664 machine (x64)
               7 number of sections
        EE8136FB time date stamp
               0 file pointer to symbol table
               0 number of symbols
              F0 size of optional header
              22 characteristics
                   Executable
                   Application can handle large (>2GB) addresses

OPTIONAL HEADER VALUES
             20B magic # (PE32+)
           14.38 linker version
            2000 size of code
            9000 size of initialized data
               0 size of uninitialized data
            1740 entry point
            1000 base of code
       140000000 image base
            1000 section alignment
            1000 file alignment
-- suprimido --
```

No exemplo acima, o campo _entrypoint_, que é um RVA, tem o valor 0x1740. Como este campo é relativo ao _ImageBase_, o VA \(endereço virtual\) do _entrypoint_ é então dado pela sua soma com o valor de _ImageBase_:

```python
>>> ep = 0x1740 + 0x140000000
>>> hex(ep)
'0x140001740'
```

{% hint style="danger" %}
Os RVA's podem ser relativos à outras bases que não à da imagem. É preciso consultar na documentação qual a relatividade de um determinado RVA para convertê-lo corretamente para o VA correspondente.
{% endhint %}

Com isso encerramos o capítulo sobre o formato PE. Agora que você conhece a estrutura de um executável, vamos ver o que acontece depois que alguém dá um duplo-clique nele.
