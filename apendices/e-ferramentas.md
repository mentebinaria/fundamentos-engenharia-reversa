# Ferramentas

Esta seção aborda não somente ferramentas utilizadas no livro, mas também outras que vale a pena citar na esperança que o leitor se sinta atraído a baixar, usar e tirar suas próprias conclusões em relação à eficiência delas.

## Editores Hexadecimais

Este tipo de ferramenta é útil para editar arquivos binários em geral, não somente executáveis, _dumpar_ (copiar) conteúdo de trechos de arquivos, etc. Também é possível editar uma partição ou disco com bons editores hexadecimal a fim de recuperar arquivos, por exemplo.

<table><thead><tr><th width="266.3333333333333">Nome</th><th width="117">Licença</th><th>Descrição</th></tr></thead><tbody><tr><td><a href="https://www.sweetscape.com/010editor/">010 Editor</a></td><td>Shareware</td><td>Multiplataforma, bastante usado. Suporta templates e scripts. Alguns dizem que é o melhor atualmente.</td></tr><tr><td><a href="https://bvi.sourceforge.net/">bvi</a></td><td>Livre</td><td>Editor de interface de texto (TUI), similar ao HT Editor. Tem versão para DOS/Windows mas reina mesmo é no Linux.</td></tr><tr><td><a href="https://github.com/echo-devim/fhex">fhex</a></td><td>Livre</td><td>Editor gráfico multiplataforma capaz de exibir o binário graficamente, além de suportar expressões regulares na busca.</td></tr><tr><td><a href="http://jemarch.net/poke">GNU poke</a></td><td>Livre</td><td>Editor REPL genérico (não só para executáveis) com uma abordagem muito interessante. Escrevemos um <a href="https://www.mentebinaria.com.br/artigos/editando-execut%C3%A1veis-com-o-gnu-poke-parte-1-r49/">artigo</a> sobre.</td></tr><tr><td><a href="http://jacquelin.potier.free.fr/HeliumHexEditor/">Helium Hex Edtitor</a></td><td>Comercial</td><td>Editor incrível para Windows. Tem recursos muito legais como edição e alocação de memória, operações com dados (XOR, SHL, etc) e na versão paga tem suporte a vários algoritmos criptográficos, disassembly e mais.</td></tr><tr><td><a href="http://www.hexworkshop.com/">Hex Workshop</a></td><td>Comercial</td><td>Pago, somente para Windows, antigo, mas muito bem feito.</td></tr><tr><td><a href="http://ridiculousfish.com/hexfiend/">HexFiend</a></td><td>Livre</td><td>Somente para macOS, com recursos legais como diff e data inspector.</td></tr><tr><td><a href="https://etto48.github.io/HexPatch/">HexPatch</a></td><td>Livre</td><td>Multiplataforma, interface de texto (TUI), suporta editar via SSH. É novo e tem futuro!</td></tr><tr><td><a href="https://hexinator.com/">Hexinator</a></td><td>Shareware</td><td>Linux, macOS e Windows. Tem templates (chamados de "grammars"), scripting em Python e Lua e compara binários.</td></tr><tr><td><a href="https://github.com/meme/hexxed">hexxed</a></td><td>Livre</td><td>Baseado na ncurses, portanto de interface gráfica de texto. Tinha potencial, mas o desenvolvimento parou no final de 2022.</td></tr><tr><td><a href="http://www.hiew.ru/">Hiew (Hacker's View)</a></td><td>Comercial</td><td>Editor (e disassembler) muito poderoso, principalmente por conta de seus módulos HEM. Tem suporte à vários formatos e é muito usado por analistas de malware, mas é pago.</td></tr><tr><td><a href="http://hte.sourceforge.net/">HT Editor</a></td><td>Livre</td><td>Interface gráfica baseada em texto, parecido com o Hiew. Tem muitos recursos, mas infelizmente o desenvolvimento parou em 2015. Hora de alguém fazer um <em>fork</em>!</td></tr><tr><td><a href="https://mh-nexus.de/en/hxd/">HxD</a></td><td>Freeware</td><td>Bem bom. Possui recursos extras como geração de hashes e suporte a abrir discos.</td></tr><tr><td><a href="https://github.com/WerWolv/ImHex">ImHex</a></td><td>Livre</td><td>Impressionante editor gráfico para Windows, macOS e Linux. Possui uma linguagem de <em>patterns</em> para aplicar no arquivo, exporta/importa patches e muito mais.</td></tr><tr><td><a href="https://github.com/solemnwarning/rehex">Reverse Engineer's Hex Editor</a></td><td>Livre</td><td>Nova proposta de um editor especificamente para engenharia reversa.</td></tr><tr><td><a href="https://www.synalysis.net/">Synalyze It!</a></td><td>Comercial</td><td>Para macOS, desenvolvido por quem faz o Hexinator. Tem o mesmo nível de recursos.</td></tr><tr><td><a href="https://www.x-ways.net/winhex/">WinHex</a></td><td>Comercial</td><td>Editor com edições especiais focadas em forense, mas faz tudo que os outros fazem também. Somente para Windows.</td></tr><tr><td><a href="https://sourceforge.net/projects/wxhexeditor/">wxHexEditor</a></td><td>Livre</td><td>Multiplataforma, recursos interessantes. Infelizmente o desenvolvimento parou em 2017.</td></tr><tr><td><a href="http://www.chmaas.handshake.de/delphi/freeware/xvi32/xvi32.htm">XVI32</a></td><td>Freeware</td><td>Sem muitos recursos, mas quebra um galho no Windows para uma edição rápida.</td></tr></tbody></table>

## Analisadores Estáticos de Executáveis

Analisam estaticamente os binários, sem carregá-los. São úteis para uma primeira visão sobre um executável desconhecido.

<table><thead><tr><th width="269.3333333333333">Nome</th><th width="118">Licença</th><th>Descrição</th></tr></thead><tbody><tr><td><a href="https://github.com/mandiant/capa">capa</a></td><td>Livre</td><td>Detecta as capacidades de executáveis PE e shellcodes para Windows.</td></tr><tr><td><a href="https://ntcore.com/?page_id=388">CFF Explorer</a></td><td>Freeware</td><td>Sendo parte do Explorer Suite, é na real um editor de PE. Com ele é possível adicionar imports, remover seções, etc.</td></tr><tr><td><a href="https://horsicq.github.io">DIE (Detect It Easy)</a></td><td>Freeware</td><td>Detecta compilador, linker, packer e protectors em binários. Também edita os arquivos.</td></tr><tr><td><a href="learn.microsoft.com/en-us/cpp/build/reference/dumpbin-reference/">DUMPBIN</a></td><td>Freeware</td><td>Analisador de PE de linha de comando disponível no SDK do Visual Studio.</td></tr><tr><td><a href="https://hiew.ru/#edump">Edump</a></td><td>Freeware</td><td><em>Parser</em> de linha de comando para Windows. Suporta arquivos NE, LX/LE, PE/PE32+, ELF/ELF64 (little-endian), Mach-O (little-endian), e TE.</td></tr><tr><td><a href="http://www.exeinfo.o7.pl/">Exeinfo PE</a></td><td>Freeware</td><td>Detecta compilador, packer, protectors e edita os arquivos, além de suportar vários plugins loucos. Tem versão VIP mediante doação.</td></tr><tr><td><a href="https://github.com/alexandreborges/malwoverview">Malwoverview</a></td><td>Livre</td><td>Mais um nacional pra uma primeira impressão de arquivos suspeitos, URLs e domínios. Checa também APKs.</td></tr><tr><td><a href="https://www.gnu.org/software/binutils/">objdump</a></td><td>Livre</td><td>Parte do GNU binutils, também analisa PE, além de ELF, a.out, etc.</td></tr><tr><td><a href="https://hshrzd.wordpress.com/pe-bear/">PE-Bear</a></td><td>Freeware</td><td>Analisador gráfico (Qt) multiplataforma que também detecta packers/protectors.</td></tr><tr><td><a href="https://pedump.me">PEdump</a></td><td>Livre</td><td>Analisador online muito legal!</td></tr><tr><td><a href="https://www.winitor.com">pestudio</a></td><td>Freeware</td><td>Analisador de PE padrão da indústria, com foco em malware. Tem versão Pro (paga).</td></tr><tr><td><a href="https://www.gnu.org/software/binutils/">readelf</a></td><td>Livre</td><td>Também parte do binutils, analisador de ELF.</td></tr><tr><td><a href="https://github.com/mentebinaria/readpe">readpe</a></td><td>Livre</td><td>Anteriormente chamado de <strong>pev</strong>, este é o nosso 💚 toolkit de ferramentas de linha de comando para análise de PE. Artigo introdutório <a href="https://www.mentebinaria.com.br/artigos/estude-bin%C3%A1rios-de-windows-com-o-novo-pev-r18/">aqui</a>.</td></tr><tr><td><a href="http://www.cgsoftlabs.ro/studpe.html">Stud_PE</a></td><td>Freeware</td><td>Analisador e editor com suporte a plugins, assinaturas do antigo PEiD, editor de recursos e mais.</td></tr></tbody></table>

## Bibliotecas para _parsear_ Executáveis

<table><thead><tr><th width="271.3333333333333">Nome</th><th width="117">Licença</th><th>Descrição</th></tr></thead><tbody><tr><td><a href="https://ftp.gnu.org/pub/old-gnu/Manuals/bfd-2.9.1/html_chapter/bfd_1.html">BFD</a></td><td>Livre</td><td><strong>B</strong>inary <strong>F</strong>ile <strong>D</strong>escriptor é a biblioteca usada por programas como readelf e objdump. Tem suporte a muitos tipos de arquivos, incluindo PE e ELF, claro.</td></tr><tr><td><a href="https://github.com/merces/libpe">libpe</a></td><td>Livre</td><td>Nossa 💚 biblioteca multiplataforma para <em>parsing</em> de arquivos PE.</td></tr><tr><td><a href="https://github.com/hasherezade/libpeconv">libPeConv</a></td><td>Livre</td><td>Biblioteca em C++ para PE usada pelo PE-Bear.</td></tr><tr><td><a href="https://github.com/erocarrera/pefile">pefile</a></td><td>Livre</td><td>Famosa biblioteca em Python pra fazer qualquer coisa com arquivos PE.</td></tr><tr><td><a href="https://github.com/secana/PeNet">PeNet</a></td><td>Livre</td><td>Biblioteca em .Net para PE.</td></tr><tr><td><a href="https://github.com/dorkbox/PeParser">PeParser</a></td><td>Livre</td><td>Biblioteca em Java para PE.</td></tr><tr><td><a href="https://github.com/eliben/pyelftools">pyelftools</a></td><td>Livre</td><td>Biblioteca em Python para <em>parsear</em> binários ELF.</td></tr></tbody></table>

## Assemblers

<table><thead><tr><th>Nome</th><th width="143">Licença</th><th>Descrição</th></tr></thead><tbody><tr><td><a href="https://flatassembler.net/">flat assembler (FASM)</a></td><td>Livre</td><td>Assembler bem recente que já vem com vários exemplos de código. Windows e Linux.</td></tr><tr><td><a href="https://sourceware.org/binutils/docs/as/">GNU Assembler (GAS)</a></td><td>Livre</td><td>Também chamado simplesmente de <strong>as</strong>, é o assembler do projeto GNU e provavelmente já está instalado no seu Linux!</td></tr><tr><td><a href="https://learn.microsoft.com/en-us/cpp/assembler/masm/microsoft-macro-assembler-reference">Microsoft Macro Assembler (MASM)</a></td><td>Freeware</td><td>Atualmente já vem com o Visual Studio da Microsoft, mesmo na versão Community. Segue um <a href="https://www.mentebinaria.com.br/forums/topic/146-hello-world-em-masm-no-windows/">tutorial de como compilar um "Hello, world"</a>.</td></tr><tr><td><a href="https://www.nasm.us/">Netwide Assembler (NASM)</a></td><td>Livre</td><td>Multiplataforma, suporte à sintaxe Intel e bem popular. Veja um <a href="https://www.mentebinaria.com.br/forums/topic/51-%E2%80%9Chello-world%E2%80%9D-em-nasm-no-linux-x86/">tutorial de como compilar um "Hello, world" no Linux</a>.</td></tr><tr><td><a href="https://yasm.tortall.net">Yasm Modular Assembler (YASM)</a></td><td>Livre</td><td>Multiplataforma, escrito com base no NASM pra ser um substituto mas acho que não vingou. hehe</td></tr></tbody></table>

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
| [rr](https://rr-project.org/)                                                                         | Livre    | Debugger para Linux que implementa a "depuração reversa", isto é, você executa uma vez o alvo e o rr grava tudo. Depois você depura essa gravação.                                                                                                                                                                                     |
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

Abaixo um comparativo onde dumpamos os primeiros 32 bytes de um binário `/bin/ls` utilizando os visualizadores acima:

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
