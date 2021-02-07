# Topologia-com-3-roteadores

## Objetivo

O objetivo deste projeto foi criar todas as rotas possíveis que nossos pacotes de dados poderiam utilizar e, assim, criando um ambiente redundante e à prova de falhas usando o roteamento estático.

![Alt Text](https://i.ibb.co/0yHM2LY/54-Topologia-com-3-Roteadores.png)

## Instalações necessárias

Antes de começarmos o projeto, é muito importante lembrarmos de instalar a placa de expansão hwic-2t no nosso roteador cisco, pois com ela utilizamos a porta serial para roteamento de longa distância. Segue abaixo uma foto da placa de expansão:

![Alt Text](https://images-na.ssl-images-amazon.com/images/I/61rxXusQxxL._AC_SL1301_.jpg)

## Topologia e os primeiros passos a serem tomados

Após a instalação da placa de expansão hwic-2t, temos uma topologia conforme a imagem abaixo, com os roteadores sem as suas respectivas conexões:

![Alt Text](https://i.ibb.co/nRqY2Yq/imagem-2021-02-07-173953.png)

Primeiramente precisamos fazer a conexão das portas seriais dos nossos roteadores, fazendo uma junção em formato de um triângulo entre os 3 roteadores:

![Alt Text](https://i.ibb.co/QNqSSVw/imagem-2021-02-07-174425.png)

## Configuração dos roteadores

Feita todas as instalações dos cabos, partiremos para as configurações do roteadores:

### Roteador-1

![Alt Text](https://i.ibb.co/vB7Cv83/imagem-2021-02-07-174845.png)

O roteador 1 está em sua rede principal 192.168.0.0/24 mas com a sua interface g0/0 desativada e sem estar configurada. O primeiro passo que devemos tomar é utilizar os seguintes comandos para ativar a interface e colocar o ip de sua respectiva rede:

* int g0/0 
* ip add 192.168.0.1 255.255.255.0
* no shutdown

Adicionando o ip na interface g0/0 e ligando ela, devemos fazer o mesmo nas interfaces seriais:

* int s0/0/0
* ip add 190.140.0.1 255.255.255.252
* no shutdown
* int s0/0/1
* ip add 190.140.1.1 255.255.255.252
* no shutdown

### Roteador-2

![Alt Text](https://i.ibb.co/Y3qHvGJ/imagem-2021-02-07-175511.png)

Para o roteador 2 utilizamos os mesmos comandos utilizados no roteador 1, ativando e colocando ip nas interfaces, lembrando sempre de alterar os ips de cada interface de acordo com a sua respectiva rede. Segue os seguintes comandos para a int g0/0:

* int g0/0
* ip add 172.16.0.1 255.255.0.0
* no shutdown

Para as interfaces seriais:

* int s0/0/0
* ip add 190.140.0.2 255.255.255.252
* no shutdown
* int s0/0/1
* ip add 190.140.2.1 255.255.255.252
* no shutdown
* ex

### Roteador-3 

![Alt Text](https://i.ibb.co/WDW0rp7/imagem-2021-02-07-180040.png)

O roteador 3 também é sem segredo, fazemos as mesmas configurações dos roteadores acima, mudando sempre o ip para sua respectiva rede. Segue abaixo os comandos para a int g0/0:

* int g0/0
* ip add 10.0.0.1 255.0.0.0
* no shutdown

Para as interfaces seriais:

* int s0/0/0
* ip add 190.140.1.2 255.255.255.252
* no shutdown
* int s0/0/1
* ip add 190.140.2.2 255.255.255.252
* no shutdown

## Rotas estáticas

Colocado o ip em cada interface dos roteadores, estamos prontos para fazer suas respectivas rotas, lembrando que faremos todas as rotas possíveis para todas as redes disponíveis, criando assim um ambiente seguro e à prova de falhas.

![Alt Text](https://i.ibb.co/2dSjdgV/imagem-2021-02-07-180518.png)

### Roteador-1

No caso do roteador 1 criaremos 6 rotas:

#### Rotas para a rede 2
* ip route 172.16.0.0 255.255.0.0 190.140.0.2
* ip route 172.16.0.0 255.255.0.0 190.140.1.2

#### Rotas para a rede 3
* ip route 10.0.0.0 255.0.0.0 190.140.1.2
* ip route 10.0.0.0 255.0.0.0 190.140.0.2

#### Rotas para a rede WAN 3
* ip route 190.140.2.0 255.255.255.252 190.140.0.2
* ip route 190.140.2.0 255.255.255.252 190.140.1.2

### Roteador-2

Para roteador 2 também criaremos 6 rotas:

#### Rotas para a rede 1
* ip route 192.168.0.0 255.255.255.0 190.140.0.1
* ip route 192.168.0.0 255.255.255.0 190.140.2.2

#### Rotas para a rede 2
* ip route 10.0.0.0 255.0.0.0 190.140.2.2
* ip route 10.0.0.0 255.0.0.0 190.140.0.1

#### Rotas para a rede WAN 2
* ip route 190.140.1.0 255.255.255.252 190.140.2.2
* ip route 190.140.1.0 255.255.255.252 190.140.0.1

### Roteador-3 

No roteador de número 3 será utilizado o mesmo número de rotas:

#### Rotas para a rede 1
* ip route 192.168.0.0 255.255.255.0 190.140.1.1
* ip route 192.168.0.0 255.255.255.0 190.140.2.1

#### Rotas para a rede 2
* ip route 172.16.0.0 255.255.0.0 190.140.2.1
* ip route 172.16.0.0 255.255.0.0 190.140.1.1

#### Rotas para a rede WAN 1
* ip route 190.140.0.0 255.255.255.252 190.140.2.1
* ip route 190.140.0.0 255.255.255.252 190.140.1.1

## Roteamento dinâmico e sua função

Feita todas as rotas, a rede já está totalmente funcional e utilizando todas suas rotas possíveis para o envio de dados entre as redes. O ponto é: existe uma forma mais fácil de fazer todas essas rotas sem ser pelo roteamento estático? Sim, existe, e é chamado de roteamento dinâmico. Como visto no roteamento estático, as informações que um roteador precisa saber para poder encaminhar seus dados corretamente aos seus destinos, são colocadas manualmente na tabela de rotas, assim nos comandos acima.
Diferente, no roteamento dinâmico, os roteadores podem descobrir estas informações automaticamente e compartilhá-las com outros roteadores via protocolos de roteamento dinâmico.

## Conclusão

Ou seja, poderíamos muito bem utilizar o protocolo de roteamento dinâmico nos roteadores, isso nos pouparia tempo e seria mais prático, mas o objetivo principal do projeto é entender como funciona cada rota; não vale de nada utilizar o roteamento dinâmico sem saber exatamente o que ele está fazendo. Por isso, criamos manualmente as rotas para entendermos como o roteamento dinâmico irá funcionar e como ele formula cada rota para a respectiva rede.


Autor: Lucas Silva Conrado
Revisão: Sasha Sanches










