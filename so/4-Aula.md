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
```São os bytes que sobram. Não existe solução para acabar com esse problema.```

**Fragmentação externa**
```
só acontece na alocação contigua. Não importa o tam. do arquivo, ele deverá ir para o disco de forma sequencial.

(cada posição tem 2k de espaço)
[0][1][2][3][4][5][6][7][8]
2k--2k-2k-2k-2k- ...

Se o arquivo (teste.txt) tem 3k (tamanho) ele poderia ir na posição 2 e 3, fragmentação interna de 1k - ou seja sobrou 1k.

Porém, precisamos agora alocar um arquivo de 10k, ele precisa de 10k livre sequencialmente, como não tem, se dá a fragmentação externa.
```

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

NOME ARQUIVO | ONDE COMEÇA | TAMANHO
-------------------------------------


Temos que dizer uma tabela maior com todos os detalhes. Existem 2 abordagens:
- Encadeamento (FAT)
- Indexação (NTFS) - tem uma lista de posições: 1,2,3,4...
```
NOME ARQUIVO | ONDE COMEÇA | TAMANHO
-------------------------------------
Teste.txt    -    9k       - 1,2,3,4...
```

Alocação Encadeada:


```

NOME ARQUIVO | ONDE COMEÇA | TAMANHO
-------------------------------------
Teste.txt    -    9k       -    5

Bloco
  0     1   2   3   4   5   6   7  
|----------------------------------
|  3  |	  |	7 |	3 |	  |	2 |	  |	0 |	
|-2k---2k--2k--2k--2k--2k--2k--2k--

Como ler isso?
Sabemos que o arquivo começa no 5, o que tem no 5? r: 2. O que significa isso?
Significa que a continuação está na posição 2. Lá vai ter um 7, então seguimos o 7 e descobrimos que o próximo é o 0.
Em 0, temos 3, em 3 temos 3 denovo, isso é um jeito de falar que terminou a cadeia.

Pequeno probleminha nesta técnica:
perde-se 3 bits para anotar o próximo número na cadeia.
-1 para indicar o fim, ou o mesmo numero do indice, caso não suporte numero negativo. (3)

Isso acaba com o problema de fragmentação externa.

Mapa de bits do exemplo acima. (utilizando 8 bits)

  0     1   2   3   4   5   6   7  
|----------------------------------
|  1  |	0 |	1 |	1 |	0 |	1 |	0 |	1 |	
|----------------------------------

1 - Ocupado
0 - Livre

O mapa de bits só não foi deixado de lado por motivos de que podem haver lixos de memória, arquivos apagados.

```

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





