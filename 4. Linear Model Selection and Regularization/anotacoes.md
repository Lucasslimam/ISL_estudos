Nesse capítulo são discutidas algumas formas diferentes de se fazer o *fit* de modelos lineares, além do *least squares*. Assim como será discutiso, diferentes métodos de *fitting* podem melhorar a **acurácia da predição** e a **interpretabilidade do modelo**.

- **Acurácia da predição**: Assumindo que a relação entre as variáveis explicativas e a variável dependente é aproximadamente linear, o método de mínimos quadrados terá pouco *bias*. Se $n >> p$, isto é, o número de observações (n) é muito maior que o número de variáveis preditivas (p), então o método de mínimos quadrados tente a ter pouco *bias*, entretanto, caso $n$ não seja tão maior, então é possível que tenha muita varioabilidade no método *least squares*. Além disso, caso $p > n$, então há infinitas combinações de coeficientes, infinitas soluções, ajustando perfeitamente em dados de treino, por conseguir chegar a ter erro zero, mas performando terrivelmente nos dados de teste.

Há diversas alternativas, clássicas e modernas para usar o *least squares* no *fit*, nesse capítulo são discutidos **3 classes importantes de métodos**:

- **Subset Selection**: Envolve somente selecionar um subset de variáveis que acreditamos ter uma relação linear com o target e então fitamos utilizando *least squares* com esse subconjunto.

- **Shrinkage**: Refere-se a um método de ajuste de modelos estatísticos que busca reduzir o impacto de alguns coeficientes estimados pelo *least squares* encolhendo-os em direção ao zero, reduzindo a influência e diminuindo a variância. Dependendo da forma como é realizado, é possível que alguns coeficientes chegem a exatamente zero, fazendo então uma seleção de variáveis também.

- **Dimensionality Reduction**: Essa abordagem é basicamente projetar os $p$ preditores para um espaço $M$-dimensional em que $M < p$. Essa redução pode ser feita, por exemplo, por meio da combinação linear entre variáveis, ou projeções. Posteriormente, essas $M$ novas variáveis são utilizadas no treinamento com *least squares*.

# Subset Selection
Aqui detalharei um pouco as formas de se realizar essa seleção, sem ser de forma aleatória.

## Best Subset Selection
Dados $p$ variáveis preditivas, realizamos $2^p$ combinações de preditores, a que performar melhor de acordo com alguma métrica de escolha, é a combinação definitiva. Apesar dessa forma ser a "melhor", é inviável computacionalmente quanto maior o número de variáveis e número de dados. Portanto, geralmente usam técnicas *Stepwise*.

## Forward Stepwise Selection
Entretanto, há formas de fazer isso de um jeito mais eficiente. Primeiro podemos pensar que começamos com um modelo com zero variáveis e vamos incrementando por um processo iterativo. Para escolher a primeira variável verificamos qual performou melhor e selecionamos ela, posteriormente, pegamos uma segunda variável e combinamos com essa primeira e escolhemos a que teve melhor resultado. 

A métrica utilizada para selecionar o próximo preditor pode ser o $RSS$ ou o $R^2$ em relação aos dados de treino mesmo

## Backward Stepwise Selection
É parecido com o forward mas ele começa com todas as variáveis preditivas e, são testadas combinações com $p_{k} -1$ preditores e seleciona-se a que tem menor $RSS$ ou maior $R^2$ em relação aos dados de treino.

## Como selecionar o subconjunto ótimo de preditores?
Os métodos que descrevi acima geram modelos distintos com diferentes subconjuntos de preditores, como escolher o **melhor** modelo? Para isso há duas abordagens principais:

1. Estima-se **indiretamente** o *test error* fazendo ajustes ao *training error* para levar em conta o viés causado por *overfitting*.

2. Pode-se estimar **diretamente** o *test error* usando um conjunto de validação ou validação cruzada.


# Shrinkage Methods
Aqui iremos tratar sobre alguns métodos de regularização que "encolhem" os coeficientes para algo próximo a zero ou exatamente zero, também realizando seleção de variáveis para esses casos.

## Ridge Regression
Sabemos que a otimização por  *least squares* trata-se basicamente de minimizar a seguinte expressão:

$$RSS = e_1^{2} + e_2^{2} + \ldots + e_n^{2} = \sum_{i = 1}^{n} e^{2}_{i}$$

Entretanto, com ridge regression, tenta-se estimar coeficientes $\beta^{R}$ que minimizem:

$$Ridge = RSS + \lambda \sum_{k = 1}^{p}\beta^{2}_{k}$$

onde $\lambda \geq 0$ é um *tuning parameter*, para ser determinado separadamente. Assim como os mínimos quadrados, o *ridge regression* busca encontrar coeficientes que minimizem o $RSS$. Entretanto, o segundo termo, $\lambda \sum_{k = 1}^{p}\beta^{2}_{k}$ chamado de *shrinkage penalty* é pequeno quando $\beta_1, \beta_2, \ldots, \beta_p$ são próximos de zero, ou seja, minimizar a função envolve minimizá-los também de encontro a zero.

O parâmetro $\lambda$ serve para controlar o impacto desses dois termos nos coeficientes da regressão. Quando $\lambda = 0$ então o impacto é nulo e o resultado será o mesmo produzido com *least squares* normalmente. Entretanto, conforme $\lambda \to \infty$ o impacto do *shrinkage* aumenta e os coeficientes $\beta_k$ tendem a zero

### Qual a vantagem do Ridge Regression?
A vantagem dele sobre *least squares* comum está no *trade-off* entre viés e variância. Conforme $\lambda$ aumenta, a variãncia diminui mas o bias aumenta. Ele funciona melhor em casos que o *least squares* simples tem muita variância. Além disso, o custo computacional dele é muito reduzido em relação a casos em que precisamos realizar $2^p$ combinações e, foi demonstrado que as computações necessárias para resolver *simultãneamente para todos os valores de $\lambda$ são quase idênticas ao tentar resolver o *least squares* simples.


## The Lasso

