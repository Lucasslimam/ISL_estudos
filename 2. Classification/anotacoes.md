# Anotações sobre o capítulo

A classificação ocorre em contextos em que a variável que você está realizar a predição é qualitativa e não quantitativa. Por exemplo, se você quer prever se uma transação de cartão de crédito se trata de uma fraude ou não. Ou por exemplo, com base em características físicas da pessoa classificar entre : criança, jovem, adulto, etc.

## Logistic Regression
Esse modelo, inicialmente, se trata de um modelo do **Classificação Binária**, em que é calculada uma probabilidade da amostra ser uma instância da classe "0" ou "1"
com base na regressão obtida. Esse modelo pode ser expressado matemáticamente pela seguinte expressão, que se trata de uma função sigmoidal:

$$p(X) = \frac{e^{\beta _0 + \beta _1 X}}{1 + e^{\beta _0 + \beta _1 X}}$$

Dessa forma, utilizando essa expressão é notória que estamos calculadno uma probabilidade, dado um conjunto de features, com garantia que $0 \leq p(x) \leq 1$

### Como estimar os coeficientes da regressão logistica ?
Basicamente, é possível pensar em duas maneiras principais. Aproveitando o conceito de minimizar o RSS, é possível, com base em certas manipulações algébricas, concluir que:

$$log\left( \frac{p(X)}{1 - p(X)}\right) = \beta_0 + \beta_1 X $$

Dessa forma, é possível verificar a semelhança entre esse modelo e o de regressão linear e otimizar com base na minimização do RSS também. Entretanto, o método preferido é um mais conhecido como *maximum likelihood*, chamado em português de método de *máxima verossimilhança*.

Vale lembrar que até o modelo em si somente calcula probabilidades, também chamada de *scores* para cada input recebido pelo modelo. Entretanto, a decisão de qual classe pertence, acaba se tratando basicamente de um *trigger* arbitrário escolido pelo criador do modelo.

### Premissas assumidas
- Dados independentes entre si
- Assume que as variáveis de predição não devem ser muito correlacionadas entre si
- Depende muito do tamanho da base de treino, é preciso que seja grande para performar bem

A regressão logística não modela diretamente a probabilidade de um evento ocorrer, mas sim o **logaritmo das chances** (log odds) desse evento.
As chances representam a razão entre a probabilidade do evento ocorrer e seu complementar, e o *log odds* transforma essa razão em uma **escala linear**.

Dessa forma, o modelo assume os **log odds** são uma **combinação linear das variáveis independentes**, ou seja:
$$log\left( \frac{p}{1 - p}\right) = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \ldots + \beta_n x_n$$

Ou seja, ela não funciona bem se a relação entre as variáveis preditoras e o *target* não for aproximadamente linear nos **log odds**.

### Multinomial Logistic Regression
Em essência é a mesma coisa da regressão logística mas com múltiplas variáveis independentes explicativas, ela é útil, por exemplo, para contesto de multi-classificação. Entretanto, no contexto de multi-classificação, a essência é basicamente utilizar regressão logística múltiplas vezes. Por exemplo, digamos que você precise avaliar se é $y \in \{"Azul", "Verde", "Preto"\}$. É possível dividir primeiro entre verificar se $y \in \{"Azul", "Verde"\}$  ou $y = "Preto"$ e depois calcular separadamente para dividir entre "Azul" e "Verde".



## LDA
Basicamente é um algoritmo que gera um eixo que tenta aumentar a separação entre instâncias de classes diferentes. LDA é parecido com PCA mas ele foca
em **maximizar a separabilidade entre as categorias/classes conhecidas**. Por exemplo, no caso de um gráfico 2D, o LDA gera uma reta que, ao projetar esses pontos para essa reta,
é possível ter reta que maximiza a separabilidade entre as categorias.

### Como o LDA funciona? Como ele cria o novo eixo?
Ele é criado com base em dois critérios.

1) Maximização da distância entre as médias das instâncias de categorias diferentes.
2) Minimizar a variação, que chamam de $s^2$ em cada categoria

Isso é traduzido matematicamente, supondo que temos 2 categorias, por:

$$\frac{(\mu_A - \mu_B)^2}{s_A^2 + s_B^2} $$

O esperado é que $(\mu_A - \mu_B)^2$ seja o **maior possível** e que $(s_A^2 + s_B^2)$ seja o **menor possível**.

### E como funciona o LDA para quando temos 3 categorias?
Nesse caso, duas coisas mudam, mas só um pouco:

1. A forma como é medida a distância entre as médias:
    - É primeiro encontrado um ponto que seja **central** a todos os dados;
    - Depois, as distâncias são calculadas entre a média de cada categoria e o **ponto central** e também minimizando a **variância** pela equação: $$\frac{d_A^2 + d_B^2 + d_C^2}{s_A^2 + s_B^2 + s_C^2}$$
2. A segunda diferença é que são criados 2 novos eixos para separar os dados.

