concorrente - nem todo concorrente é paralelo
paralela    - todo paralelo é concorrente. +1 de 1 processador é paralela
distribuida - rodando em maquinas diferentes, se é distribuido é paralelo e concorrente


## secao critica
exemplo do banheiro (o banheiro é a secao critica)
- ind. das velocidades
- progresso
- espera limitada
- exclusao mutua

post. indefinida (starvation): vao ficar um tempo esperando mas um dia vao conseguir
paralisacao (deadlock): necessita interrupcao do processo

Solucoes
spin-lock: SO usa internamente (n se usa para coisas demoradas, "qual é o proximo PID que eu quero incrementar" - coisas rapidas) 
semaforo: disponibilizada para todos SO
  nao se passa informacao pelo semaforo. Gerencia uma fila de processos

  P/DOWN/SLEEP/WAIT - antes da SC
  V/UP/WAKEUP/SIGNAL - depos da SC

2 TIPOS:
  - contadores
  - binarios (lock e unlock)


### arquiteuras:
- monolitica: cada componente ta dentro do SO, eficiente, comunicacao var globais
- em camadas: tecnica para usar em cima do monolitico, na pratica agrupa-se componentes em camadas, prejudica muito o desempenho mas é bom em organizacao do codigo.
- micronucleo: MINIX - ao contrario do monolitico, + facil de manter, comunicacao mensagens
- hibrida: ter desempenho melhor que monolitico e estabilidade do micronucleo

Exercicio:

```
Mostre como seria a execução dos processos descritos a seguir em sistemas operacionais com núcleos não interrompível,
interrompível não preemptivo e interrompível preemptivo, considerando: 3 (três) níveis de execução (aplicação executando
em modo usuário, sistema operacional executando em modo monitor, e entrada e saída em andamento); que, ao passar
de um ciclo de UCP (Unidade de Central de Processamento) para um ciclo de E/S (entrada e saída), gasta-se sempre 2
(duas) unidades de tempo para execução da respectiva chamada de sistema (que realiza a programação do respectivo
dispositivo de E/S); que, ao terminar uma operação de E/S, gasta-se sempre 2 (duas) unidades de tempo para gerenciar a
transferência de informações para o processo envolvido e, se for o caso, agendar: a próxima operação de E/S; e que, no
nível do usuário, o escalonamento é preemptivo, baseado em prioridades.
```
|Processo|TUCP, TE/S, TUCP, ...|Prioridade|
|-|-|-|
|A|4,3,2,2,2,2,2|+|
|B|4,3,3|-|

Nucleos:
   n interrompível
   interrompível nao preemptivo
   interrompível preemptivo
UCP (usuario) = processador


### não interrompível
| n interrompivel  | | | | | | | | |¹| | | | | | | |¹| | | |¹| | | | | | |¹| | | | | |
| ---------------- |-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|
| UCP (usuario)    |a|a|a|a| | |b|b|b| | |a|a| | |b| | | | |a| | |a| | |b|b| | |a|a|b|
| UCP (so)         | | | | |a|a| | | |a|a| | |a|a| |b|b|a|a| |b|b| |a|a| | |a|a| | | |
| E/S              | | | | | | |a|a|a| | | | | | |a|a| |b|b|b| | | | | |a|a| | | | | |

### interrompível nao preemptivo
> Ou seja, volta pro processo que estava executando.
| nao preemptivo   | | | | | | | | |¹| | | | | | | |¹| | | | | |¹| | | | |¹| | | | | |
| ---------------- |-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|
| UCP (usuario)    |a|a|a|a| | |b|b|b| | |a|a| | |b| | | | |a|a| | | | |b|b| | |a|a|b|
| UCP (so)         | | | | |a|a| | | |a|a| | |a|a| |b|a|a|b| | |a|b|b|a| | |a|a| | | |
| E/S              | | | | | | |a|a|a| | | | | | |a|a| | | |b|b|b| | | |a|a| | | | | |

### interrompivel preemptivo
> Mais rapido
| preemptivo       | | | | | | | | |¹| | | | | | | |¹| | | | | |¹| | | | |¹| | | | | |
| ---------------- |-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|
| UCP (usuario)    |a|a|a|a| | |b|b|b| | |a|a| | |b| | | |a|a| | | | | |a| | |a|b|b|b|
| UCP (so)         | | | | |a|a| | | |a|a| | |a|a| |b|a|a| | |a|b| |a|a| |b|b| | | | |
| E/S              | | | | | | |a|a|a| | | | | | |a|a| | | | | |a|a|b|b|b| | | | | | |

¹: interrupção hw
