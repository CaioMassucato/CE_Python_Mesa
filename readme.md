# Modelo Forest Fire

## Resumo

O [forest fire model](http://en.wikipedia.org/wiki/Forest-fire_model) é um modelo simples que simula o fogo alastrando por uma floresta, a qual é uma grade de células as quais podem ser vazias ou conter uma árvore. As árvores podem estar pegando fogo, queimadas ou não queimadas. O fogo se espalha de cada árvore que esteja pegando fogo para árvores vizinhas que não estejam queimadas e então, aquela se torna queimada. Isso continua até que não tenha mais árvores pegando fogo.

A modificação aqui realizada, busca trazer uma nova variável para o modelo: o tamanho da árvore. Para cada célula com uma árvore, existe uma chance daquela ser grande, além disso, para cada árvore grande, existe uma chance menor de pegar fogo quando comparada a uma árvore pequena. Essa modificação pretende direcionar o modelo para a realidade, onde nem todas as árvores pegam fogo da mesma maneira, de forma que em algumas regiões da floresta, por mais que estejam queimadas as árvores em volta, algumas árvores permanecem, dependendo de sua resistência, grossura do casco e outros fatores.

## Como inicializar o servidor

Para rodar o modelo, execute o comando no diretório do projeto:

```
    $ mesa runserver
```

## Arquivos

### ``forest_fire/model.py``

Este arquivo possui as definições do modelo, onde estão contidos os agentes **TreeCell**, o qual possui coordenadas (x, y), condition (*Fine*, *On Fire* e *Burned Out*) e size (*Small* ou *Big*). Em cada criação da árvore, existe uma chance da mesma ser *Small* ou *Big* e, para cada árvore *Big* existe uma chance menor da mesma pegar fogo, quando comparada a uma árvore *Small*. Em cada passo, se a condition é *On Fire*, o fogo se espalha para as árvores vizinhas (de acordo com [Von Neumann neighborhood](http://en.wikipedia.org/wiki/Von_Neumann_neighborhood) que atenderem a probabilidade de pegar fogo de acordo com o tamanho.

A classe **ForestFire** representa o recipiente do modelo, é instanciado com os parâmetros width, height e size, que definem, respectivamente, o tamanho da grade, a densidade (que é a probabilidade de cada célula possuir uma árvore) e o tamanho da árvore (probabilidade de cada árvore ser *Big*). Quando um modelo é instanciado, cada célula possui uma probabilidade = densidade de conter uma árvore e, para cada célula que contenha uma árvore, existe uma probabilidade = size da mesma ser *Big*. Além disso, todas as árvores na primeira coluna (x = 0) possuem o estado *On Fire*

Em cada passo, as árvores são ativadas aleatoriamente, espalhando o fogo e queimando. Isso se repete até que não existam mais árvores no estado *On Fire*.


### ``forest_fire/server.py``

Este arquivo define e inicializa a visualização do modelo no navegador, que transforma o objeto de TreeCell em portrayal, desenhando no navegador. Cada árvore é um retângulo na grade, onde a cor mostra seu estado: verde para *Fine*, vermelho para *On Fire* e preto para *Burned Out*.

Além disso, nos gráficos abaixo da grade, podemos ver, para cada passo da simulação, a quantidade de árvores para cada estado e para cada tamanho de árvore.

