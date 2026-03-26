# 🥇 Introdução

## O que é Engenharia Reversa

Em termos amplos, engenharia reversa é o processo de entender como algo funciona através da análise de sua estrutura e de seu comportamento. É uma arte que permite conhecer como um sistema foi pensado ou desenhado sem ter contato com o projeto original.

No campo militar e em contexto de guerras é comum assegurar que inimigos não tenham acesso às armas avançadas: aviões, tanques e outros dispositivos, por ser importante que adversários não desmontem esses equipamentos, entendam seu funcionamento e, consequentemente, que criem versões superiores deles ou encontrem falhas que lhes permitam inutilizá-los ou contorná-los com mais facilidade. Na prática, é evitar a engenharia reversa.

> Vale a pena assistir ao filme Jogo da Imitação (_Imitation Game_) que conta a história do criptoanalisa inglês Alan Turing, conhecido como o pai da ciência de computação teórica, que quebrou a criptografia da máquina nazista Enigma utilizando engenharia reversa.

## Engenharia Reversa de Software

Este livro foca na engenharia reversa de software, ou seja, no processo de entender como uma ou mais partes de um programa funcionam, sem ter acesso a seu código-fonte. Focaremos inicialmente em programas para a plataforma x86 (de 32-bits), rodando sobre o sistema operacional Windows, da Microsoft, mas vários dos conhecimentos expressos aqui podem ser úteis para engenharia reversa de software em outros sistemas operacionais, como o GNU/Linux e até mesmo em outras plataformas, como ARM.

## Por que a Engenharia Reversa de Software é possível

Assim como o _hardware, o software_ também pode ser desmontado. De fato, existe uma categoria especial de _softwares_ com esta função chamados de _disassemblers_, ou desmontadores. Para explicar como isso é possível, primeiro é preciso entender como um aplicativo é criado atualmente. Farei um resumo aqui, mas entenderemos mais a fundo em breve.

A parte do computador que de fato executa os programas, é o chamado processador. Nos computadores de mesa (_desktops_) e _laptops_ atuais, normalmente é possível encontrar processadores fabricados pela Intel ou AMD. Para ser compreendido por um processador, um programa precisa falar sua língua: a **linguagem (ou código) de máquina**.

Os humanos, em teoria, não falam em linguagem de máquina. Bem, alguns falam, mas isso é outra história. Acontece que, para facilitar a criação de programas, algumas boas almas começaram a escrever programas onde humanos escreviam código (instruções para o processador) numa linguagem mais próxima da falada por eles (inglês no caso). Assim nasceram os primeiros **compiladores**, que podemos entender como programas que "traduzem" códigos em linguagens como **Assembly** ou **C** para código de máquina.

Um programa é então uma série de instruções em código de máquina. Quem consegue olhar para ele desta forma, consegue entender sua lógica, mesmo sem ter acesso ao código-fonte que o gerou. Isso vale para praticamente qualquer tipo de programa, seja ele criado em linguagens compiladas como C, C++, Pascal, Delphi, Visual Basic, D, Go e até mesmo em linguagens interpretadas como Python, Ruby, Perl ou PHP. Lembre-se de que o processador só entende código de máquina e para ele não importa qual é o código-fonte, ou se a linguagem é compilada (análise e execução separadas) ou interpretada (análise e execução juntas). Então, é importante notar que, para o processador poder executar, "tudo tem que acabar em linguagem de máquina". Qualquer um que a conheça poderá inferir qual lógica o programa possui. Aliado ao conhecimento do ambiente de execução, é possível inclusive descrever exatamente o que um programa faz e, nisto, está a arte da engenharia reversa de software que você vai aprender neste livro.

## Áreas de aplicação da Engenharia Reversa de Software

### Análise de Malware

Naturalmente, os criadores de programas maliciosos não costumam compartilhar seus códigos-fonte com as empresas de segurança de informação. Sendo assim, analistas que trabalham nessas empresas ou mesmo pesquisadores independentes podem lançar mão da engenharia reversa a fim de entender como essas ameaças digitais funcionam e então poder criar suas defesas.

### Análise de Vulnerabilidade

Alguns _bugs_ encontrados em _software_ podem ser exploráveis por outros programas. Por exemplo, uma falha no componente SMB do Windows permitiu que a NSA desenvolvesse um programa que dava acesso a qualquer computador com o componente exposto na internet. Para encontrar tal vulnerabilidade, especialistas precisam conhecer sobre engenharia reversa, dentre outras áreas.

### Correção de _Bugs_

Às vezes um _software_ tem um problema e, por algum motivo, você ou sua empresa não possuem mais o código-fonte para repará-lo ou o contrato com o fornecedor que desenvolveu a aplicação foi encerrado. Com engenharia reversa, deve ser possível corrigir tal problema.

### Mudança e Adição de Recursos

Mesmo sem ter o código-fonte, é possível também alterar a maneira como um programa se comporta. Por exemplo, um programa que salva suas configurações num diretório específico pode ser instruído a salvá-las num compartilhamento de rede.

Adicionar um recurso é, em geral, trabalhoso, mas possível.

### (Anti-)pirataria

_Software_ proprietário costuma vir protegido contra pirataria. Você já deve ter visto programas que pedem número de série, chave de registro, etc. Com engenharia reversa, os chamados _crackers_ conseguem quebrar essas proteções. Por outro lado, saber como isso é feito é útil para programadores protegerem melhor seus programas.

### Reimplementação de Software e Protocolos

Um bom exemplo de uso da engenharia reversa é o caso da equipe que desenvolve o LibreOffice: mesmo sem ter acesso ao código-fonte, eles precisam entender como o Microsoft Office funciona, a fim de que os documentos criados nos dois produtos sejam compatíveis. Outros bons exemplos incluem:

- **Wine**, capaz de rodar programas feitos para Windows no GNU/Linux;
- **Samba**, que permite que o GNU/Linux e o Windows troquem arquivos via rede;
- **Pidgin**, que conecta numa série de protocolos de mensagem instantânea;
- **ReactOS**, um sistema operacional que lhe permite executar seus aplicativos e drivers favoritos do Windows em um ambiente de código aberto e gratuito.

Estes são exemplos de implementações em software livre, que tiveram de ser criadas a partir da engenharia reversa feita em programas e/ou protocolos de rede proprietários.
