## Fase 1 — Fundamentos (2-3 semanas)

### Semana 1 — Matemática essencial

**Álgebra linear** (o mais importante dos três)
- Vetores: o que são, soma, produto escalar (dot product) — é literalmente a operação que roda dentro de cada neurônio
- Matrizes: multiplicação de matrizes, transposição — todo o forward pass de uma rede neural é multiplicação de matrizes
- Intuição geométrica: por que multiplicar por uma matriz é "transformar" um espaço (rotação, escala) — isso ajuda muito a entender embeddings depois

**Probabilidade básica**
- Distribuições, especialmente a softmax (transforma números em probabilidades) — é a função que decide qual "próxima palavra" um LLM escolhe
- Entropia e cross-entropy — é a função de perda (loss) que praticamente todo LLM usa para treinar

**Cálculo (intuição, não rigor)**
- Derivada como "taxa de variação" e gradiente como "direção de maior crescimento"
- Você não precisa saber resolver integrais à mão — precisa entender que o gradiente diz "para que lado mexer os pesos para errar menos"

**Recurso específico**: série "Essence of Linear Algebra" e "Essence of Calculus", ambas do 3Blue1Brown (YouTube) — visualização excelente, poucas horas de conteúdo total.

### Semana 2 — Machine Learning clássico

- **Aprendizado supervisionado**: o que é um dataset de treino/validação/teste e por que essa separação existe
- **Overfitting vs. underfitting**: por que um modelo pode "decorar" os dados em vez de aprender o padrão — conceito que reaparece constantemente em LLMs (memorização vs. generalização)
- **Regressão linear e logística**: os modelos mais simples possíveis, mas que já introduzem a ideia central: ajustar pesos para minimizar erro
- **Métricas**: acurácia, precision/recall — importante para depois entender benchmarks de LLM

**Recurso específico**: primeiras semanas do curso de Machine Learning do Andrew Ng (Coursera) — pode assistir só os vídeos, sem fazer os exercícios formais, se o foco é intuição rápida.

### Semana 3 — Redes neurais

- **Perceptron**: a unidade mais simples — soma ponderada + função de ativação
- **Redes multicamada (MLP)**: como empilhar perceptrons cria capacidade de aprender padrões complexos
- **Funções de ativação**: ReLU (a mais usada hoje), sigmoid, por que "não-linearidade" é essencial
- **Backpropagation**: como o erro "flui de volta" pela rede para ajustar cada peso — este é o conceito mais importante da semana

**Recurso específico**: "Neural Networks: Zero to Hero" de Andrej Karpathy — especificamente os dois primeiros vídeos (micrograd e makemore), onde ele constrói backpropagation do zero em Python puro, sem bibliotecas. É denso, mas depois disso você nunca mais vai ver backprop como uma "caixa preta".

---

### Projeto prático da fase

**MLP do zero para classificar dígitos (MNIST)**

O objetivo não é ter o melhor modelo — é implementar cada peça manualmente para entender o que está acontecendo:

1. Carregar o dataset MNIST (imagens de dígitos escritos à mão, 28x28 pixels)
2. Implementar o forward pass usando apenas NumPy (multiplicação de matrizes + ReLU + softmax)
3. Implementar backpropagation manualmente (sem `autograd`, sem PyTorch/TensorFlow)
4. Treinar por algumas épocas e ver a acurácia subir
5. **Bônus**: depois de funcionar do zero, reimplementar em PyTorch em 15 linhas e comparar — você vai *sentir* o quanto o framework está abstraindo

Isso fecha o ciclo: você sai da Fase 1 sabendo exatamente o que uma biblioteca como PyTorch faz por baixo dos panos, o que torna a Fase 2 (Transformers) muito mais intuitiva em vez de mágica.