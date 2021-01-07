# Cabeçalhos

Cabeçalhos, como o nome sugere, são áreas de dados no início de um arquivo. Basicamente são definidos por conjuntos de diferentes campos, que admitem valores.

Cada campo possui um **tipo** que também já define seu tamanho. Por exemplo, se dissermos que o primeiro campo de um primeiro cabeçalho é do tipo WORD, estamos afirmando que este tem 2 _bytes_ de tamanho, conforme a tabela a seguir:

| Nomenclatura Intel | Nome do tipo em C \(x86\) | Tamanho em _bytes_ |
| :--- | :--- | :--- |
| BYTE | char | 1 |
| WORD | short int | 2 |
| DWORD | int | 4 |
| QWORD | long int | 8 |

Há também os campos que possuem o que chamamos de máscara de bits. Neste campos, cada _bit_ de seus _bytes_ podem significar alguma coisa. Um bom exemplo é o campo "Characteristics" do cabeçalho de seções do arquivo PE, que veremos mais adiante.

