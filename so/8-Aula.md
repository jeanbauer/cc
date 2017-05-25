24/05
Correção do T21

Hoje é:
quantidade de paginas = quantidade de frames

Nº de página: 13 bits
Cada processo tem uma tabela de páginas, que funciona com a seguinte entrada:

--

Como acontece?
Um programa tem 3 areas:
- Dados
  - Variaveis globais, malloc
- Codigo
  - Funcoes
- Pilha
  - Atribuição de função, etc

Isso é visto em segmentação também.

Se o codigo inteiro tem 100 bytes ele ocupa 1 pagina e disperdiça 28 por fragmentação interna.
Compilado (.exe) consume no minimo 3 paginas, 1 pra dados, 1 pra codigo e 1 pra pilha.

Digamos que: (dentro do .exe no cabeçalho do executavel - essas info servem o S.O. - o compilador que faz¹).

Pilha 200 bytes
Codigo 500 bytes
Dados 200 bytes

Cada pag = 128 bytes.

**P1**
PAG | FRAME | Valido ou Invalido
---------------------------------
13 bits |  13 bits  |  1 bit
---------------------------------
(0000000000000) dados p0 |  8  | 1
dados p1 |  13 | 1
pilha p2 |   5 | 1
codigo p3 |  5 | 1
codigo p4 |  3 | 1
codigo p5 |  4 | 1
codigo p6 |  11 | 1

!!!!!!!!!!!!!!!!!!!! perguntar para o Elgio como se deu as escolhas dos frames. !!!!!!!!!!!

Depois de ajustar os indices de pagina o S.O. coloca o Frame e se é valido ou nao.

¹ Por isso as vezes o programa nem começa a executar e o SO ja diz que nao tem memoria para executá-lo.

Criou o main.c:
```
int main(){
  variaveisEstaticas	
};
```    

variaveis automaticas sao criadas na pilha.

______ Paginação lógica

4 bits           9 bits            7 bits 
[] -------------- [] --------------- []
<- mais         pagina          deslocamento
significativo

Cada processo tem uma tabela só.
p1 pode ter 512 páginas.





______ Memória Virtual

[23 bits - Logico][7 bits - fisico]
Ou seja, minto para o meu processo, "Eu (S.O.) pode ir que tem 4gb".

_P.S. Está meio em desuso ultimamente. Temos 4gb de RAM. _

Quantidade de Frames > Quantidade de páginas

```
1. int main(){
2.   for ( ... ) { }
3.   vet = malloc(1kk) // aprox. 4mb
4. };
```   

Digamos que seja um programa bem extenso. Ele executando ocuparia 10 páginas.
Quando eu fiz o malloc da linha 3 ele consumiu 200 paginas. Mas o programa não precisa de tantas paginas o tempo todo, por isso podemos fazer store dela no disco e só buscar quando necessário.
Quando precisar da pagina que tá no disco, o S.O. pega do disco e traz para a memória. Se não tiver ele já manda pro disco.

Agora a tabela de páginas será diferente, teremos:

Memória virtual só é possível com auxilio de Hardware.

o S.O. alimentou:
**P1**
PAG | FRAME | Valido ou Invalido
---------------------------------
0 |  5 | 1
1 |  3 | 1
2 |  8 | 0
3 |  5 | 0
4 |  10 | 1

0 em Valido ou Invalido significa que não está na memória, ai o S.O. vai carregar.

Caso ele tente acessar a pag 5 ai realmente não vai ter, é invalido, o S.O. que vai saber.


Frame na memória.

E se todos os frames estiverem ocupados?
Ai o S.O. tem a incubencia de escolher uma pagina como vitima, que vai deixar a memoria e ir para o disco. :scream:

:dart: Como ele faz essa escolha?!
Há vaaaarias maneiras.
Mas a melhor seria uma pagina que nao será mais usada. Para descobrir isso, usamos algoritmos de aproximação.

Quanto menor o numero de page fault melhor é o algorimo.

Digamos que nós temos uma memória de 3 Frames.

```
Ordem:          -> futuro
[4][5][6][8]...  [5][6][8][9][10]
-----------------------------
FR 0 | 4 |  4 |  4  | 8¹| 8 | 8 | 8 | 8 | 8
FR 1 |   |  5 |  5  | 5 | 5 | 5 | 5 | 9 | 9
FR 2 |   |    |  6  | 6 | 6 | 6 | 6 | 6 |10
       X    X    X    X               X   X

¹ A pagina 8 entrou no lugar do 4.
```

Vamos ver como o FIFO se sai:
```
Ordem:          
[4][5][6][8][5][6][8][9][10]
-----------------------------
FR 0 | 4 |  4 |  4  | 8 | 8 | 8 | 8 | 8
FR 1 |   |  5 |  5  | 5 | 5 | 5 | 9 | 9
FR 2 |   |    |  6  | 6 | 6 | 6 | 6 | 10
       X    X    X    X           X    X
```
