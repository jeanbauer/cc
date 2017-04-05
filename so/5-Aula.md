Gerência de memória

a = a + 20;
Compilado. 

LOAD A
ADD 20
STORE A

No intel seria uma instrução só: ADD A, 20
Isso é uma instrução de máquina.

Como funciona +- dentro do processador: (sopinha de bits)
Depois, a ULA vai pegar, exemplo, 011 e colocar no decodificador, quando for 011 vai para o circuito somador.

3 bits significaria apenas 8 instruções, por isso, é só um exemplo.

UPCODE - PARAMETROS
011
ADD

O grande problema é saber onde colocar a varável **a**. Fingindo que estamos numa máquina pura que podemos escolher o local da variável **a**, ficaria:

UPCODE - PARAMETROS
011    - 0000 0000 0001 0100 // isso é uma instrução.
// ADD    - instrução

Como resolve o problema de realocação de código?
1 - Solução 100% feita pelo S.O.
2 - Usando algum recurso do Hardware

1.
Compilador parte sempre do endereço 0. No exemplo da variavel a, a variavel a sempre seria a 0.
O S.O. carregaria para a memória, a memória só está livre a partir de 100, então a memória retorna essa informação.

Cada vez que o S.O. leva uma porção de código para a memória é somado 100 para ficar dentro da area disponivel, como se ele recompilasse toda vez de levar o binário para a memória, instrução por instrução, somando os bits.
Exemplo:
011 0110 0 1001 (100)

2.
Em execução, tradução de endereços, a paritr do 8088, segmentação.

386 através da paginação.


### Como o S.O. sabe quem tá onde, qual memória tá livre ou não, independente dos 2 itens acima.
> Na parte de gerenciamento de memória as técnicas são muito parecidas com alocação de arquivos.

Alocação Contigua de Memória
Um copy and paste de Alocação Contigua de Bloco, que era basicamente ao ocupar um disco, tem que ocupar um bloco do lado do outro.

Blocos de 8k, formando 64k.
[x][x][x][][][][][]

A unica coisa que muda é que bloco é chamada de PARTIÇÃO.
Igual na ACB, partições são potência de 2. 
Outra regra que só tem aqui, todas as partições devem ter tamanho fixo.

Escolha de espaço:
- first-fit
- best-fit
- worst-fit

Nesta técnica temos:
- fragmentação interna (e, assim como no disco, temos esse problema insolucionavel)
- fragmentação externa


Com ou sem a ajuda do Hardware, deve haver **proteção de memória**, ainda mais considerando que o compilador sempre vai gerar para máquina pura, ou seja sempre gerando a posição 0, pode haver conflito de memória, um programa tentando acessar uma área que não é dele.

Proteção de memória, somente com auxílio do HARDWARE, pois neste momento o S.O. está em standby.
Nas primeiras técnicas se utilizava somente um registrador de limite.
