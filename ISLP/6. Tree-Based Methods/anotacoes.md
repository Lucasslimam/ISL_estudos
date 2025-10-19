# The Basics of Decision Trees
Árvores de decisão podem ser aplicados à problemas de regressão ou de classificação. Primeiro focaremos em árvores para regressão e, posteriormente, em classificação.

## Regression Trees

Descrevendo brevemente, pode-se dizer que a árvore de regressão é construída a partir dos seguintes passos:
1. Dividimos o espaço de predições, ou seja, o conjunto de possíveis valores $X_1, X_2, \ldots, X_p$ em $J$ partes distintas e sem *overlap* chamadas $R_1, \ldots, R_J$
2. Para cada observação que caia na região $R_j$, nós damos a mesma predição, que seria simplesmente a **média** dos valores em treinamento na região $R_j$.

### Como são construídas as regiões $R_j$ ?
O objetivo é traçar hiperplanos retangulares que separam os dados de forma a obter o menor erro, minimizando o $RSS$, ou seja:

$$min(RSS), \text{sendo }RSS = \sum_{j=1}^{J}\sum_{i \in R_j}(y_i - \hat{y}_{R_j})^2$$
com $\hat{y}_{R_j}$ sendo a média dos valores dos dados de treinamento na região $R_j$

Na prática, é impossível criar todas as combinações de divisões possíveis e calcular o $RSS$ para ver qual tem o menor resultado. Dessa forma, é tomada uma abordagem *top-down* e *greedy* chamada **Recursive Binary Splitting**. Ela é chamada *top-down* por iniciar no topo da árvore e ramificando depois, e é chamada de gulosa pois a decisão de como dividir a árvore é tomada em cada etapa focando somente na iteração atual, sem olhar passos posteriores para a tomada de decisão.

Passos do **Recursive Binary Splitting**:
1. Primeiro é selecionada a feature $X_j$ e o *cutpoint* *s* tal que a separação seja a melhor possível levando em conta a minimização do $RSS$, e essa separação é sempre gerando outras duas regiões, de tal forma que o erro é dado por:

$$\sum_{x_i \in R_1(j, s)}(y_i - \hat{y}_{R_1})^2  + \sum_{x_i \in R_2(j, s)}(y_i - \hat{y}_{R_2})^2$$

Sendo:
$\hat{y}_{R_1}:$ média dos valores da região 1 delimitada pela feature $j=1$ e cutpoint $s$.

2. Repete o passo 1 mas com a próxima feature até a condição de parada, ou também até nenhuma região conter mais de 5 observações.

### Tree Pruning (Poda)
É uma técnica comum para poder tratar casos de overfitting. Uma árvore menor com menos divisões pode levar a um modelo mais interpretável pelo custo de um pequeno bias.
Uma alternativa também é realizar as divisões em 2 regiões somente quando a **diminuição do $RSS$** ultrapassa um limiar mínimo definido.

Na prática, o que ocorre é crescer uma grande árvore $T_0$ e podar ela de volta de forma a ter uma subárvore.
#### Cost Complexity Pruning
Basicamente é colocada a quantidade de nós (ou regiões) de uma árvore na mesma conta que cálcula o RSS total juntamente com um parâmetro $\alpha > 0$, de tal forma que  cada valor de $\alpha$ corresponde a uma subárvore $T \subset T_0$ tal que buscamos minimizar a seguinte soma:

$$\sum_{m=1}^{|T|} \sum_{x_i \in R_m} (y_i - \hat{y}_{R_m})^2 + \alpha |T|$$
Sendo:
- $|T|$: o número de nós terminais da árvore (ou número de regiões);
- $y_i$: valor *target* para a amostra $i$;
- $\hat{y}_{R_m}$: valor predito pelo modelo na região $R_m$.

### Passos para construção da árvore de regressão

1. Usar o processo recursivo de split durante o crescimento da árvore, parando somente quando alguma divisão ficar com menos que um número mínimo de observações definido por você;
2. Aplicar o conceito de **Cost Complexity pruning** para obter uma sequência de "melhores subárvores", como uma função do parâmetro $\alpha$
3. Usar **K-fold cross validation** para escolher $\alpha$. Isso é, dividir os dados de treinamento em $K$ partes, e para cada $k = 1, 2, \ldots, K$:
    - (a) Repetir os passos 1 e 2 em todos com exceção do k-ésimo fold dos dados de treino
    - (b) Calcular o MSE sobre os dados que foram deixados de lado (o k-ésimo fold) como uma função de $\alpha$
    Calcule a média dos resultados para cada valor de $\alpha$ e escolha $\alpha$ que minimize o erro médio
4.  Retorne a subárvore do passo 2 que corresponde ao valor escolhido de $alpha$.


## Classification Trees
Ela é bem semelhante à árvore de regressão, a diferença é só que estamos tentando prever uma classe e, portanto, o $\hat{y}_{R_m}$ é dado pela **maioria** da região $R_m$.

Entretanto, o **$RSS$ não pode ser usado** como critério de ramificação da árvore e, portanto, temos alguns métodos alternativos:

