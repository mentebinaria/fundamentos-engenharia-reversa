# Cabeçalhos

Cabeçalhos, como o próprio nome sugere, são áreas de dados no início de um arquivo. Eles contém diferentes campos, que admitem valores.

Cada campo possui um **tipo** que define seu tamanho. Por exemplo, se dissermos que o primeiro campo de um primeiro cabeçalho é do tipo WORD, estamos afirmando que este tem 2 _bytes_ de tamanho, conforme a tabela a seguir:

| Nomenclatura Microsoft | Nome do tipo em C (Microsoft Visual Studio) | Tamanho em _bytes_ |
| ---------------------- | ------------------------------------------- | ------------------ |
| BYTE                   | unsigned char                               | 1                  |
| WORD                   | unsigned short                              | 2                  |
| DWORD                  | unsigned long                               | 4                  |
| QWORD                  | \_\_int64                                   | 8                  |

Há também os campos que possuem o que chamamos de máscara de bits. Neste campos, cada _bit_ de seus _bytes_ podem significar alguma coisa se estiverem ligados. Um bom exemplo é o campo "Characteristics" do cabeçalho de seções do arquivo PE, que veremos oportunamente.
