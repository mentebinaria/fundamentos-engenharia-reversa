# Exemplos de código em Assembly

Reuno aqui alguns exemplos de códigos em Assembly, úteis para a compreensão de trechos de binários quando fazemos engenharia reversa.

## Zerar variáveis

{% code-tabs %}
{% code-tabs-item title="Assembly" %}
```text
xor eax, eax
```
{% endcode-tabs-item %}

{% code-tabs-item title="C" %}
```c
int eax=0;
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Contagem de 1 a 10

{% code-tabs %}
{% code-tabs-item title="Assembly v1" %}
```text
xor ecx, ecx
loop:
  inc ecx
  cmp ecx, 0xa
  jl loop
```
{% endcode-tabs-item %}

{% code-tabs-item title="Assembly v2" %}
```
mov ecx, 0
loop:
  add ecx, 1
  cmp ecx, 0x9
  jle loop
```
{% endcode-tabs-item %}

{% code-tabs-item title="C" %}
```c
int ecx;
for (ecx=0; ecx<10; ecx++) {}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Testar se é zero

{% code-tabs %}
{% code-tabs-item title="Assembly v1" %}
```text
cmp eax, 0
je destino
```
{% endcode-tabs-item %}

{% code-tabs-item title="Assembly v2" %}
```
test eax, eax
je destino
```
{% endcode-tabs-item %}

{% code-tabs-item title="C" %}
```
if (eax == 0)
    // destino
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Não fazer nada

Parece bobo, mas "fazer nada" corretamente significa não alterar nenhuma _flag_, nem nenhum registrador. Seguem as instruções que conheço:

{% code-tabs %}
{% code-tabs-item title="Assembly v1" %}
```text
xchg eax, eax
```
{% endcode-tabs-item %}

{% code-tabs-item title="Assembly v2" %}
```
nop
```
{% endcode-tabs-item %}

{% code-tabs-item title="C" %}
```
;
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Instruções que não fazem nada também podem ser utilizadas como _padding_ necessário para o correto alinhamento das seções do binário em memória. Já vi o GCC utilizar XCHG AX, AX neste caso.
