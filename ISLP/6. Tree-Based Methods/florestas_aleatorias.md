# Florestas Aleatórias

Florestas aleatórias são um aprimoramento das árvores de decisão empacotadas (bagging), através de um pequeno ajuste aleatório que **decorrela** as árvores da floresta. O objetivo principal é reduzir a correlação entre as árvores para melhorar a precisão do modelo.

## Como funcionam as florestas aleatórias?

1. **Amostras bootstrap**: Assim como no bagging, uma floresta aleatória é construída utilizando **múltiplas árvores de decisão**. Cada árvore é treinada em uma amostra bootstrap dos dados, ou seja, uma amostra aleatória retirada com reposição do conjunto de dados original.

2. **Seleção de preditores aleatórios**: O grande diferencial das florestas aleatórias ocorre **durante a construção de cada árvore**. Ao realizar a divisão (split) de um nó, ao invés de considerar todos os preditores disponíveis, é escolhida **uma amostra aleatória de m preditores** do conjunto total de p preditores. Em seguida, o algoritmo escolhe o melhor preditor dentre os m selecionados para realizar a divisão.

3. **Decorrelação das árvores**: Cada árvore da floresta é "forçada" a considerar apenas um subconjunto aleatório de preditores, o que impede que as árvores usem os mesmos preditores em suas divisões. Isso garante que as árvores da floresta sejam **menos correlacionadas** entre si, o que melhora a performance do modelo.

4. **Escolha de m**: Em muitos casos, o número de preditores considerados em cada divisão (m) é escolhido como sendo aproximadamente igual à **raiz quadrada do número total de preditores (√p)**. Por exemplo, se há 13 preditores, em cada divisão o modelo consideraria cerca de 4 preditores (√13 ≈ 4). Isso é importante para evitar que um preditor muito forte domine todas as árvores, o que aconteceria no bagging, onde todas as árvores poderiam usar esse preditor em suas divisões principais.

## Benefícios das florestas aleatórias

- **Redução de variância**: Uma das vantagens de usar florestas aleatórias é a redução da variância das previsões. No bagging, as árvores podem ser altamente correlacionadas, o que não ajuda muito na redução da variância geral. As florestas aleatórias, ao decorrelacionar as árvores, reduzem essa variância de maneira mais eficaz.
  
- **Maior robustez**: Como as árvores na floresta são menos correlacionadas, a média das previsões das árvores tende a ser mais confiável, melhorando a **precisão do modelo**.

- **Previsões mais precisas**: Em muitos casos, o desempenho de uma floresta aleatória supera o de um único modelo de árvore de decisão, especialmente em problemas com muitos preditores.

## Diferença entre Bagging e Florestas Aleatórias

A principal diferença entre bagging e florestas aleatórias é a escolha de m, o número de preditores considerados em cada divisão:

- **Bagging**: Quando m = p (ou seja, o número de preditores considerados em cada divisão é igual ao número total de preditores), isso equivale ao bagging, onde todas as árvores têm acesso a todos os preditores.
  
- **Floresta aleatória**: Quando m < p, as árvores são decorrelacionadas, o que melhora a performance, especialmente quando há muitos preditores correlacionados.

## Exemplo com Dados Biológicos

Um exemplo de uso de florestas aleatórias foi em um conjunto de dados biológicos de expressão de genes. O objetivo era prever o tipo de câncer de pacientes com base na expressão de 500 genes com maior variação.

- Com **400 árvores** e diferentes valores de m (tamanho do subconjunto de preditores), foi possível observar que o uso de **m = √p** resultou em um erro de teste ligeiramente melhor do que o bagging (m = p).
- O **erro de uma única árvore** foi de 45,7%, enquanto o **erro de teste** para a floresta aleatória foi significativamente menor, mostrando a eficácia de usar múltiplas árvores decorrelacionadas.

## Conclusão

As florestas aleatórias oferecem uma maneira poderosa de melhorar o desempenho de modelos de árvore de decisão, principalmente quando lidamos com grandes volumes de dados com muitos preditores. Elas ajudam a reduzir a **variância** e a **correlação** entre as árvores, proporcionando previsões mais robustas e precisas. Ao introduzir a aleatoriedade no processo de construção das árvores, as florestas aleatórias evitam que um único preditor forte domine, tornando o modelo mais flexível e adaptável a diferentes tipos de dados.
