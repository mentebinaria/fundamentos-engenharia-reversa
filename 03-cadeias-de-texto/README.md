# 🧵 Cadeias de Texto

Se o computador só entende números, como podemos trabalhar com texto então? Bem, não se engane, o computador realmente só entende números. O fato de você apertar uma tecla no teclado que tem o desenho de um símbolo do alfabeto utilizado no seu país não garante que é isto que de fato seja enviado para o computador e, certamente, não é. Ao invés disso, cada tecla possui um código conhecido como _scan code_ ou _make code_ que é enviado, entre outras informações, pelo fio teclado para a placa-mãe do computador e passa por vários estágios até chegar ao _kernel_, o núcleo do sistema operacional. Se sua intenção for escrever o texto "a" num editor de textos (no Bloco de Notas por exemplo), então uma tabela de conversão entra em jogo. Essa tabela vai armazenar no arquivo de texto que você está criando um ou mais **números** que são equivalentes ao caractere "a". Ou seja, o **texto** "a", na prática, não existe.

Esses números têm um nome: pontos de código ou, na sua forma mais conhecida em inglês, _code points_.

## Code points

Um ponto de código (do inglês _code point_ ou _codepoint_) é um número que faz o caractere. Por exemplo, o número 97 é o _code point_ que faz o caractere "a" minúsculo codificação ASCII, que veremos a seguir.
