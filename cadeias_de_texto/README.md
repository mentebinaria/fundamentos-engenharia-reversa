# Cadeias de texto

Se o computador só entende números, como podemos trabalhar com texto então? Bem, não se engane, o computador realmente só entende números. O fato de você apertar uma tecla no teclado que tem o desenho de um símbolo do alfabeto utilizado no seu país não garante que é isto que de fato seja enviado para o computador e, naturalmente, não é. Ao invés disso, cada tecla possui um código conhecido como _scan code_ ou _make code_ que é enviado, entre outras informações, pelo barramento de dados do teclado para a placa-mãe do computador e passa por vários estágios até chegar ao _kernel_, o núcleo do sistema operacional. Por exemplo, de acordo com informação contida no site [Beyond Logic](http://retired.beyondlogic.org/keyboard/keybrd.htm), o _scan code_ da tecla "A" é o byte 0x1c.

Após receber o _scan code_ da tecla e outros dados, o _driver_ pode adicionar ou modificar valores. É o que acontece no Linux, por exemplo. Com o comando abaixo é possível por exemplo analisar os _bytes_ gerados no dispositivo do teclado quando a tecla "A" é pressionada:

```text
# hd /dev/input/event0
00000000  f1 e7 6e 59 00 00 00 00  6d e6 04 00 00 00 00 00  |..nY....m.......|
00000010  04 00 04 00 1e 00 00 00  f1 e7 6e 59 00 00 00 00  |..........nY....|
00000020  6d e6 04 00 00 00 00 00  01 00 1e 00 01 00 00 00  |m...............|
00000030  f1 e7 6e 59 00 00 00 00  6d e6 04 00 00 00 00 00  |..nY....m.......|
00000040  00 00 00 00 00 00 00 00  f1 e7 6e 59 00 00 00 00  |..........nY....|
00000050  8b 3b 06 00 00 00 00 00  04 00 04 00 1e 00 00 00  |.;..............|
00000060  f1 e7 6e 59 00 00 00 00  8b 3b 06 00 00 00 00 00  |..nY.....;......|
00000070  01 00 1e 00 00 00 00 00  f1 e7 6e 59 00 00 00 00  |..........nY....|
00000080  8b 3b 06 00 00 00 00 00  00 00 00 00 00 00 00 00  |.;..............|
00000090
```

O simples pressionar de uma tecla gerou 144 \(0x90\) _bytes_ de dados. Esta é uma especificidade do Linux, que define vários tipos de eventos, cada um com 24 _bytes_ de tamanho. O utilitário **evtest** interpreta os _bytes_ acima e produz uma saída mais amigável ao pressionarmos a mesma tecla "A":

```text
# evtest /dev/input/event0
Event: time 1500442813.544820, type 4 (EV_MSC), code 4 (MSC_SCAN), value 1e
Event: time 1500442813.544820, type 1 (EV_KEY), code 30 (KEY_A), value 1
Event: time 1500442813.544820, -------------- EV_SYN ------------
Event: time 1500442813.610261, type 4 (EV_MSC), code 4 (MSC_SCAN), value 1e
Event: time 1500442813.610261, type 1 (EV_KEY), code 30 (KEY_A), value 0
Event: time 1500442813.610261, -------------- EV_SYN ------------
```

Perceba os 6 eventos. Se quiser saber mais sobre como o _kernel_ Linux padroniza e transforma os _scan codes_ enviados pelo teclado para estes valores, basta ler a [documentação](https://github.com/torvalds/linux/blob/master/Documentation/input/event-codes.rst), mas este não é nosso assunto alvo aqui.

Assim como na entrada de dados pelo teclado, o tratamento da entrada do _mouse_ ou de qualquer outro dispositivo também é numérico e, de maneira geral, o computador necessita entender que ao ler um determinado número, precisa tomar alguma ação, como desenhar o que conhecemos por caractere "a". Para ele é um número, para nós, um símbolo. Veremos nas seções a seguir como essa conversão se dá.
