# Exemplos de Código em Assembly

Reuni aqui alguns exemplos de códigos em Assembly, úteis para a compreensão de trechos de binários quando fazemos engenharia reversa.

## Zerar Variáveis

```
xor eax, eax
```

## Contar de Um a Dez

```
xor ecx, ecx
loop:
  inc ecx
  cmp ecx, 0xa
  jl loop
```

Outra versão:

```
mov ecx, 0
loop:
  add ecx, 1
  cmp ecx, 0x9
  jle loop
```

## Testar Se É Zero

```
cmp eax, 0
je destino
```

Outra versão:

```
test eax, eax
je destino
```

## Fazer Nada

Parece bobo, mas "fazer nada" corretamente significa não alterar nenhuma _flag_, nem nenhum registrador. A instrução em Assembly Intel mais famosa para tal é a NOP (NO Operation):

```
nop
```

Mas também é possível atingir o mesmo resultado com instruções como a XCHG (eXCHanGe). Por exemplo, se você trocar o valor do registrador EAX com ele mesmo, acaba por não fazer "nada":

```
xchg eax, eax
```

Instruções que não fazem nada também podem ser utilizadas como _padding_ necessário para o correto alinhamento das seções do binário em memória. Já vi o GCC utilizar XCHG AX, AX neste caso.
