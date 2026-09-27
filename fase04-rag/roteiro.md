## Fase 4 — RAG (Retrieval-Augmented Generation) (2-3 semanas)

### Semana 1 — Embeddings e busca semântica

**Embeddings de texto (revisão e aprofundamento)**
- Diferença entre embeddings de *token* (vistos na Fase 2, internos ao modelo) e embeddings de *documento/sentença* (usados em RAG) — estes últimos são gerados por modelos especializados para representar o significado de um texto inteiro em um único vetor
- Modelos de embedding comuns: `text-embedding-3` (OpenAI), `voyage-3` (Voyage AI, usado pela Anthropic), modelos open-source como os da família `bge` ou `e5`
- Dimensionalidade: por que vetores de 768, 1024 ou 1536 dimensões, e o trade-off entre dimensão maior (mais expressivo) e custo de armazenamento/busca

**Similaridade e busca**
- Similaridade de cosseno: a métrica padrão para comparar dois embeddings
- Por que "textos com significado parecido ficam próximos no espaço vetorial" — a mesma ideia geométrica de embeddings vista na Fase 2, agora aplicada a documentos inteiros
- Busca exata (força bruta) vs. busca aproximada (ANN — Approximate Nearest Neighbors): por que força bruta não escala além de alguns milhares de documentos

**Recurso específico**: artigo "Understanding Embeddings" da documentação da OpenAI ou Anthropic, mais o paper original do algoritmo HNSW (Hierarchical Navigable Small World) se quiser entender como bancos vetoriais fazem busca aproximada rapidamente — não é obrigatório, mas ajuda a entender por que alguns bancos vetoriais são mais rápidos que outros.

### Semana 2 — Chunking, indexação e bancos vetoriais

**Chunking (divisão de documentos)**
- Por que não se pode simplesmente jogar um documento inteiro num embedding: perda de granularidade e limite de tokens do modelo de embedding
- Estratégias: chunking por tamanho fixo (ex: 500 tokens com overlap de 50), chunking semântico (dividir em pontos de quebra natural — parágrafos, seções), chunking recursivo (tentar por parágrafo, se muito grande cair para frase, se ainda grande cair para caractere)
- O trade-off central: chunks pequenos = mais precisos na busca, mas perdem contexto; chunks grandes = mais contexto, mas a busca fica menos precisa (um chunk grande "dilui" o sinal semântico do trecho relevante)

**Metadados e indexação**
- Anexar metadados a cada chunk (fonte, data, seção, autor) para permitir filtros além da busca semântica pura
- Bancos vetoriais: Pinecone (gerenciado), Chroma (local/embarcado, ótimo para prototipagem), Weaviate, pgvector (extensão do Postgres — útil se você já usa Postgres e não quer mais uma peça de infraestrutura)
- Índice híbrido: combinar busca vetorial (semântica) com busca por palavra-chave tradicional (BM25) — cada uma captura um tipo de relevância diferente (semântica vs. correspondência exata de termos)

**Recurso específico**: documentação do LangChain ou LlamaIndex sobre estratégias de chunking (mesmo sem usar o framework inteiro, a documentação é um bom mapa das opções existentes e quando usar cada uma).

### Semana 3 — Re-ranking, arquitetura RAG completa e limitações

**Pipeline RAG completo**
1. Consulta do usuário → embedding da consulta
2. Busca dos top-k chunks mais similares no banco vetorial
3. **Re-ranking** (opcional mas importante): um modelo mais lento e mais preciso reordena os top-k candidatos antes de decidir quais realmente entram no prompt — a busca vetorial inicial prioriza velocidade sobre precisão, o re-ranker corrige isso
4. Montagem do prompt final: contexto recuperado + pergunta do usuário + instruções
5. Geração da resposta pelo LLM, idealmente com citação de qual chunk fundamenta cada afirmação

**Hybrid search**
- Combinar scores de busca vetorial e busca por palavra-chave (BM25), geralmente com uma fórmula de fusão (ex: Reciprocal Rank Fusion) — resolve casos onde a busca semântica falha (ex: buscar por um código de produto específico, um nome próprio incomum, uma sigla)

**Por que RAG falha na prática — e por que isso importa mais do que a implementação em si**
- **Falha na recuperação**: a pergunta do usuário é vaga ou usa vocabulário diferente do documento (esse é o problema mais comum e mais subestimado)
- **Contexto perdido no meio**: LLMs tendem a prestar menos atenção a informação no meio de um contexto muito longo ("lost in the middle") — jogar muitos chunks no prompt não resolve, pode até piorar
- **Chunking mal ajustado**: um chunk que corta uma tabela ou uma definição ao meio destrói a informação, mesmo com busca perfeita
- **Perguntas que exigem múltiplos documentos ou raciocínio agregado** (ex: "quantos contratos venceram no último trimestre?"): busca por similaridade não resolve isso — é fundamentalmente um problema de busca, não de agregação, e RAG "ingênuo" não foi desenhado para isso

**Recurso específico**: pesquisar por "RAG failure modes" ou "advanced RAG techniques" — há bons posts de engenharia (Pinecone, LlamaIndex, Anthropic) documentando casos reais de falha e como mitigar cada um.

---

### Projeto prático da fase

**RAG caseiro sobre seus próprios documentos**

1. Reunir um conjunto pequeno de documentos seus (PDFs, notas, artigos — algo que você conhece bem, para poder avaliar se as respostas estão certas)
2. Implementar o pipeline completo: extrair texto → chunking → gerar embeddings → indexar num banco vetorial local (Chroma é o mais simples para começar)
3. Implementar a busca: dada uma pergunta, recuperar os top-k chunks mais relevantes
4. Montar o prompt final com os chunks recuperados e gerar a resposta via API, **exigindo que o modelo cite de qual chunk tirou cada informação**
5. Testar deliberadamente com perguntas que você sabe que vão *falhar* (vocabulário diferente do documento, pergunta que exige agregar informação de vários chunks) — isso ensina mais que só testar os casos fáceis
6. **Bônus**: adicionar um re-ranker simples e comparar a qualidade da resposta antes/depois

Esse projeto conecta com a Fase 5: RAG é, na prática, uma das "ferramentas" mais comuns que um agente usa — entender bem como ele funciona (e onde falha) evita o erro comum de tratar RAG como uma caixa preta dentro de um agente maior.