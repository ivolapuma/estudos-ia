# Roteiro de Estudos: IA, LLMs, Agentes e Skills

> Um plano progressivo do fundamento à especialização, com projetos práticos testados em cada fase.

---

## Fase 1 — Fundamentos (2-3 semanas)
**Objetivo:** entender o que acontece "por baixo" de um LLM.

**Semana 1 — Matemática essencial**
- Álgebra linear: vetores, produto escalar, multiplicação de matrizes, intuição geométrica de transformação de espaço
- Probabilidade: distribuições, softmax, entropia e cross-entropy
- Cálculo (intuição): derivada como taxa de variação, gradiente como direção de maior crescimento
- Recurso: séries "Essence of Linear Algebra" e "Essence of Calculus" — 3Blue1Brown (YouTube)

**Semana 2 — Machine Learning clássico**
- Treino/validação/teste, overfitting vs. underfitting
- Regressão linear e logística, métricas (acurácia, precision/recall)
- Recurso: curso de Machine Learning de Andrew Ng (Coursera) — primeiras semanas

**Semana 3 — Redes neurais**
- Perceptron, redes multicamada (MLP), funções de ativação (ReLU, sigmoid)
- Backpropagation: como o erro flui de volta pela rede
- Recurso: "Neural Networks: Zero to Hero" — Andrej Karpathy (YouTube), vídeos de micrograd e makemore

**Projeto prático**: MLP implementado do zero (NumPy puro, sem frameworks) para classificar dígitos, incluindo backpropagation manual. Comparação com a mesma rede em PyTorch (15 linhas) para sentir o quanto o framework abstrai.
📓 `mlp_do_zero.ipynb`

---

## Fase 2 — Arquitetura Transformer e LLMs (3-4 semanas)
**Objetivo:** entender como um LLM realmente funciona.

**Semana 1 — Tokenização e embeddings**
- BPE (Byte Pair Encoding), trade-offs de tamanho de vocabulário
- Embeddings de token, embeddings posicionais
- Recurso: "Let's build the GPT Tokenizer" — Karpathy (YouTube)

**Semana 2 — Mecanismo de atenção**
- Self-attention, Query/Key/Value, scaled dot-product attention (`/ √d`)
- Multi-head attention, causal masking
- Recurso: "Attention is All You Need" (paper) + "The Illustrated Transformer" (Jay Alammar)

**Semana 3 — Arquitetura Transformer completa**
- Encoder vs. decoder, blocos residuais, layer normalization, feed-forward layers
- Recurso: nanoGPT (Karpathy) — menos de 300 linhas, ler linha por linha

**Semana 4 — Treinamento e comportamento de LLMs**
- Pré-treino, fine-tuning/instruction tuning, RLHF/RLAIF
- Leis de escala, context window, temperature/top-p, quantização
- Recurso: Hugging Face NLP Course (capítulos de fine-tuning)

**Projeto prático**: Mini-GPT treinado do zero em texto próprio — tokenização por caractere, self-attention causal, multi-head attention, blocos residuais, treino e geração de texto, tudo implementado e explicado passo a passo.
📓 `mini_gpt_do_zero.ipynb`

---

## Fase 3 — Engenharia de Prompt e Uso Prático (2 semanas)

**Semana 1 — Técnicas fundamentais**
- Zero-shot vs. few-shot, chain-of-thought (CoT), ReAct (base conceitual de agentes)
- Recurso: guia de prompt engineering da Anthropic (docs.claude.com)

**Semana 2 — Estruturação, avaliação e API**
- Tags XML para estruturar prompts, delimitação de dados não confiáveis
- Evals: baseado em regras, baseado em modelo (LLM como juiz), benchmarks (MMLU, HumanEval, GSM8K)
- Prática direta com API (parâmetros: temperature, max_tokens, system prompt)
- Recurso: documentação oficial da API da Anthropic

