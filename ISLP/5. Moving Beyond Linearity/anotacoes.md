# Moving Beyond Linearity
Nesse capítulo do livro basicamente são descritos sobre métodos de ultrapassar a simplicidade de modelos lineares.

# Regressão Polinomial
A forma padrão de estender uma regressão linear para uma regressão polinomial se dá utilizando diferentes graus da variável independente, ou seja:

$$\text{Regressão linear padrão: } \beta_0 + \beta_1 x_i + \epsilon_i$$
$$\text{Regressão linear polinomial: } \beta_0 + \beta_1 x_i + \beta_2 x^2_i + \beta_3 x^3_i + \ldots + \beta_d x^d_i + \epsilon_i$$

Conforme aumentamos o grau $d$ é possível produzir curvas extramemente não-lineares

## Como encontrar os coeficientes?
É possível encontrar os coeficientes para essa regressão com **mínimos quadrados** também, assim como fazemos na regressão linear tradicional. É possível imaginar que ao invés de estarmos utilizando somente uma feature e elevando ela, podemos estar trabalhando com diversas features que tem os valores de $x^2, x^3, \ldots, x^d$, ou seja, é como uma regressão linear múltipla também.

## Limitações e usabilidade
É incomum utilizarem $d$ maior que 3 ou 4, pois ela pode se tornar flexível "demais" e pode assumir formatos estranhos 


## Observações
Conforme maior é a variância dos coeficientes estimados, maior são os intervalos de confiança.

# Step Functions (Funções degrau)
Para essas funções degrau basicamente são definidos *cutpoints*, que podem ser definidos com base na distribuição de cada feature considerada. Definindo esses $K$ *cutpoints* são definidos $K + 1$ intervalos e são utilizadas funções indicadoras $I() \in \{0, 1\}$, e são definidos novos valores $C()$ da seguinte forma:

- $C_0(X) = I(X < c_1)$
- $C_1(X) = I(c_1 \leq X < c_2)$
- $C_2(X) = I(c_2 \leq X < c_3)$, e assim por diante.

Em que $I() = 1$ quando a condição é verdadeira e 0 caso contrário.

Vale notar que para qualquer valor de $X$, vale que $C_0(X) + C_1(X) + \ldots + C_K(X) = 1$ visto que $X$ está sempre em exatamente um dos $K+1$ intervalos. Dessa forma, utilizamos **mínimos quadrados** para treinar um modelo usando esses $C(\ldots)$ como features, da seguinte forma:

$$y_i = \beta_0 + \beta_1C_1(x_i) + \beta_2C_2(x_i) + \ldots + \beta_3C_3(x_i) + \epsilon $$

# Basis Functions
A ideia é basicamente utilizar uma fampília de funções $b(.)$ que são conhecidas e a função a otimizada é da seguinte forma:

$$y_i = \beta_0 + \beta_1 b_1 (x_i) + \beta_2 b_2 (x_i) + \ldots + \beta_3 b_3 (x_i) + \epsilon$$

Novamente, vale lembrar que as funções $b_i(.)$ são fixas e conhecidas, e transformam as features $X$, de tal forma que é possível utilizar **mínimos quadrados** para otimizar isso, e é possível relacionar essa função com a forma polinomial e step também, ou seja, é possível dizer que aqui estamos tratando de um caso mais genérico somente.

# Regression Splines