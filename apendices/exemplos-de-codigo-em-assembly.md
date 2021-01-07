# Exemplos de código em Assembly

Reuno aqui alguns exemplos de códigos em Assembly, úteis para a compreensão de trechos de binários quando fazemos engenharia reversa.

## Zerar variáveis

{% tabs %}
{% tab title="Assembly" %}
```text
xor eax, eax
```
{% endtab %}

{% tab title="C" %}
```c
int eax=0;
```
{% endtab %}
{% endtabs %}

## Contagem de 1 a 10

{% tabs %}
{% tab title="Assembly v1" %}
```text
xor ecx, ecx
loop:
  inc ecx
  cmp ecx, 0xa
  jl loop
```
{% endtab %}

{% tab title="Assembly v2" %}
```text
mov ecx, 0
loop:
  add ecx, 1
  cmp ecx, 0x9
  jle loop
```
{% endtab %}

{% tab title="C" %}
```c
int ecx;
for (ecx=0; ecx<10; ecx++) {}
```
{% endtab %}
{% endtabs %}

## Testar se é zero

{% tabs %}
{% tab title="Assembly v1" %}
```text
cmp eax, 0
je destino
```
{% endtab %}

{% tab title="Assembly v2" %}
```text
test eax, eax
je destino
```
{% endtab %}

{% tab title="C" %}
```text
if (eax == 0)
    // destino
```
{% endtab %}
{% endtabs %}

## Não fazer nada

Parece bobo, mas "fazer nada" corretamente significa não alterar nenhuma _flag_, nem nenhum registrador. Seguem as instruções que conheço:

{% tabs %}
{% tab title="Assembly v1" %}
```text
xchg eax, eax
```
{% endtab %}

{% tab title="Assembly v2" %}
```text
nop
```
{% endtab %}

{% tab title="C" %}
```text
;
```
{% endtab %}
{% endtabs %}

Instruções que não fazem nada também podem ser utilizadas como _padding_ necessário para o correto alinhamento das seções do binário em memória. Já vi o GCC utilizar XCHG AX, AX neste caso.

