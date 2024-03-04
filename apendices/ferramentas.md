# Ferramentas

Esta seção aborda não somente ferramentas utilizadas no livro, mas também outras que vale a pena citar na esperança que o leitor se sinta atraído a baixar, usar e tirar suas próprias conclusões em relação à eficiência delas.

## Editores Hexadecimais

Este tipo de ferramenta é útil para editar arquivos binários em geral, não somente executáveis, _dumpar_ (copiar) conteúdo de trechos de arquivos, etc. Também é possível editar uma partição ou disco com bons editores hexadecimal a fim de recuperar arquivos, por exemplo.

| Nome                                                                    | Licença   | Descrição                                                                                                                                                                                                           |
| ----------------------------------------------------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [010 Editor](https://www.sweetscape.com/010editor/)                     | Shareware | Multiplataforma, bastante usado. Suporta templates e scripts. Alguns dizem que é o melhor atualmente.                                                                                                               |
| [fhex](https://github.com/echo-devim/fhex)                              | Livre     | Editor gráfico multiplataforma capaz de exibir o binário graficamente, além de suportar expressões regulares na busca.                                                                                              |
| [GNU poke](http://jemarch.net/poke)                                     | Livre     | Editor REPL genérico (não só para executáveis) com uma abordagem muito interessante. Escrevemos um [artigo](https://www.mentebinaria.com.br/artigos/editando-execut%C3%A1veis-com-o-gnu-poke-parte-1-r49/) sobre.   |
| [Helium Hex Edtitor](http://jacquelin.potier.free.fr/HeliumHexEditor/)  | Comercial | Editor incrível para Windows. Tem recursos muito legais como edição e alocação de memória, operações com dados (XOR, SHL, etc) e na versão paga tem suporte a vários algoritmos criptográficos, disassembly e mais. |
| [Hex Workshop](http://www.hexworkshop.com/)                             | Comercial | Pago, somente para Windows, antigo, mas muito bem feito.                                                                                                                                                            |
| [HexFiend](http://ridiculousfish.com/hexfiend/)                         | Livre     | Somente para macOS, com recursos legais como diff e data inspector.                                                                                                                                                 |
| [hexxed](https://github.com/meme/hexxed)                                | Livre     | Baseado na ncurses, portanto de interface gráfica de texto. Tinha potencial, mas o desenvolvimento parou no final de 2022.                                                                                          |
| [Hiew (Hacker's View)](http://www.hiew.ru/)                             | Comercial | Editor (e disassembler) muito poderoso, principalmente por conta de seus módulos HEM. Tem suporte à vários formatos e é muito usado por analistas de malware, mas é pago.                                           |
| [HT Editor](http://hte.sourceforge.net/)                                | Livre     | Interface gráfica baseada em texto, parecido com o Hiew. Tem muitos recursos, mas infelizmente o desenvolvimento parou em 2015. Hora de alguém fazer um _fork_!                                                     |
| [HxD](https://mh-nexus.de/en/hxd/)                                      | Freeware  | Bem bom. Possui recursos extras como geração de hashes e suporte a abrir discos.                                                                                                                                    |
| [ImHex](https://github.com/WerWolv/ImHex)                               | Livre     | Impressionante editor gráfico para Windows, macOS e Linux. Possui uma linguagem de _patterns_ para aplicar no arquivo, exporta/importa patches e muito mais.                                                        |
| [Reverse Engineer's Hex Editor](https://github.com/solemnwarning/rehex) | Livre     | Nova proposta de um editor especificamente para engenharia reversa.                                                                                                                                                 |
| [wxHexEditor](https://sourceforge.net/projects/wxhexeditor/)            | Livre     | Multiplataforma, recursos interessantes. Infelizmente o desenvolvimento parou em 2017.                                                                                                                              |
| [XVI32](http://www.chmaas.handshake.de/delphi/freeware/xvi32/xvi32.htm) | Freeware  | Sem muitos recursos, mas quebra um galho no Windows para uma edição rápida.                                                                                                                                         |

## Analisadores Estáticos de Executáveis

Analisam estaticamente os binários, sem carregá-los. São úteis para uma primeira visão sobre um executável desconhecido.

| Nome                                                                        | Licença  | Descrição                                                                                                                                                                                        |
| --------------------------------------------------------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [capa](https://github.com/mandiant/capa)                                    | Livre    | Detecta as capacidades de executáveis PE e shellcodes para Windows.                                                                                                                              |
| [CFF Explorer](https://ntcore.com/?page\_id=388)                            | Freeware | Sendo parte do Explorer Suite, é na real um editor de PE. Com ele é possível adicionar imports, remover seções, etc.                                                                             |
| [DIE (Detect It Easy)](https://horsicq.github.io)                           | Freeware | Detecta compilador, linker, packer e protectors em binários. Também edita os arquivos.                                                                                                           |
| [DUMPBIN](learn.microsoft.com/en-us/cpp/build/reference/dumpbin-reference/) | Freeware | Analisador de PE de linha de comando disponível no SDK do Visual Studio.                                                                                                                         |
| [Edump](https://hiew.ru/#edump)                                             | Freeware | _Parser_ de linha de comando para Windows. Suporta arquivos NE, LX/LE, PE/PE32+, ELF/ELF64 (little-endian), Mach-O (little-endian), e TE.                                                        |
| [Exeinfo PE](http://www.exeinfo.o7.pl/)                                     | Freeware | Detecta compilador, packer, protectors e edita os arquivos, além de suportar vários plugins loucos. Tem versão VIP mediante doação.                                                              |
| [Malwoverview](https://github.com/alexandreborges/malwoverview)             | Livre    | Mais um nacional pra uma primeira impressão de arquivos suspeitos, URLs e domínios. Checa também APKs.                                                                                           |
| [objdump](https://www.gnu.org/software/binutils/)                           | Livre    | Parte do GNU binutils, também analisa PE, além de ELF, a.out, etc.                                                                                                                               |
| [PE-Bear](https://hshrzd.wordpress.com/pe-bear/)                            | Freeware | Analisador gráfico (Qt) multiplataforma que também detecta packers/protectors.                                                                                                                   |
| [PEdump](https://pedump.me)                                                 | Livre    | Analisador online muito legal!                                                                                                                                                                   |
| [pestudio](https://www.winitor.com)                                         | Freeware | Analisador de PE padrão da indústria, com foco em malware. Tem versão Pro (paga).                                                                                                                |
| [pev](https://pev.sourceforge.io)                                           | Livre    | Nosso 💚 toolkit de ferramentas de linha de comando para análise de PE. Artigo introdutório [aqui](https://www.mentebinaria.com.br/artigos/estude-bin%C3%A1rios-de-windows-com-o-novo-pev-r18/). |
| [readelf](https://www.gnu.org/software/binutils/)                           | Livre    | Também parte do binutils, analisador de ELF.                                                                                                                                                     |
| [Stud\_PE](http://www.cgsoftlabs.ro/studpe.html)                            | Freeware | Analisador e editor com suporte a plugins, assinaturas do antigo PEiD, editor de recursos e mais.                                                                                                |

## Bibliotecas para _parsear_ Executáveis

| Nome                                                                               | Licença | Descrição                                                                                                                                                       |
| ---------------------------------------------------------------------------------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [BFD](https://ftp.gnu.org/pub/old-gnu/Manuals/bfd-2.9.1/html\_chapter/bfd\_1.html) | Livre   | **B**inary **F**ile **D**escriptor é a biblioteca usada por programas como readpe e objdump. Tem suporte a muitos tipos de arquivos, incluindo PE e ELF, claro. |
| [libpe](https://github.com/merces/libpe)                                           | Livre   | Nossa 💚 biblioteca multiplataforma para _parsing_ de arquivos PE.                                                                                              |
| [libPeConv](https://github.com/hasherezade/libpeconv)                              | Livre   | Biblioteca em C++ para PE usada pelo PE-Bear.                                                                                                                   |
| [pefile](https://github.com/erocarrera/pefile)                                     | Livre   | Famosa biblioteca em Python pra fazer qualquer coisa com arquivos PE.                                                                                           |
| [PeNet](https://github.com/secana/PeNet)                                           | Livre   | Biblioteca em .Net para PE.                                                                                                                                     |
| [PeParser](https://github.com/dorkbox/PeParser)                                    | Livre   | Biblioteca em Java para PE.                                                                                                                                     |
| [pyelftools](https://github.com/eliben/pyelftools)                                 | Livre   | Biblioteca em Python para _parsear_ binários ELF.                                                                                                               |

## Assemblers

| Nome                                                                                                                         | Licença  | Descrição                                                                                                                                                                                                                    |
| ---------------------------------------------------------------------------------------------------------------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [flat assembler (FASM)](https://flatassembler.net/)                                                                          | Livre    | Assembler bem recente que já vem com vários exemplos de código. Windows e Linux.                                                                                                                                             |
| [GNU Assembler (GAS)](https://sourceware.org/binutils/docs/as/)                                                              | Livre    | Também chamado simplesmente de **as**, é o assembler do projeto GNU e provavelmente já está instalado no seu Linux!                                                                                                          |
| [Microsoft Macro Assembler (MASM)](https://learn.microsoft.com/en-us/cpp/assembler/masm/microsoft-macro-assembler-reference) | Freeware | Atualmente já vem com o Visual Studio da Microsoft, mesmo na versão Community. Segue um [tutorial de como compilar um "Hello, world"](https://www.mentebinaria.com.br/forums/topic/146-hello-world-em-masm-no-windows/).     |
| [Netwide Assembler (NASM)](https://www.nasm.us/)                                                                             | Livre    | Multiplataforma, suporte à sintaxe Intel e bem popular. Veja um [tutorial de como compilar um "Hello, world" no Linux](https://www.mentebinaria.com.br/forums/topic/51-%E2%80%9Chello-world%E2%80%9D-em-nasm-no-linux-x86/). |
| [Yasm Modular Assembler (YASM)](https://yasm.tortall.net)                                                                    | Livre    | Multiplataforma, escrito com base no NASM pra ser um substituto mas acho que não vingou. hehe                                                                                                                                |

## Disassemblers

| Nome                                     | Licença   | Descrição                                                                                                                                         |
| ---------------------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Binary Ninja](https://binary.ninja)     | Comercial | Novo disassembler que teoricamente compete com o IDA. Possui versão [online gratuita](https://cloud.binary.ninja/) mediante registro.             |
| [Ghidra](https://ghidra-sre.org)         | Livre     | Disassembler livre lançado pela NSA. Temos um [treinamento](https://www.mentebinaria.com.br/treinamentos/curso-de-ghidra-r9/) em vídeo sobre ele. |
| [Hopper](https://www.hopperapp.com)      | Comercial | Disassembler com foco em binários de Linux e macOS.                                                                                               |
| [IDA](https://www.hex-rays.com/ida-pro/) | Comercial | Disassembler **interativo** padrão de mercado. Possui versão freeware.                                                                            |

## Debuggers

| Nome                                                                                                  | Licença  | Descrição                                                                                                                                                                                                                                                                                                                              |
| ----------------------------------------------------------------------------------------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [dnSpy](https://github.com/0xd4d/dnSpy)                                                               | Livre    | Somente para .NET, descompila, debuga e (dis)assembla.                                                                                                                                                                                                                                                                                 |
| [edb (Evan's Debugger)](https://github.com/eteran/edb-debugger)                                       | Livre    | Debugger gráfico (Qt) com foco em binários ELF no Linux.                                                                                                                                                                                                                                                                               |
| [GDB (GNU Debugger)](https://sourceware.org/gdb/)                                                     | Livre    | Super conhecido debugger pra Linux do projeto GNU. Tem uma [versão não oficial pra Windows](http://www.equation.com/servlet/equation.cmd?fa=gdb) também. É modo texto, mas possui vários front-ends como [GDBFrontend](https://github.com/rohanrhu/gdb-frontend), dentre [outros](https://sourceware.org/gdb/wiki/GDB%20Front%20Ends). |
| [GEF](https://github.com/hugsy/gef)                                                                   | Livre    | O **G**DB **E**nhanced **F**eatures extende o GDB com recursos para engenharia reversa.                                                                                                                                                                                                                                                |
| [HyperDbg](https://hyperdbg.com)                                                                      | Livre    | Debugger que roda no nível do hypervisor, também conhecido como ring -1.                                                                                                                                                                                                                                                               |
| [OllyDbg](http://ollydbg.de)                                                                          | Freeware | Poderoso debugger, apesar de não mais mantido.                                                                                                                                                                                                                                                                                         |
| [PEDA](https://github.com/longld/peda)                                                                | Livre    | O **P**ython **E**xploit **D**evelopment **A**ssistance também extende o GDB, assim como o GEF. A interface é um pouco diferente, no entanto.                                                                                                                                                                                          |
| [WinDbg](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/debugger-download-tools) | Freeware | Debugger ring0/3, parte integrante do SDK do Windows.                                                                                                                                                                                                                                                                                  |
| [x64dbg](https://x64dbg.com/)                                                                         | Livre    | Debugger user-mode para Windows com suporte a 32 e 64-bits.                                                                                                                                                                                                                                                                            |

## Desofuscadores

| Nome                                                 | Licença | Descrição                                                                            |
| ---------------------------------------------------- | ------- | ------------------------------------------------------------------------------------ |
| [de4dot](https://github.com/0xd4d/de4dot)            | Livre   | Para .NET. Conhece vários ofuscadores e permite trabalhar genericamente também.      |
| [Threadtear](https://github.com/GraxCode/threadtear) | Livre   | Para programas feito em Java. Suporta os ofuscadores mais comuns como Stinger e ZKM. |

## Descompiladores

### .NET

| Nome                                          | Licença | Descrição                                |
| --------------------------------------------- | ------- | ---------------------------------------- |
| [ILSpy](https://github.com/icsharpcode/ILSpy) | Livre   | Descompilador multiplataforma para .NET. |

### Delphi

| Nome                                                                        | Licença | Descrição                           |
| --------------------------------------------------------------------------- | ------- | ----------------------------------- |
| [IDR (Interactive Delphi Reconstructor)](https://github.com/crypto2011/IDR) | Livre   | Para binários compilados em Delphi. |

### Genéricos (C, C++, Delphi, etc)

| Nome                                                    | Licença | Descrição                                                                                                              |
| ------------------------------------------------------- | ------- | ---------------------------------------------------------------------------------------------------------------------- |
| [Reko](https://github.com/uxmal/reko)                   | Livre   | Descompilador genérico com versão de linha de comando, gráfica e até web.                                              |
| [RetDec (Retargetable Decompiler)](https://retdec.com/) | Livre   | Descompilador para C/C++ feito pelo time do Avast. Inclui detector de compilador e packer, plugins para IDA e radare2. |
| [Snowman](https://derevenets.com/)                      | Livre   | Descompilador livre para C/C++. Pode ser usado como plugin no IDA, x64dbg, etc.                                        |

### Java

| Nome                                                                      | Licença | Descrição                                                                                                                                   |
| ------------------------------------------------------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| [CFR](https://www.benf.org/other/cfr/)                                    | Livre   | Descompilador para Java de linha de comando. O output é o código fonte e só.                                                                |
| [IntelliJ IDEA](https://www.jetbrains.com/idea/download/other.html)       | Mista   | Na verdade é uma IDE para programação em Java mas mesmo a versão Community possui descompilador e debugger para classes compiladas em Java. |
| [JD-GUI](https://github.com/java-decompiler/jd-gui)                       | Livre   | Descompilador para Java com GUI.                                                                                                            |
| [Procyon](hhttps://bitbucket.org/mstrobel/procyon/wiki/Java%20Decompiler) | Livre   | Para Java, com suporte a recursos novos da linguagem. É modo texto mas há GUI's disponíveis documentadas no link.                           |

### Visual Basic

| Nome                                            | Licença   | Descrição                                             |
| ----------------------------------------------- | --------- | ----------------------------------------------------- |
| [VB Decompiler](https://www.vb-decompiler.org/) | Comercial | Descompilador para VB5/6 e disassembler para VB .NET. |

## Emuladores

| Nome                                                | Licença | Descrição                                                                                                                                                                                        |
| --------------------------------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [Qiling](https://github.com/qilingframework/qiling) | Livre   | Framework que combina emulação com instrumentação de binários. É possível emular programas de Windows e Linux, além de outros SOs, incluindo ring0.                                              |
| [Speakeasy](https://github.com/mandiant/speakeasy)  | Livre   | Emulador feito pela FireEye, inicialmente para programas nativos de Windows (ring0) mas possui um crescente suporta à programas em usermode (ring3), emulando várias das funções da Windows API. |

## Frameworks

| Nome                          | Licença | Descrição                                                                                                 |
| ----------------------------- | ------- | --------------------------------------------------------------------------------------------------------- |
| [angr](https://angr.io)       | Livre   | Framework para análise estática e simbólica de binários.                                                  |
| [Frida](https://www.frida.re) | Livre   | Framework para instrumentação dinâmica de binários.                                                       |
| [PANDA](https://panda.re)     | Livre   | Plataforma de análise dinâmica baseada no QEMU, que faz até replay de ações.                              |
| [Radare](https://rada.re/r/)  | Livre   | Suíte completa com debugger, disassembler e outras ferramentas para quase todo tipo de binário existente! |

## Monitores de Processos

| Nome                                                                              | Licença  | Descrição                                                                                                                                                                                                                        |
| --------------------------------------------------------------------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Process Explorer](https://learn.microsoft.com/en-us/sysinternals/)               | Freeware | Clássica ferramenta do conjunto SysInternals, que exibe muito mais que o Gerenciador de Tarefas comum.                                                                                                                           |
| [System Explorer](https://github.com/zodiacon/SystemExplorer/releases)            | Livre    | Versão do Pavel Yosifovich que conta com algumas vantagens sobre o Process Explorer, como visualizar informações de processos protegidos (através de um driver), listar todas as threads abertas no Windows, todos os jobs, etc. |
| [System Informer](https://systeminformer.sourceforge.io/) (antigo Process Hacker) | Livre    | Talvez o melhor da categoria. Mostra os handles, objetos, processos, quem tá usando o que e muita mais.                                                                                                                          |

## Sandboxes (Linux)

Existem outros projetos como Limon, Detux, HaboMalHunter, mas na lista abaixo procurei deixar somente os que estão ativos.

| Nome                                          | Licença | Descrição                                                                                  |
| --------------------------------------------- | ------- | ------------------------------------------------------------------------------------------ |
| [LiSa](https://github.com/danieluhricek/LiSa) | Livre   | Possui uma API bem simples e suporta binários ELF compilados para x86, x86-64, ARM e MIPS. |

## Sandboxes (Windows)

Esta lista não inclui serviços de sandbox puramente comerciais.

| Nome                                                              | Licença | Descrição                                                                                                                                    |
| ----------------------------------------------------------------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| [DRAKVUF Sandbox](https://github.com/CERT-Polska/drakvuf-sandbox) | Livre   | Projeto co-financiado pelo CERT da Polônia que usa o engine DRAKVUF para criar uma sandbox de Windows 7 ou Windows 10 sem agente no _guest_. |
| [CAPE](https://github.com/ctxis/CAPE)                             | Livre   | Sandbox específica para extração de configuração de malware.                                                                                 |
| [Cuckoo](https://cuckoosandbox.org/)                              | Livre   | Provavelmente a sandbox mais popular e utilizada.                                                                                            |

## Sandboxes (Online)

| Nome                                                | Licença   | Descrição                                                                                     |
| --------------------------------------------------- | --------- | --------------------------------------------------------------------------------------------- |
| [Any.Run](https://app.any.run/submissions)          | Community | Apesar do nome, só suporta artefatos de Windows, mas é muito bom!                             |
| [Hybrid Analysis](https://www.hybrid-analysis.com/) | Community | Sandbox muito boa. Também é um portal de investigação inclusive com suporte à regras de Yara. |
| [Joe Sandbox](https://www.joesandbox.com/#advanced) | Community | Um dos primeiros serviços. Suporta ELF (x86 e x86-64), PE e documentos.                       |

## Visualizadores Hexadecimais

| Nome                                             | Licença  | Descrição                                                                                                     |
| ------------------------------------------------ | -------- | ------------------------------------------------------------------------------------------------------------- |
| [hdump](https://github.com/merces/hdump)         | Livre    | Clone nosso 💚, multiplataforma (funciona no Windows!) do hexdump que imita a saída do **hd**.                |
| [heksa](https://github.com/raspi/heksa)          | Livre    | Multiplataforma, com saída colorida e recursos interessantes como seek negativo (a partir do fim do arquivo). |
| [HexLasso](https://suszter.com/hexlasso-online/) | Gratuito | Visualizador web que faz análises estatísticas visuais muito úteis.                                           |
| [hexyl](https://github.com/sharkdp/hexyl)        | Livre    | Multiplataforma, também com _output_ colorido, exibição de borda, etc.                                        |
| hexdump/hd                                       | Livre    | Padrão no BSD e Linux. Se chamado por "hd" exibe saída em hexa/ASCII.                                         |
| od                                               | Livre    | Padrão no UNIX e Linux. O comando **od -tx1** produz uma saída similar à do **hd**.                           |
| xxd                                              | Livre    | Vem com o vim. Uma saída similar à do **hd** é obtida com **xdd -g1**.                                        |

Abaixo um comparativo onde dumpamos os primeiros 32 bytes do binário /bin/ls utilizando os visualizadores acima:

```
$ hdump -n 32 /bin/ls
00000000  7f 45 4c 46 01 01 01 00  00 00 00 00 00 00 00 00  |.ELF............|
00000010  03 00 28 00 01 00 00 00  21 3e 00 00 34 00 00 00  |..(.....!>..4...|

$ heksa -l 32 /bin/ls
00000┊7f 45 4c 46 01 01 01 00  00 00 00 00 00 00 00 00┊.ELF...Ø ØØØØØØØØ
00010┊03 00 28 00 01 00 00 00  21 3e 00 00 34 00 00 00┊.Ø(Ø.ØØØ !>ØØ4ØØØ

$ hexyl -n32 /bin/ls
┌────────┬─────────────────────────┬─────────────────────────┬────────┬────────┐
│00000000│ 7f 45 4c 46 02 01 01 00 ┊ 00 00 00 00 00 00 00 00 │•ELF•••0┊00000000│
│00000010│ 02 00 3e 00 01 00 00 00 ┊ fc 4a 40 00 00 00 00 00 │•0>0•000┊×J@00000│
└────────┴─────────────────────────┴─────────────────────────┴────────┴────────┘

$ hd -n32 /bin/ls
00000000  7f 45 4c 46 01 01 01 00  00 00 00 00 00 00 00 00  |.ELF............|
00000010  03 00 28 00 01 00 00 00  21 3e 00 00 34 00 00 00  |..(.....!>..4...|
00000020

$ od -Ax -tx1 -N32 /bin/ls
0000000    7f  45  4c  46  01  01  01  00  00  00  00  00  00  00  00  00
0000010    03  00  28  00  01  00  00  00  21  3e  00  00  34  00  00  00
0000020

$ xxd -g1 -l32 /bin/ls
00000000: 7f 45 4c 46 01 01 01 00 00 00 00 00 00 00 00 00  .ELF............
00000010: 03 00 28 00 01 00 00 00 21 3e 00 00 34 00 00 00  ..(.....!>..4...
```

## Outras

| Nome                                            | Licença   | Descrição                                                                                                             |
| ----------------------------------------------- | --------- | --------------------------------------------------------------------------------------------------------------------- |
| [Kaitai Struct](https://kaitai.io/)             | Livre     | Se você pensa em _parsear_ um formato desconhecido ou tá estudando PE/ELF, vale olhar este gerador de _parser_ livre. |
| [Intezer Analyzer](https://analyze.intezer.com) | Community | Online. Requer um pequeno cadastro. Tem um recursos de identificar "genes" estaticamente de famílias de binários.     |
