# Anotações sobre o capítulo

## K-Fold Cross Validation
Quando treinamos um modelo acabamos utilizando $75%$ dos dados que temos para treino e $25%$ para teste. Entretanto, como saber qual é a melhor separação possível? E como garantir que os dados estão bem distribuídos na parcela que estamos pegando para treinar ou testar?

Dessa forma, o método de validação cruzada se trata, basicamente, de separar o dado em $K$ partes, de tal forma que, por iteração, é escolhida 1 dessas $K$ partes para teste e o resto para treino, para cada uma das partes. No fim, é analisado, por exemplo, a média de algumas métricas de escolha para avaliação do modelo.

### Leave-one-out Cross Validation
Basicamente funciona da mesma forma que foi dito anteriormente na sessão de K-fold, a diferença é que cada amostra é considerada como uma "parte", então você treina com o dataset inteiro com exceção de 1 observação (que servirá de teste) e faz isso para cada uma das amostras. É um método bem pesado

### Bias-Variance Trade-Off for k-Fold Cross-Validation
Basicamente, o Leave-one-out tem variância muito alta mas menos Bias, dado que você está lidando com boa parte do Dataset. Entretanto, há mais variância, enquanto que no k-fold há menos variância, então tem o trade-off entre essas coisas.

#### Bias (Viés)

O **Bias** mede o desvio médio entre a predição média do modelo $( \mathbb{E}[\hat{f}(X)])$ e a função verdadeira $f(X)$:

$$\text{Bias}^2 = (\mathbb{E}[\hat{f}(X)] - f(X))^2$$


### Variância

A **Variância** mede a variabilidade das predições do modelo em torno de sua média:

$$\text{Variância} = \mathbb{E}[(\hat{f}(X) - \mathbb{E}[\hat{f}(X)])^2]$$

### Qual métrica é usada para a sumarização?
Pensando em problemas de regressão, por exemplo, a métrica pode ser o MSE, enquanto que em contextos de problemas de classificação geralmente utiliza-se o número de casos de classificações erradas, ou seja, a contagem mesmo.


## Bootstrap
Basicamente se trata de um método em que realiza-se a reamostragem com reposição várias vezes e é pega, em cada caso, alguma métrica tipo média ou mediana. E é montada uma distribuição das médias/medianas capturadas. 

A partir dessa distribuição, podemos utilizar o p-valor para ver se aceitamos a hipótese nula, exemplo de H0: "o remédio não faz efeito nos pacientes".

### Passos de utilização do Bootstrap com P-valor (bem resumido)

1. Calcular a média das amostras originais
2. Realizar o deslocamento das amostras pela média obtida no passo 1 (centralização da amostra)
3. Realizar a reamostragem com reposição a partir das amostras criadas no passo 2
4. Dada a distribuição obtida, somar as probabilidades do valor obtido ser maior $\geq média$ ou $\leq média$
5. Verificar se a probabilidade calculada no passo 4 é maior que a de $\alpha = 1 - IC$ sendo IC o intervalo de confiança e $\alpha$ sendo o nível de **significância**
6. caso $p > \alpha$ então reijeita-se a hipótese nula.