# Aula 4

Resumo:
Gerência de disco.
O que é um setor?
Qual é o tamanho?

Setor físico: 512 bytes
Setor lógico: bloco

Bloco são vários setores, pode ser no mínimo 1k.
1k = 2 setores  \
2k = 4 setores   | -> gerenciável.
4k = 8 setores  /

São duas coisas que o S.O. precisa fazer para gerenciar arquivo:
- Nome (teste.txt)
- Onde se encontra (bloco x9080100)

**Fragmentação interna** 
> São os bytes que sobram. Não existe solução para acabar com esse problema.

**Fragmentação externa**
> só acontece na alocação contigua. Não importa o tam. do arquivo, ele deverá ir para o disco de forma sequencial.

```
(cada posição tem 2k de espaço)
[0][1][2][3][4][5][6][7][8]
2k--2k-2k-2k-2k- ...
```
Se o arquivo (teste.txt) tem 3k (tamanho) ele poderia ir na posição 2 e 3, fragmentação interna de 1k - ou seja sobrou 1k.

Porém, precisamos agora alocar um arquivo de 10k, ele precisa de 10k livre sequencialmente, como não tem, se dá a fragmentação externa.


S.O. gerencia
- tabela de nomes de arquivo.
- também gerencia blocos livres x ocupados, usando a técnica de mapa de Bits, serve apenas para marcar livre ou ocupado. Exemplo: Se eu tenho 8 bits eu só consigo marcar 8 blocos.
Se eu tenho um disco gerenciável de 16k e cap. máxima de 8 blocos.

Sempre potencia de 2. Não existe bloco de 5k. Não pode ter 3 setores.
Quanto maior for o meu disco:

128k de discos com 8 bit de mapa, blocos de qual tamanho? 16k.

Problema quando os blocos sao maiores -> aumenta a fragmentação interna. Desperdicio.

Por isso que alguns S.O. (FAT) pode-se aumentar o tamanho do bloco.

MAPA DE BITS:
Marca blocos livres ou ocupados.
Quanto maior o nº de bits melhoro a fragmentação interna.

O ideal é trabalhar com 512 bytes (limite físico).

Então podemos aumentar o mapa de bits permitindo reduzir o tamanho do bloco para o mesmo disco.




--

Agora o arquivo vai poder ocupar o bloco que quiser.
O problema é: Como vamos modelar isso em formato de tabela? Utilizando lista encadeada? ...

Agora, não basta mais dizer:

| Nome Arq.     | Onde começa   | Tamanho  |
| ------------- |:-------------:| --------:|


Temos que dizer uma tabela maior com todos os detalhes. Existem 2 abordagens:
- Encadeamento (FAT)
- Indexação (NTFS) - tem uma lista de posições: 1,2,3,4...

| Nome Arq.     | Onde começa   | Tamanho  |
| ------------- |:-------------:| --------:|
| Teste.txt     | 	9k 	 		| 1,2,3,4  |



Alocação Encadeada:




| Nome Arq.     | Onde começa   | Tamanho  |
| ------------- |:-------------:| --------:|
| Teste.txt     | 	9k 	 		| 5        |

```
Bloco
__0_____1___2___3___4___5___6___7__ 
|----------------------------------
|  3  |	  |	7 |	3 |	  |	2 |	  |	0 |	
|-2k---2k--2k--2k--2k--2k--2k--2k--
```

Como ler isso?
Sabemos que o arquivo começa no 5, o que tem no 5? r: 2. O que significa isso?
Significa que a continuação está na posição 2. Lá vai ter um 7, então seguimos o 7 e descobrimos que o próximo é o 0.
Em 0, temos 3, em 3 temos 3 denovo, isso é um jeito de falar que terminou a cadeia.

Pequeno probleminha nesta técnica:
perde-se 3 bits para anotar o próximo número na cadeia.
-1 para indicar o fim, ou o mesmo numero do indice, caso não suporte numero negativo. (3)

Isso acaba com o problema de fragmentação externa.

Mapa de bits do exemplo acima. (utilizando 8 bits)

__0_____1___2___3___4___5___6___7__ 
|----------------------------------
|  1  |	0 |	1 |	1 |	0 |	1 |	0 |	1 |	
|----------------------------------

1 - Ocupado
0 - Livre

O mapa de bits só não foi deixado de lado por motivos de que podem haver lixos de memória, arquivos apagados.



Desta forma tradicional, os 3 bits para marcar "encomodaram", então foi proposto uma adaptação deste modelo:

#### Alocação encadeada com índice
> Procurou minimizar os danos dos 3 bits.

