# Endereçamento

### Memória virtual

Onde já se viu dois executáveis com o **ImageBase** em 0x400000 rodarem ao mesmo tempo se ambos são carregados no mesmo endereço de memória? Bem, a verdade é que não são. Existe um esquema chamado de **memória virtual** que consiste num mapeamento da memória RAM real física para uma memória virtual para cada processo no sistema, dando a eles a ilusão de que estão sozinhos num ambiente monotarefa como era antigamente \(vide MS-DOS e outros sistemas antigos\). A tabela a seguir ilustra este mecanismo:

| Identificador do processo \(PID\) | Tamanho em memória | Endereço virtual | Endereço físico |
| :--- | :--- | :--- | :--- |
| 341 | 0x400 | 0x400000 | 0x80000000 |
| 2181 | 0x1000 | 0x400000 | 0x80000400 |
| 124 | 0x800 | 0x400000 | 0x80001400 |
| 843 | 0x2000 | 0x400000 | 0x80001C00 |

A tabela é apenas ilustrativa, mas abrange o necessário para entender o que acontece no Windows de 32-bits em relação à memória virtual: o sistema gerencia uma tabela que relaciona endereço físico de memória \(real\) com endereço virtual, para cada processo. Por isso é possível para o _kernel_ \(o núcleo\) do sistema saber que, por exemplo, de 0x80000000 a 0x80000400 reside o processo de PID 341, mesmo que este pense que está sozinho no endereço 0x400000. É uma completa enganação mesmo. Todos acham que estão sozinhos, mas na verdade rodam juntos sob controle do _kernel_.

### Endereço Virtual

O endereço virtual, em inglês Virtual Address, ou simplesmente VA, é justamente a localização virtual em memória de um dado ou instrução. Por exemplo, quando alguém fazendo engenharia reversa num programa diz que no endereço 0x401000 existe uma função que merece atenção, quer dizer que ela está no VA 0x401000 do binário quando carregado. Para ver a mesma função, você precisa carregar o binário em memória \(normalmente feito com um _debugger_, como veremos num capítulo futuro\) e apontar para tal endereço.

### Endereço Virtual Relativo

Em inglês, _Relative Virtual Address_, é um VA não absoluto, mas relativo à uma base. Por exemplo, o valor do campo _entrypoint_ no cabeçalho Opcional é um RVA relativo à base da imagem \(campo _ImageBase_ no mesmo cabeçalho\). Sendo assim, avalie seu valor na saída a seguir:

```text
Optional/Image header
    Magic number:                    0x10b (PE32)
    Linker major version:            48
    Linker minor version:            0
    Size of .text section:           0x1a00
    Size of .data section:           0x800
    Size of .bss section:            0
    Entrypoint:                      0x39c2
    Address of .text section:        0x2000
    Address of .data section:        0x4000
    ImageBase:                       0x400000
    Alignment of sections:           0x2000
    Alignment factor:                0x200
    Major version of required OS:    4
    Minor version of required OS:    0
    Major version of image:          0
    Minor version of image:          0
    Major version of subsystem:      4
    Minor version of subsystem:      0
    Size of image:                   0x8000
    Size of headers:                 0x200
    Checksum:                        0
    Subsystem required:              0x3 (IMAGE_SUBSYSTEM_WINDOWS_CUI)
    DLL characteristics:             0x8540
    DLL characteristics names
                                         IMAGE_DLLCHARACTERISTICS_DYNAMIC_BASE
                                         IMAGE_DLLCHARACTERISTICS_NX_COMPAT
                                         IMAGE_DLLCHARACTERISTICS_NO_SEH
                                         IMAGE_DLLCHARACTERISTICS_TERMINAL_SERVER_AWARE
    Size of stack to reserve:        0x100000
    Size of stack to commit:         0x1000
    Size of heap space to reserve:   0x100000
    Size of heap space to commit:    0x1000
```

No exemplo acima, o campo _entrypoint_ tem o valor 0x39c2, que é um RVA. Como este campo é relativo ao _ImageBase_, o VA \(endereço virtual\) do _entrypoint_ é então dado pela sua soma com o valor de _ImageBase_:

```text
entrypoint_va = entreypoint_rva + imagebase
entrypoint_va = 0x39c2 + 0x400000
entrypoint_va = 0x4039c2
```

{% hint style="danger" %}
Os RVA's podem ser relativos à outras bases que não a base da imagem. É preciso consultar na documentação qual a relatividade de um RVA para convertê-lo corretamente para o VA correspondente.
{% endhint %}



