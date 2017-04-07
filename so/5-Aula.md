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


Partições Múltiplas
- Começa com uma partição (toda a memória)

Unidade de alocação está para a memória assim como um setor está para um disco.

se UDA for 4k, então só posso criar uma compartição de 8k, 16k, 24k
se UDA for 2k, então só posso criar uma compartição de 4k, 6k, 8k

Exemplo:
1 partição com 64k
Unidade de alocação: 2k.

Entra um JOB1 com necessidade de 5k.

A ideia é criar uma partição suficiente pro JOB. 
Então dividimos em 2 partições:
- 1 de 6k
- outra de 58k

JOB1 fica na de 6k.

Vem JOB2 precisando de 10k.

Divide 58 em 2, tornando 3 partições:
6 - ocupada JOB1
10 - ocupada JOB2
48 - livre

No máximo um mapa de bits de 32 bits. (Com o máximo).
E uma outra informação com quantas partições eu tenho.


#T15

**1) Considere um sistema com partições variáveis de memória com unidades de alocação de 2Kb. Existe
uma tabela de jobs querendo espaço em memória e o job 1 e 2 já se encontram alocados, devendo sair
assim que terminarem suas execuções. Assume­se que a memória encontra­se como o apresentado na
figura 1 (não é relevante o que aconteceu antes que gerou esta situação). Faça a devida alocação dos
processos ao longo do tempo, desenhando uma figura equivalente sempre que ocorrerem mudanças na
memória (job entrou ou job terminou). A cada mudança, calcule a fragmentação externa e interna (se
houver). Técnica First­Fit. Para este exercício, considere que o desenho da figura representa o tempo
zero ­ T0 ­ e que ainda não está completo, ou seja, ainda devem ser alocados novos processos na
memória se houver recursos.** 
<Imagem>

						
				Job	Tam	Tempo
0k	    Monitor			Monitor	31kb	sempre
				Job1	55kb	10
				Job2	9kb	5
32k				Job3	17kb	5
	     Job3			Job4	93kb	10
50k						
						
64k	     Job 1			Tempo 0		
				Monitor	30kb	sempre
120k				Job1	55kb	10
				Job2	9kb	5
				Job3	17kb	5
200k	Job 2			Job4	93kb	10
210k						
						
				Fragmentação Interna		1 + ...
				Fragmentação externa		14+80+46
					
256k						



**2) Pense em como seria um FAT24. Considere que foram reservados 4 bits para contar quantos setores
tem cada bloco. Qual o tamanho máximo de cada cluster? Qual o tamanho máximo de disco que o
sistema suportaria? Qual o tamanho, em bytes, da tabela FAT?**

**3) O que é relocação de código? Porque é necessario? Porque não precisa disto na máquina pura?
Explique as técnicas estudadas até aqui para relocação.**
Realocação de código é quando um devido código precisa ter seu endereço na memória alterado. É necessário pois na maioria das vezes um programa não roda em máquina pura, ou seja, ele divide a memória com outros processos, assim, a realocação age impedindo que ocorra invasão de um programa em outra area que não lhe pertence. Na máquina pura não precisa pois a memória não precisa se preocupar com isso, afinal só vai servir a um serviço, podendo deixar o código compilado como foi gerado, ou seja, se utilizou uma variavel a na posição 0, ela pode ficar lá.
Estudamos até aqui 2 abordagens:

1 - Solução 100% feita pelo S.O.
2 - Usando algum recurso do Hardware

1.
Compilador parte sempre do endereço 0. No exemplo da variavel 'a', a variavel 'a' sempre seria a 0.
O S.O. carregaria para a memória, a memória só está livre a partir de 100, então a memória retorna essa informação.

Cada vez que o S.O. leva uma porção de código para a memória é somado 100 para ficar dentro da área disponível, como se ele recompilasse toda vez de levar o binário para a memória, instrução por instrução, somando os bits.

2.
Em execução, tradução de endereços, a partir do 8088, segmentação.
386 através da paginação.

**4) Explique porque o sistema de proteção de memória para partições de memória (fixa ou múltiplas) só
precisa de um registrador de limite para funcionar e não de dois (limite inferior e superior).**