**Classification error rate**: é simplesmente a fração entre as observações de treino na região $R_m$ que **não pertencem** ao grupo da **classe mais comum**
$$Erro = 1 - max_{k}(\hat{p}_{mk})$$

Sendo:

$\hat{p}_{mk}$: proporção de observações na região $R_m$ que são da classe $k$ e

Entretanto, essa métrica não costuma ser muito usada pela pouca sensbilidade, sendo substituídas pelas seguintes métricas:

**Gini**: o índice Gini é definido pela seguinte equação
$$Gini = \sum_{k=1}^{K}\hat{p}_{mk} (1 - \hat{p}_{mk})$$

Sendo:
- $K$: conjunto de classes
- $\hat{p}_{mk}$: proporção de observações na região $R_m$ que são da classe $k$

Ou seja, o índice Gini é basicamente a soma dos produtos da proporção que pertence ou não à classe de cada região para cada uma das classes. Vale verificar também que se a proporção for muito próxima de 1 então o índice Gini se aproxima de zero. Ele é considerado também uma métrica de "pureza" dos nós, dado o funcionamento descrito acima.

**Entropia**: basicamente mede o quanto de incerteza temos em cada região
$$D = - \sum_{k=1}^{K} \hat{p}_{mk} log(\hat{p}_{mk})$$

Dado que $0 \leq \hat{p}_{mk} \leq 1$, segue que $0 \leq -\hat{p}_{mk}$. Dado seu cálculo, é possível verificar que a entropia fica próxima de zero se os valores $\hat{p}_{mk}$ são todos próximos de zero ou próximos de 1. Portanto, assim como no índice Gini, ele terá um valor baixo se $m$-ésimo nó é puro. Além disso, o índice Gini e a Entropia são bem similares numericamente.

**Atenção**
Apesar das métricas Gini e Entropia serem mais utilizadas, caso o objetivo do modelo é ter uma acurácia alta, a métrica de **Classification error rate** tem preferência.


## Trees Versus Linear Models
Basicamente, é falado sobre o fato de árvores de regressão não performaram tão bem quanto regressões lineares caso realmente haja uma relação linear entre a variável independente e a target.

## Advantages and Disadvantages of Trees
### Vantagens:

1. **Interpretabilidade**
2. **Semelhança com a tomada de decisão de humanos**
3. **Facilidade em lidar com variáveis qualitativas**

### Desvantagens:
1. Não possuem o mesmo nível de acurácia preditiva em relação a outras abordagens de regressão e classificação já estudadas
2. Elas podem ser pouco robustas, ou seja, pequenas mudanças nos dados de treinamento podem causar grandes mudanças na árvore final.


# Bagging, Random Forests, Boosting and Bayesian Additive Regression Trees
Um método de *ensemble* que é o que discutirei em diante basicamente combina
modelos mais simples para obter, em conjunto, um modelo muito mais robusto e com maior potencial.

# Bagging
Como discutido anteriormente, as árvores podem ser bem sensíveis ao ponto de, com pequenas mudanças nos dados de treino, a árvore resultante pode ser bem diferente.
Dessa forma, utilizam *Bootstrap aggregation*, mais conhecido como *Bagging*, que atende o propósito de **reduzir a variância de um método de aprendizado estatístico**, sendo comumente usado no contexto de árvores de decisão.

Lembrando que dado um conjunto de *n* observações independentes $Z_1, \ldots, Z_n$, cada um com variância $\sigma^2$, a variância da média $\hat{Z}$ das observações é dado por $\sigma^2/n$. Ou seja, **tirar a média de um conjunto de observações reduz a variância**.
Portanto, uma forma natural de reduzir a variância e aumentar a acurácia do modelo é:
1. retirar diversos conjuntos de dados da população (reamostragem);
2. treinar um modelo distinto para cada reamostragem de dados (treinamento);
3. retirar a média das predições de cada um dos modelos.

Ou seja, na prática, estamos construindo diversas funções predititoras e tirando a média dos resultados, tendo algo como:

$$f_{bagging}(x) = \frac{1}{B} \sum^{B}_{b=1} f_{b}(x)$$ 

Essa técnica é util também para outros modelos de regressão, entretanto demonstra resultados especialmente bons para árvores de decisão.
## Bagging with Classification
Também é possível estender esse método para classificação mas ao invés da média o que ocorre, geralmente, é a contabilização de "votos" entre os modelos. Ou seja, se 3 submodelos classificaram como "Azul" e 2 como "Vermelho" então a cor é Azul.

## Out-of-bag Error Estimation
PENDENTE

## Variable Importance Measures
PENDENTE

# Random Forests
As random forests se assemelham muito ao processo anterior no processo de criação, mas possui uma sacada diferente em relação aos preditores utilizados.
Assim como na forma anterior, são criadas diferentes árvores por meio de reamostragem. O diferencial desse modelo está na forma como ele ramifica durante a criação de cada uma das árvores, pois a cada divisão é considerada uma subamostra diferente entre todos os preditores da base (geralmente o tamanho é da ordem de $\sqrt{p}$ preditores escolhidos).