**Projeto prático**: Harness de comparação sistemática de 4 variantes de prompt (zero-shot, few-shot, chain-of-thought, XML estruturado) contra um dataset de 15 exemplos com gabarito, incluindo parsing/grading separados e teste de consistência entre execuções.
📓 `comparacao_prompts.ipynb`

---

## Fase 4 — RAG (Retrieval-Augmented Generation) (2-3 semanas)

**Semana 1 — Embeddings e busca semântica**
- Embeddings de documento (diferente de embeddings de token), similaridade de cosseno
- Busca exata vs. aproximada (ANN, HNSW)

**Semana 2 — Chunking, indexação e bancos vetoriais**
- Estratégias de chunking (tamanho fixo, semântico, recursivo), metadados
- Bancos vetoriais: Pinecone, Chroma, Weaviate, pgvector
- Busca híbrida (vetorial + BM25)

**Semana 3 — Pipeline completo, re-ranking e limitações**
- Pipeline RAG: query → busca → re-ranking → montagem de prompt → geração com citação
- Por que RAG falha: vocabulário diferente, "lost in the middle", chunking mal ajustado, perguntas de agregação multi-documento

**Projeto prático**: RAG caseiro completo sobre documentos fictícios — chunking, TF-IDF, BM25 implementado à mão, fusão híbrida (Reciprocal Rank Fusion), geração com citação obrigatória, e **testes deliberados de falha** (vocabulário diferente, agregação multi-documento) mostrando exatamente onde e por que RAG ingênuo falha.
📓 `rag_caseiro.ipynb`

---

## Fase 5 — Agentes de IA (4-6 semanas)
**O núcleo do roteiro.**

**Semana 1 — Harness (agent scaffolding)**
- Loop de execução, parsing de saída, gerenciamento de contexto, tratamento de erros, limites de execução
- Por que entender isso primeiro: a maioria dos "bugs de agente" é harness mal projetado, não o modelo

**Semana 2 — Tool use / function calling**
- Como o modelo sugere ações (nunca executa diretamente), design de ferramentas e schemas, mensagens de erro úteis

**Semana 3 — Padrões de arquitetura**
- ReAct, planejamento (direto vs. hierárquico), reflexão/self-critique

**Semana 4 — Frameworks**
- LangChain/LangGraph, CrewAI, AutoGen — opiniões diferentes sobre como construir um harness

**Semana 5 — Multi-agent systems e memória**
- Orquestração, quando vale a pena (raramente), memória de curto/longo prazo, memória episódica

**Semana 6 — Avaliação de agentes**
- Trajetórias vs. respostas únicas, métricas (sucesso, passos, custo, recuperação de erro), LLM como juiz de trajetória

**Projetos práticos (em ordem):**
1. Harness do zero: loop manual sem framework, com ferramentas reais, dispatcher com tratamento de erro, limite de passos. Testado com cliente mock cobrindo caminho feliz, ferramenta inexistente e loop infinito.
   📓 `harness_do_zero.ipynb`
2. O mesmo agente em LangGraph, com tabela comparativa explícita do que o framework automatiza (schema, loop, roteamento, dispatcher, memória, streaming, visualização).
   📓 `langgraph_agente.ipynb`
3. Agente com múltiplas ferramentas (calculadora + RAG da Fase 4 + busca web) e memória de curto/longo prazo entre turnos de uma sessão.
   📓 `agente_multi_ferramentas_memoria.ipynb`
4. Harness de avaliação de agente: rastreamento de trajetória completa (ferramentas chamadas, tokens, passos), taxa de sucesso, teste de consistência entre execuções.
   📓 `harness_avaliacao_agente.ipynb`

---

## Fase 6 — Skills e Especialização de Agentes (2-3 semanas)

**Semana 1 — Formas de especializar um modelo/agente**
- Fine-tuning vs. RAG vs. skills — skills carregam *procedimento* ("como fazer X bem"), não *fatos*
- Recurso: documentação da Anthropic sobre Claude Skills

**Semana 2 — Anatomia e design de uma skill**
- Instruções + metadados de ativação + ferramentas/templates opcionais
- Granularidade (nem genérica demais, nem específica demais)
- Descrições de ativação importam tanto quanto o conteúdo