Basicamente fizeram uma tabela auxiliar (menor) só com nº de blocos e para qual bit vai a informação.
(Na foto são as cetas roxas).

Um problema é: Como chegar no 3 encadeamento? 
Mesma resposta de qualquer lista encadeada, tenho que visitar todos os blocos até chegar nele. Acesso sequencial. (Parece uma fita). 

O disco era aleatório e agora virou sequencial?! Parece retrocesso.

Como resolver isso?

#### Alocação encadeada com índice
> Volto a ter acesso aleatório

Trago aqueles 3 bits de informação para a memória, não perdendo um único bit no bloco para encadear.




IT'S FAT TIME!
Let's talk about FAT.

> file alocation table
- DOS
- WINDOWS

FAT12, FAT16, FAT32 tem a seguinte métrica: Uma tabela indexada de blocos.

## FAT 12
> Pois tem 12 bits para a tabela. 12 bits dá 4096 bytes. 2¹² (4096) blocos no máximo.

Com esses parâmetros (blocos de 2k + fat12 posso gerenciar um disco de 8 mega).

```
Blocos:
0 - Não pode ser alocado, uso exclusivo do S.O.
1 - Não pode ser alocado, uso exclusivo do S.O.
2 - 5
3 - 0
4 - 1
5 - 3
6 - 1
7 - 1
...
até 4095 (pois é 12 bits) <- isso é o máximo que posso ter. Ou seja, posso ter menos. 8 blocos apenas e o resto é nulo, sem uso.
```

| Nome Arq.     | Onde começa   | Tamanho  |
| ------------- |:-------------:| --------:|
| Teste.txt     | 	2 	 		|  6k      |


Agora vamos adicionar mais um arquivo ao nosso disco, o Abc.txt:

| Nome Arq.     | Onde começa   | Tamanho  |
| ------------- |:-------------:| --------:|
| Teste.txt     | 	2 	 		|  6k      |
| Abc.txt       | 	4 <- 	 	|  3k      |


Nossos blocos ficariam:

Blocos:
0 - Não pode ser alocado, uso exclusivo do S.O.
1 - Não pode ser alocado, uso exclusivo do S.O.
2 - 5
3 - 0
4 - 6 <- novo
5 - 3
6 - 0 <- novo
7 - 1



Para indicar o fim do encadeamento podemos apontar para a posição 0, afinal seria uma posição inválida uma vez que ela é de uso do S.O. apenas. Apontar p/ o bloco 1 significa que está livre.
- Pos 0: EOF, end of file.
- Pos 1: Posição livre.

Não temos mais mapa de bits. Abaixo, a representação do disco na horizontal:
```
__0_____1___2___3___4___5___6___7__ 
|    							  |
|     |	  |	  |	  |	  |	  |	  |	  |	16kbytes disco
|----------------------------------
__2k___2k__2k__2k__2k__2k__2k__2k__ 
```

Windows chama cada pedaço de cluster ao invés de bloco.
Leia-se um cluster de 2k, são 


(Trocar essa linha p/ foto do quadro com FAT12)


Um pouco de história:
Primeira versão gerenciava 4096 setores. 4096 x 512 bytes = 2kbytes era o tam. máximo.
Segunda versão: Permitiu que se agrupassem setores, se tornando clusters. Poderia ter bloco de: 1k, 2k, 4k ou 8k apenas. Discos de até 4096 (4k) blocos * 8k (tam. máximo de cada bloco) = 32 mb de limite.


## FAT 16

- 2 na 16 = 65536.
Mas na verdade era 65252, pois a FAT16 reservou muitos números (livres, ocupados, etc).

65252 (64k) * 32k (Máximo tamanho de bloco) = 2gb

Mesmo as mais novas pen-drive não usam mais FAT16. (A maioria no mercado tem minimo de 4gb de espaço).

Misteriosamente o padrão da FAT no Windows NT se permitia que cada cluster fosse até 256k.

Windows NT chegava a 16 gb. (64k * 256k)

## FAT 32
> 4 bits reservados para marcação
- 2 na 28 blocos = 
Tamanho máximo: 2 tera
Tamanho máximo deveria ser: (2 na 28 * 32k) = 8 tera, porém há um limite. 32 bits só para enumerar setores, ou seja, até 4 giga setores, cada setor é 512 bytes, então 4G * 512bytes = 2 tera. 1º Caso onde o resultado do tamanho máximo não é o cálculo de blocos * maior número de bloco (32k).

Blocos de: 1k, 2k, 4k, 8k, 16k ou 32k.

