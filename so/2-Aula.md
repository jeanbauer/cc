# Aula 2

Memória, Von neuman
Máquina Pura: Memória divida em parte lógica e dados do programa.

Máquina pura: Quando a memória está 100% disponível para o meu programa.
```
---------------|
|			   |
|  Dados P1    |
|			   |
|  			   |
|---------------
|			   |
|  Código P1   |
|			   |
|---------------
```

_Ainda assim é uma máquina pura._


### Monitor Residente
> 'Primeiro' S.O.

```
---------------|
|			   |
|  Monitor     |
|  Residente   |
|  			   |
|---------------
|			   |
|      P1      |
|			   |
|---------------
|			   |
|      P2      |
|			   |
|---------------
```

- Gerencia a memória
- Carrega outros processos
- Evitar a ociosidade da CPU
  Como ele faz? Quando ela está ociosa ele poe outro processo em execução. IO poe outro processo em exec.
- É a primeira vez.

Supondo que o código comece na posição 1000 na memória.
```
a = 0; // 'a' estará na pos 1000.
a = a + b; // 'b' estará na pos 1001.

// significa
[1000] = [1000] + [1001]
```

Realocação de código
- Se um endereço já estiver em uso na memória
- Onde está o monitor residente?

```
---------------|---------  POSIÇÕES ------|
|			   | 0
|  Monitor     | ...
|  Residente   | ...
|  			   | 1000	 
|------------------------------------------ Contagem de memória Relativa para os processos, começa do 0.
|			   | a = 0;     // [500] = 0;
|      P1      | b = 50;    // [501] = 50;
|			   | c = a + b; // [502] = [500] + [501];
|---------------
|			   |
|      P2      |
|			   |
|---------------
```

No primeiro momento, o P1 determina que a variavel A comece na posição 500, B 501, C 502.
Quando o programa for entrar na memória, o meu *Monitor Residente*, vai transformar as posições para algo real na memória. Afinal, o 500 já é do próprio *Monitor Residente* na memória.

Ele (MR) vai organizar de jeito que;
Variável `a` fique em [2500] = 0
Variável `b` fique em [2501] = 50
Variável `c` fique em [2502] = [2500] + [2501]

Temos então um pseudo gerenciamento de memória feito pelo *Monitor Residente*.




### Sistema Operacional
```
---------------|
|  Programas   |
|---------------
|      SO      |
|---------------
|  Hardware    |
|---------------
```

- SO possui bibliotecas.
- Oferece serviços ao programador, como por exemplo, abrir arquivos, acessar memória e etc.

#### Tipos de Sistemas Operacionais
```
---------------|
|  Kernel (SO) |
|---------------
| MicroKernel  |
|---------------
|  Hardware    |
|---------------
```

#### Monolíticos
Coloca tudo aqui dentro, o que tu acha que vai precisar, coloca.
O que tu não quiser, não tem.

#### Modular
Colocamos o básico, se precisar de algo, carrega.
Kernel enxuto
Módulos para drivers
Linux

Linux é *modular*, isso quer dizer que;
- 7 mega com coisas básicas e o resto é carregado sob demanda.
> mobprobe 8021q
> lsmod | grep 802
a partir de agora o kernel suporta. É possível fazer um módulo e por na árvore do Linux.
> rmmod 8021q # removemos.

#### Monousuário
> Ou seja, apenas 1 usuário. (Faz tempo que algum SO é mono... DOS)

#### Multiusuário
> Ou seja, mais de 1.

#### Monotarefa
> Um programa em execução.

#### Multitarefa
> Vários programas em execução.

#### Multiprogramação
> Meu SO dá a cpu para o processo 1 (P1), se o P1 fizer um IO e acessar o disco, por exemplo, meu SO coloca outro processo em execução até P1 voltar do IO.

#### Depois, veio o conceito de _time sharing_.
Como funciona?
P1 ganha CPU por algum tempo.
Perde a CPU depois que o tempo passou.

Dentro do _time sharing_ temos o:
- Colaborativo: Windows usava até o 3.1.
SO: _"Eu te dou a CPU por um tempo, e.g. 5 milisegundos, passou esse tempo, devolva para mim que preciso entregar para outro processo."_
Era muito fácil fazer um programa que travasse a máquina, ele pegava por um tempo e não devolvia. Era fácil cometer esse erro.
Funcionou um bom tempo com interrupções (hardware tinha suporte *só não era usado*).


- Preemptivo
Agora só consigo fazer com a ajuda do Hardware.
Ou seja, dou a CPU por uma fatia de tempo para o processo e ele devolverá a partir de uma interrupção.

Existe o relógio do hardware, que a cada `x` tempo a CPU é interrompida e uma ação é feita. É realizado uma rotina do S.O. uma dessas coisas passíveis de ser feita é pegar a CPU e entregar para outro processo.

(Intervalo).


# Questões

##### Questão 1
Considerando as características e os tipos de Sistemas Operacionais:
O Linux é, desde a versão 2.0: modular
- Resposta 1 capacidade do Sistema Operacional pegar novamente a CPU, mesmo sem a colaboração do aplicativo em execução: **preemptivo**
- Resposta 2 um kernel com todos os módulos: **monolítico**
- Resposta 3 O sistema multitarefa do Windows 3.1, 95 e 98: **multiusuário**

##### Questão 2
O que era o monitor residente?
Escolha uma:
- a. Um embrião do que hoje chamamos de Sistema Operacional. Ele se encarregava de carregar os jobs para a memória e iniciar a execução destes. :white_check_mark:
- b. Algoritmos para determinar se um job era CPU ou IP Bound
- c. Um gerenciador de sistemas de arquivos baseado no conceito de alocação contigua

##### Questão 3
Quantos bytes tem um setor do disco (também chamado de setor físico)?
Resposta: 512 :white_check_mark:

##### Questão 4
O Windows 95 possuia um sistema de Time Sharing preemptivo, onde o próprio hardware, através das interrupções, devolve a CPU para o sistema operacional
Escolha uma opção:
- Verdadeiro :white_check_mark:
- Falso

##### Questão 5
Antes mesmo de qualquer sistemas operacional, os computadores eram operados por painéis, sendo que:
Escolha uma:
- a. Na memória haviam apenas dados, não o código a ser executado :white_check_mark:
- b. Rotinas comuns aos programas, bem como os compiladores necessários, eram lidos do disco rígido
- c. Quando um programa fazia IO o painel imediatamente começava a executar o próximo JOB
- d. Havia o monitor residente que carregava o os jobs a partir de uma fita, organizando-as por necessidades

##### Questão 6
O monitor residente passou a dar suporte ao conceito de multiprogramação. Este conceito tem como caraceterística:
Escolha uma:
- a. Executar vários jobs ao mesmo tempo, dano a impressão de que todos estão em execução
- b. Executar um processo do início ao fim, mas se ele fizer I/O, escolher um novo para usar a CPU :white_check_mark:
- c. A implementação do conceito de Time Sharing para processos CPU Bound
- d. uso de um kernel monolítico


##### Questão 7
Tipo de kernel de um sistema operacional onde todos os módulos são compilados gerando um único binário:

Resposta: Monolítico :white_check_mark:
