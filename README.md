# Topologia-com-3-roteadores

## Objetivo

O objetivo deste projeto foi criar todas as rotas possiveis que nossos pacotes de dados poderiam utilizar e assim criando um ambiente redundante e a prova de falhas usando roteamento estático.

## Instalações necessárias

Antes de começarmos o projeto é muito importante lembrarmos de instalar a placa de expansão hwic-2t no nosso roteador cisco, pois com ela utilizamos as portas serial para roteamento de longa distancia. Segue abaixo uma foto da placa de expansão:

![Alt Text](https://images-na.ssl-images-amazon.com/images/I/61rxXusQxxL._AC_SL1301_.jpg)

## Topologia e os primeiros passos a serem tomados

Após a instalação da placa de expansão hwic-2t temos uma topologia conforme a imagem abaixo com os roteadores sem as suas respectivas conexões.

![Alt Text](https://i.ibb.co/nRqY2Yq/imagem-2021-02-07-173953.png)

Primeiramente precisamos fazer a conexão das portas seriais dos nossos roteadores, fazendo um junção em formato de um triangulo entre os 3 roteadores.

![Alt Text](https://i.ibb.co/QNqSSVw/imagem-2021-02-07-174425.png)



