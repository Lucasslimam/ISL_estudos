# Recurrent Neural Networks

Em uma RNN, o objeto de entrada é uma **sequência** de dados, esse é o ponto principal das RNNs, elas foram projetadas projetadas
para tirar vantage da natureza sequência de determinados objetos, da mesma forma que CNNs são boas para tirar vantagem da estrutura espacial das imagens.

A estrutura dessas redes basicamente tem uma retroalimentação, de tal forma que a sequência de valores é considerada. Além disso, vale ressaltar que esse esquema de loopback,
quando é muito realizado, ou seja, quando são consideradas as últimas $N$ sequências com $N$ muito grande, podem ocorrer problemas bem conhecidos, como:

1. **Explosão do Gradiente**: ocorre quando você está utilizando uma sequência muito grande e o peso dessa conexão do loopback é 2, por exemplo.
Pois no fim você acabará tendo um $2^{repeticoes}$, que é uma expressão que cresce muito rápido.
2. **Desaparecimento do Gradiente**: Ocorre quando você tem um peso muito pequeno nessa aresta de loopback e então você eleva um valor menor que 1 muitas vezes
e fica muito próximo de zero.