**Semana 3 — MCP (Model Context Protocol)**
- Servidores MCP (expõem dados/ferramentas) vs. clientes MCP (consomem)
- Skills dizem *como* fazer; MCP dá *acesso* ao que falta
- Recurso: especificação oficial em modelcontextprotocol.io

**Projeto prático**: Criação de uma skill própria (resumo de reunião em formato padronizado), representada como ferramenta de "progressive disclosure" (descrição curta decide ativação, conteúdo completo carrega sob demanda). Harness de avaliação de ativação com casos óbvios, caso de borda e casos negativos, mais comparação de comportamento com/sem a skill e checklist de avaliação qualitativa.
📓 `criando_skill_propria.ipynb`

---

## Fase 7 — Segurança, Limitações e Produção (contínuo)

**Semana 1 — Alucinações**
- Por que acontecem (modelo gera texto plausível, não verificado), onde são mais prováveis, mitigações (RAG com citação, expressar incerteza, validação externa)

**Semana 2 — Jailbreaks e prompt injection**
- Jailbreak (usuário tentando contornar o modelo) vs. prompt injection (conteúdo externo com instruções escondidas)
- Mitigações: delimitação de dados não confiáveis, princípio do menor privilégio, confirmação humana para ações irreversíveis

**Semana 3 — Custos, latência e trade-offs**
- Estrutura de custo por token, streaming, cache de prompt, trade-off multi-agent vs. agente único

**Semana 4 — Deploy real e observabilidade**
- Rate limits e retry com backoff, versionamento de prompts, observabilidade (logging de trajetórias), avaliação contínua

**Projeto prático**: Auditoria de segurança do agente construído nas fases anteriores — teste de prompt injection com "canary token" (segredo plantado que nunca deveria vazar), auditoria automática de permissões (ações de alto risco exigem confirmação?), logging estruturado de trajetórias (JSON Lines) e retry com backoff exponencial testado contra falhas intermitentes simuladas.
📓 `auditoria_seguranca_agente.ipynb`

---

## Notebooks do roteiro (em ordem)

| # | Fase | Notebook | O que testa/constrói |
|---|---|---|---|
| 1 | 1 | `mlp_do_zero.ipynb` | MLP e backprop do zero em NumPy |
| 2 | 2 | `mini_gpt_do_zero.ipynb` | Transformer completo (self-attention, multi-head, blocos) |
| 3 | 3 | `comparacao_prompts.ipynb` | Harness de comparação de técnicas de prompt |
| 4 | 4 | `rag_caseiro.ipynb` | Pipeline RAG completo + testes de falha deliberados |
| 5 | 5 | `harness_do_zero.ipynb` | Harness de agente sem framework |
| 6 | 5 | `langgraph_agente.ipynb` | Mesmo agente em LangGraph, comparado |
| 7 | 5 | `agente_multi_ferramentas_memoria.ipynb` | Agente multi-ferramenta com memória |
| 8 | 5 | `harness_avaliacao_agente.ipynb` | Avaliação de trajetórias completas |
| 9 | 6 | `criando_skill_propria.ipynb` | Skill própria com teste de ativação |
| 10 | 7 | `auditoria_seguranca_agente.ipynb` | Auditoria de segurança e observabilidade |

## Como usar este roteiro
- Siga as fases em ordem — cada uma assume conceitos das anteriores (especialmente Fase 2 → 5 → 6)
- Não pule os projetos práticos: eles fixam o conteúdo muito mais que leitura passiva
- Boa parte dos notebooks reaproveita código de fases anteriores de propósito (ex: o RAG da Fase 4 vira ferramenta na Fase 5) — isso é intencional, reflete como esses conceitos se combinam na prática
- É normal voltar a fases anteriores conforme os projetos revelam lacunas
- A Fase 7 não é um destino final — é uma prática contínua que deveria acompanhar qualquer sistema levado a produção
