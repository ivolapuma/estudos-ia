## Fase 2 — Arquitetura Transformer e LLMs (3-4 semanas)

### Semana 1 — Tokenização e Embeddings

**Tokenização**
- Por que LLMs não trabalham com letras nem palavras inteiras, mas com "tokens" (pedaços de palavras)
- Algoritmos: BPE (Byte Pair Encoding) — como um vocabulário é construído a partir de estatísticas de frequência em um corpus de texto
- Trade-offs: vocabulário grande vs. pequeno, como isso afeta custo e desempenho

**Embeddings**
- Como um token (um número inteiro) vira um vetor denso de centenas de dimensões
- Por que embeddings capturam significado semântico (a famosa analogia rei - homem + mulher ≈ rainha)
- Embeddings posicionais: como o modelo sabe a *ordem* das palavras, já que attention por si só é "sem noção de ordem"

**Recurso específico**: vídeo "Let's build the GPT Tokenizer" — Andrej Karpathy (YouTube), implementa BPE do zero.

### Semana 2 — Mecanismo de Atenção

Este é o coração conceitual da fase.

- **Self-attention**: cada token "olha" para todos os outros tokens da sequência e decide quanto peso dar a cada um
- **Query, Key, Value (Q, K, V)**: a analogia mais usada — Query é "o que estou procurando", Key é "o que eu ofereço", Value é "o conteúdo que eu carrego"
- **Scaled dot-product attention**: a fórmula matemática exata, e por que existe a divisão por √d (estabilidade numérica)
- **Multi-head attention**: por que usar várias "cabeças" de atenção em paralelo em vez de uma só (cada cabeça pode aprender a focar em um tipo diferente de relação entre palavras)
- **Causal masking**: por que um modelo generativo (como GPT) só pode "olhar para trás", nunca para tokens futuros

**Recurso específico**: paper "Attention is All You Need" (leitura obrigatória, mesmo que denso) + "The Illustrated Transformer" de Jay Alammar, que traduz o paper em diagramas visuais — leia os dois em paralelo, o blog explica o que o paper assume que você já sabe.

### Semana 3 — Arquitetura Transformer completa

- **Encoder vs. Decoder**: por que modelos como BERT usam só encoder, modelos como GPT usam só decoder, e modelos como T5 usam os dois
- **Blocos residuais e layer normalization**: por que são essenciais para treinar redes muito profundas sem os gradientes "sumirem"
- **Feed-forward layers**: a parte "boba" mas necessária entre os blocos de atenção
- **Empilhamento de blocos**: como dezenas de blocos idênticos empilhados criam a capacidade de um LLM moderno

**Recurso específico**: nanoGPT (Karpathy) — código de menos de 300 linhas que implementa um GPT completo e funcional. Ler o código inteiro, linha por linha, vale mais que qualquer explicação teórica nesse ponto.

### Semana 4 — Treinamento e comportamento de LLMs modernos

- **Pré-treino**: previsão da próxima palavra em bilhões de tokens de texto — de onde vem o "conhecimento" bruto do modelo
- **Fine-tuning / instruction tuning**: por que um modelo pré-treinado "cru" não segue instruções bem, e como o ajuste fino resolve isso
- **RLHF / RLAIF**: como preferências humanas (ou de outro modelo) são usadas para alinhar o comportamento do modelo além do que fine-tuning supervisionado consegue
- **Leis de escala** (scaling laws): a relação entre tamanho do modelo, quantidade de dados e desempenho — e por que isso guiou (e ainda guia) decisões de design na indústria
- **Conceitos práticos do dia a dia**: context window (limite de tokens que o modelo "enxerga" de uma vez), temperature e top-p (como controlam aleatoriedade na geração), quantização (como reduzir o tamanho de um modelo trocando precisão numérica por menos uso de memória)

**Recurso específico**: Hugging Face NLP Course, capítulos sobre fine-tuning — é gratuito e tem exercícios práticos com modelos reais pequenos.

---

### Projeto prático da fase

**Mini-GPT treinado do zero em texto próprio**

Seguindo o espírito da Fase 1 (implementar, não só ler):

1. Pegar o nanoGPT (ou reimplementar a versão minimalista do Karpathy) usando PyTorch
2. Treinar em um corpus pequeno e divertido (ex: todas as obras de um autor, letras de um artista, ou até um livro só seu) — poucos MB de texto já bastam para ver o modelo "aprender o estilo"
3. Gerar texto a partir do modelo treinado e observar como a qualidade muda com: tamanho do modelo, quantidade de dados de treino, número de épocas
4. **Bônus**: implementar tokenização BPE do zero (do vídeo do Karpathy) e comparar o efeito de usar tokenização por caractere vs. BPE no mesmo treino

Esse projeto fecha a Fase 2 com uma experiência completa: você vai ter escrito (ou lido linha por linha) exatamente o mesmo tipo de arquitetura que roda por trás de modelos como GPT, Claude e Llama — só que em escala pequena o suficiente para rodar na sua máquina.