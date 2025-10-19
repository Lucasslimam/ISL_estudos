# Single Layer Neural Network

Suponha que você tem uma rede neural com uma única *hidden layer*, de tal forma que as entradas são dadas por um conjunto de features $X = (X_1, X_1, \ldots, X_p)$ e essas entradas passar por uma camada escondida $A$, onde é aplicada uma transformação não linear $h()$, sendo que os pesos das arestas são denotados por $w$.

Então, sendo $f(X)$ o resultado final dessa rede neural, ele é dado por:

1. Primeiramente, em $A_k$ temos o resultado da função $h_k (X)$, que se trata do resultado da função de ativação $g()$ sobre os inputs e pesos correspondentes, tendo algo da seguinte forma:

$$A_k = h_k (X) = g(w_{k0} + \sum_{j=1}^{p} w_{kj} X_j)$$

Ou seja, é o resultado da função $g$ (ReLU ou Sigmoidal) ao resultado de $w_{k0}$ que é o $bias$ associado, somado com a soma dos produtos de cada peso com cada entrada chegando nessa unidade $k$ na *hidden layer*.


# Multilayer Neural Networks
Nada demais

# Convolutional Neural Networks
Essas redes costumam ser bem utilizadas no contexto de imagens, e elas funcionam de tal forma que, primeiramente buscam pequenos padrões na imagem por meio de *convolutional layers* e as *pooling layers* destacam parte dessas amostras para escolher um subconjunto predominante.

## Convolutional Layers
Se tratam de camadas que realizam o processo de **convolução** por meio de **filtros de convolução**, que são basicamente matrizes que utilizaremos para multiplicar com partes da imagem, *convoluindo* partes da imagem.

Além disso, suponha que o filtro de convolução seja $2 \times 2$, então a convolução é aplicada, desde o canto **superior esquerdo**, de forma raster, em cada submatriz $2 \times 2$ da imagem original.
E por meio desse processo de convolução, são destacadas partes da imagem mais "no formato" do próprio filtro

## Pooling Layers
Uma camada de *pooling* provisiona uma maneira de condensar uma imagem grande em um pequeno resumo da imagem. Há diversas maneiras de realizar o pooling, uma delas é a **max pooling**, que sumariza cada submatriz $2 \times 2$ **sem overlap** (diferentemente da convolução) por meio do **valor máximo** do bloco, reduzindo a imagem por um fator de 2.

## Arquitetura de uma CNN
Na camada de *input* temos um *feature map* 3D em que cada canal se refere ao RGB e a imagem tem dimensões 32 x 32.
Cada filtro de convolução produz um novo canal e são aplicados aos 3 canais RGB de uma vez, ou seja, os filtros de convolução
possuem dimensão $K \times K \times 3$. Dessa forma, ao se aplicar 6 filtros de convolução, por exemplo, temos 6 "imagens" com resultado,
de tamanho reduzido por conta do padding ocasionado na etapa de convolução. Após isso, há a camada de pooling, que mantém o número de "canais" gerados
no momento de convolução mas reduz as imagens muitas vezes por um fator de 4 (2 em cada dimensão).

Esse processo de convolução e pooling é realizado repetidas vezes até que, por fim, os pixels são "flattados", sendo tratados como unidades
separadas  e são utilizados como "input" para uma outra subrede completamente conectada  antes de chegar ao **output layer**, que é dado por um
**softmax activation** para as 100 classes, por exemplo, que estão no escopo de identificação da CNN.

## Data Augmentation
É uma técnica de aumento de dados, utilizada em casos em que você tem poucos dados para o estudo e também para evitar *overfit*. Nessa técnica, tratando de imagens,
por exemplo, você pode gerar um tigre rotacionado, depois espelhado, depois utilizar zoom-in e zoom-out. Essas transformações são simples mas fazem com que a rede neural
aprendam o que é um tigre de forma mais genérica. Até fazem um paralelo dessa técnica com o Ridge Regression, por gerar soluções mais "suaves" por penalizar pesoss grandes nos modelos, incentivando soluções mais suaves e robustas.

