# Anotações sobre o capítulo

## Conceito
A regressão linear se trata de um modelo bem direto, em que é assumida uma relação linear entre seu conjunto de variáveis preditivas X
e um conjunto de resultados preditos Y.

Uma regressão linear com duas variáveis preditivas, por exemplo é da forma:

$$Y \approx \beta _0 + \beta X$$

Nós dizemos que essa relação é aproximada, pois é praticamente impossível que tenhamos os insumos e a clareza de quais variáveis que, quando combinadas em uma regressão linear, ajustam perfeitamente uma relação linear entre $X$ e $Y$.

## Como estimar os coeficientes $\beta$ ?


Para estimar os coeficientes $\beta$, basicamente, podemos iterativamente minimizar o erro através de métodos da **descida do gradiente descendente** ou somente encarar como um processo de otimização e **resolver analiticamente**.

A otimização a ser realizada é basicamente a minimização do RSS. No caso, dado que queremos o absoluto dos erros, e para também facilitar uma certa derivada, é utilizado o RSS, a soma dos erros quadráticos, ou seja:
$$e_i = f(X_i) - y_i$$
$$RSS = e_1^{2} + e_2^{2} + \ldots + e_n^{2}$$

Para minimizar o RSS bastaria derivar RSS em relação ao $X$ e igualar à zero para pegar o ponto de **mínimo**. Dado o formato da função RSS, que é basicamente uma parábola, o mínimo local é também o mínimo global.

## Regressão Linear Múltipla
É, em essência a mesma coisa que a regressão linear simples, entretanto, é assumida uma relação entre a combinação linear de **múltiplas** variáveis
explicativas com a variável resposta, ou seja, temos algo da forma:

$$y \approx \beta _0 + \beta _1 x_1+ \beta_2 x_2 + \ldots + \beta_n x_n, \forall y \in Y$$

## Métricas para Avaliação

### Erro quadrático médio (Mean Squared Error - MSE)
$$MSE = \frac{1}{n} \sum_{i}^{n}e_{i}^2$$

### Erro quadrático absoluto (Mean Absolute Error - MAE)
$$MAE = \frac{1}{n} \sum_{i}^{n}|e_{i}|$$

### Raiz do erro quadrático médio (RMSE):
$$RMSE = \sqrt{MSE}$$

### Utilização $R^2$
É uma boa para verificar o quanto da variância dos dados é explicada pelo modelo linear que estamos criando.

Ele é calculado da seguinte forma:
$$R^2 = 1 - \frac{SSE}{SST}$$

- SSE (Soma dos erros ao quadrado): 
$$SSE = \sum_{i=1}^{n} (y_i - y_{i_{pred}})^2$$
- SST (Soma total dos erros quadrados em torno da média): 
$$SST = \sum_{i=1}^{n} (y_i - \overline{y})^2$$

#### Interpretação do $R^2$.
- $R^2$ = 1: O modelo explica 100% da variabilidade dos dados observados, a relação linear é perfeita.
- $R^2 = 0$: O modelo não explica nenhuma variabilidade dos dados observados. Ou seja, o modelo é tão bom quanto somente utilizar a média na previsão 
- $R^2 < 0$: Se ele for negativo então ele é até pior que o modelo que se baseia somente na média.


