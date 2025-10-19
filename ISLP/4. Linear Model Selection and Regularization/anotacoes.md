Nesse capítulo são discutidas algumas formas diferentes de se fazer o *fit* de modelos lineares, além do *least squares*. Assim como será discutido diferentes métodos de *fitting* podem melhorar a **acurácia da predição** e a **interpretabilidade do modelo**.

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
O Ridge Regression tem suas desvantagens, pois diferentemente dos métodos explicados antes dele, ele envolve **todas** as variáveis explicativas, sendo que as menos importantes tendem a ter coeficiente zero, mas nenhuma é efetivamente zerada.
A expressão dada pelo *Lasso Regression* é similar ao *Ridge*, como apresentado abaixo:

$$RSS = e_1^{2} + e_2^{2} + \ldots + e_n^{2} = \sum_{i = 1}^{n} e^{2}_{i}$$
$$Lasso = RSS + \lambda \sum_{j}^{p} |\beta_j|$$

A diferença se dá apenas por se utilizar de $|\beta_j|$ ao invés de $\beta_j^2$ (Ridge). Dessa forma, é possível dizer que o Lasso realiza uma **penalidade** $l_1$ ao invés de $l_2$ como no Ridge

**Definição da norma $l_1$:**
    $$||\beta||_2 = \sum_{j}|\beta_j|$$

**Definição da norma $l_2$:**
    $$||\beta||_2 = \sqrt{\sum_{j}\beta_j^2}$$

Assim como no ridge regression, o lasso comprime os coeficientes de forma 
a tender a zero, entretanto no caso da penalidade $l_1$, ela tem o efeito de
 forçar alguns coeficientes para serem exatamente iguais a zero quando o parâmetro
  $\lambda$ é suficientemente grande. Portanto, de certa forma, ela também performa uma certa **seleção de variáveis**, gerando modelos muito mais fáceis de interpretar do que os que utilizam ridge regression.

## Como definir o $\lambda$ ?
Uma forma, seria escolher um grid de valores $\lambda$ e é computado o erro com **cross-validation** para cada valor de $\lambda$ e é selecionado o valor de $\lambda$ que teve o menor erro com validação cruzada.


# Dimension Reduction Methods
Em todos os casos acima, os métodos de controlar a variância se deram por utilizar um subset das variáveis explicativas ou "encolher" os coeficientes em direção ao zero, ambos métodos considerando somente as variáveis explicativas originais. Aqui, é tratada uma nova abordagem em que transformamos as variáveis explicativaas em um outro conjunto de variáveis (mudamos de espaço vetorial) e treinamos um modelo com base nessas novas variáveis transformadas.

## Principal Component Analysis (PCA)
1. Calcula a média para a $X_1$ e $X_2$, tendo então o ponto central de um gráfico 2D
2. Agora shiftamos os dados de tal forma que esse ponto central fique na origem $(0, 0)$
3. Desenha-se uma linha que passa pela origem e rotacionamos ela até estar "Bom", que é o que irei detalhar abaixo

Para verificar o quão bom a linha fitta os dados são realizados os seguintes passos:
1. Os dados são projetados na reta gerada por PCA
2. Mede-se as distâncias entre os dados e a linha e tenta **minimizar** essas distâncias **OU** pode tentar achar a linha que **maximiza** as distâncias entre os pontos projetados e a origem 

Observação, é mais fácil calcular da segunda forma, ou seja, maximizando as distâncias entre os dados projetados da origem

3. Fazendo da segunda forma no item 2, calcula-se a distância entre cada um dos dados projetados na linha do PCA em relação à origem e soma-se os quadrados dessas distâncias, de forma a não haver cancelamento entre distâncias positivas e negativas, chamado de SS(distances) por Sum of Squares
4. Repete-se os passos 1, 2 e 3 até ter a maior distância, ou seja, a maior SS(distances)
5. Por fim, temos então a PC1 (primeira componente principal)

Supondo, por exemplo, que a PC1 tem um coeficiente angular de $0.25$ e o eixo X é "Gene 1" e o eixo Y é "Gene 2", significa então que para se ter a linha PC1 é ncessário misturar 4 unidades do Gene 1 com 1 unidade do Gene 2. ou seja, o Gene 1 é mais relevante para descrever como os dados estão espalhados.

Podemos interpretar, portanto que o PC1 se trata de uma combinação linear do Gene 1 e 2

Além disso, para caso queiramos a segunda componente principal, basta somente traçar uma reta perpendicular ao PC1, e não é necessário realizar mais nenhum processo de otimização.

### Plotar com PCA
1. Apenas rotacionamos as componentes principais até o PC1 ficar horizontal.
2. Utilizamos os pontos projetados para encontrar onde as amostras vão no PCA Plot

### Variação explicada por PCA
Eingenvalues, ou seja, autovalores são somente medidas de variação, e isso é utilizado para calcular a explicabilidade da variância advinda de cada componente principal.

$$\text{Eingenvalue}_{PC1} = \frac{SS(\text{distâncias para o PC1})}{n-1}$$

$$\text{Eingenvalue}_{PC2} = \frac{SS(\text{distâncias para o PC2})}{n-1}$$

Imagine que a Variação para o PC1 é de 15 e do PC2 é de 3. Então a variação total de ambos os PCs é 18. Isso significa que a variação total advinda pela componente 1 é de $15/18$ e do PC2 de $3/18$