Essa técnica ajuda muito, pois em um contexto que há uma variável muito determinante, é esperado que todas as subárvores tenham uma estrutura muito semelhante entre si. Entretnato, quando você "inibe" de ter sempre o mesmo conjunto de preditores, é possível ter coleções de árvores mais diferentes.
Apesar disso, esse modo de bagging com redução de preditres não costuma levar a grande redução da variância.

# Boosting
Assim como os aprendizados anteriores, são geradas diversas árvores. A diferença é que essa técnica não envolve reamostrar diversas vezes
com Bootstrap, ao invés disso, as árvores são geradas sequencialmente por meio de versões modificadas do dataset original.
O método de *boosting* aprende *devagar*. Dado um modelo atual, é tentado fittar uma árvore aos resíduos do modelo.

## Algoritmo de Boosting
1. Defina $\hat{f}(x) = 0$ e $r_i = y_i$ **para todo $i$** nos dados de treino.
2. Para cada $b = 1, 2, \ldots, B$, repita:

    a. Treine uma árvore $\hat{f}^b$ com $d$ splits (d+1 nós terminais) aos dados de treino $(X, r)$

    b. Atualize $\hat{f}$ adicionando uma versão $shrunk$ da nova árvore:
    $$\hat{f}(x) \larr \hat{f}(x) + \lambda \hat{f}^b (x)$$

    c. Atualize os resíduos
    $$r_i \larr r_i - \lambda \hat{f}^b (x_i)$$

3. Modelo com *boost*:
$$hat{f}(x) = \sum_{b=1}^{B} \lambda \hat{f}^b (x)$$

## Parâmetros utilizados
1. Número de árvores $B$. Diferentemente de random forests, pode ocorrer overfit se $B$ for muito grande,
     geralmente utilizam **validação cruzada para definir $B$**

2. O parâmetro de shrink $\lambda$, que é um pequeno número positivo. Ele controla a velocidade com que o *boosting* aprende.
Geralmente são valores 0.01 ou 0.001. Pequenos valores de $\lambda$ podem demandar um valor maior de $B$
 para ter uma boa performance.

3. Número $d$ de *splits* em cada árvore, que controla a complexididade da árvore final. Geralmente $d=1$ funciona bem e se chama *stump*.



# Bayesian Additive Regression Trees (BART)
Esse basicamente mistura os conceitos de *bagging* (reamostrando os dados e/ou preditores) e conceitos de *boosting*, no qual são
construídas diversas árvores, de forma iterativa, sempre tentando se ajustar aos resíduos.
Em **BART**, cada árvore é construída de forma aleatória como em *bagging* e **flores aleatórias**, e cada árvore
tenta se ajustar aos resíduos do modelo (como no boosting)

**Glossário:**
- $K$: quantidade de árvores de regressão
- $B$: número de iterações que o BART vai executar
- $\hat{f}^b_{k} (x)$: predição em $x$ para a k-ésima árvore de regressãi usada na iteração $b$.

Além disso, no fim de cada iteração, as $K$ árvores serão somadas, ou seja:
$$ \hat{f}^b (x) = \sum_{k=1}^{K} \hat{f}^b_{k} (x), \text{para } b = 1, \ldots, B$$

Na **primeira iteração** todas as árvores são inciializadas tendo somente um nó raiz, 
com $$\hat{f}^1_k (x) = \frac{1}{nK} \sum_{i=1}^{n} y_i$$
Sendo então o valor médio dividido pelo número de árvores. Isso ocorre, pois dado que nenhuma
árvore foi ajustada ainda, então o valor predito por cada uma das $K$ árvores é somente a **média global**
de todos os valores target.
**Nas próximas iterações**, o BART atualiza cada uma das $K$ árvores, uma por vez.
Na iteração $b$, para atualizar a árvore $k$, nós subtraímos de cada uma das respostas, o valor predito por todas as árvores **exceto a k-ésima** para obter o **resíduo parcial**
$$r_i = y_i - \sum_{k' \leq k} \hat{f}^b_{k'} (x_i) - \sum_{k' \geq k} \hat{f}^{b-1}_{k'} (x_i), \text{para } i = 1, \ldots, n$$
para a i-ésima observação. Ao invés de "fittar" uma árvore nova para esse resíduo parcial,
 o BART escolhe a perturbação para a árvore da iteração anterior ($\hat{f}^{b-1}_{k}$)
  do conjunto de possíveis perturbações, favorecendo as que melhoram o fit ao resíduo parcial.

Por fim, computa-se o $\hat{f}^b (x) = \sum_{k=1}^{K} \hat{f}^b_k (x)$ para cada uma das iterações $B$ e,
 por fim, calcula-se a média após $L$ *burn-in samples*:
 $$\hat{f}(x) = \frac{1}{B - L} \sum_{b=L+1}^B \hat{f}^b (x)$$

Esses *burn-in samples* são casos que são "jogados fora" nas primeiras iterações do algoritmo,
 pois as primeiras árvores são bem ruins, ou seja, levam em conta somente as árvores após as $L$ iterações
 

# Summary of Tree Ensemble Methods


