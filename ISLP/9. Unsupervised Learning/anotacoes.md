# Análise de Componentes Principais (PCA)

PCA é uma abordagem não-supervisionada dado que envolve somente um conjunto de features $X = \{X_1, X_2, X_3, \ldots, X_p\}$ e nenhuma variável resposta $Y$. Além dissoi, ele é usado para visualização de dados ou para completar dados faltantes de uma base.

Suponha que temos um gráfico de $P$ features e gostaríamos de plotar a relação entre elas, dessa forma, o cainho mais simples e intuitivo seria gerar $p(p-1)/2$ gráficos. Entretanto, com o PCA é posssível encontrar uma representação espacial com menos dimensões. A ideia principal é que todas as *n* observações estão em um espaço **p-dimensional** mas nem todas essas dimensões são relevantes/interessantes. Portanto, o foco do PCA é encontrar um menor número de dimensões que são relevantes, de tal forma que a **relevândia** é medida pela variabilidade das observaç~~oes em cada uma das dimensões. **Cada dimensão encontrada pelo PCA é uma combinação linear de $k$ features**.

A **primeira componente principal** de um conjunto de features $X$ é a combinação linear **normalizada** de todas as features:

$$Z_1 = \phi_{11}X_1 + \phi_{21}X_2 + \ldots + \phi_{p1}X_p$$

Essa tem a maior variância. Por *normalizado* quer dizer que $\sum_{j=1}^{p} \phi_{j1}^2 = 1$. Chamam os elementos $\phi_{11}, \ldots, \phi_{p1}$ de **loadings** da primeira componente principal. Juntos, os *loadings* são chamados de *principal component loading vector*, da forma $\phi_{1} = (\phi_{11}, \phi_{21}, \ldots, \phi_{p1})^T$. É necessária essa normalização, pois deixando eles livres, esses elementos podem assumir um valor arbitrariamente grande em módulo, resultando em uma **variância arbitrariamente grande**.

**Encontrando a primeira componente principal**

1. Primeiramente você tira a média de cada feature $X_k$
2. Desloca os dados com essa média, de tal forma que os dados ficam centralizados na origem do espaço $p-dimensional$.
3. Desenha uma reta de forma que a distância da projeção dos pontos projetados na reta até a origem seja a maior possível
4. A partir da primeira componente principal, as demais componentes são obtidas ao colocar uma reta perpendicular à primeira componente.