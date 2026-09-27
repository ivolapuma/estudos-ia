# Roteiro de Estudos: IA, LLMs, Agentes e Skills

> Um plano progressivo do fundamento à especialização, com projetos práticos em cada fase.

---

## Fase 1 — Fundamentos (2-3 semanas)
**Objetivo:** entender o que acontece "por baixo" de um LLM.

**Tópicos**
- Matemática essencial: álgebra linear (vetores, matrizes, produto escalar), probabilidade básica, intuição de gradiente/derivadas
- Machine Learning clássico: overfitting, treino/validação/teste, regressão, classificação
- Redes neurais: perceptron, backpropagation, funções de ativação

**Recursos**
- "Neural Networks: Zero to Hero" — Andrej Karpathy (YouTube, gratuito, aprende codificando do zero)

**Projeto prático**
- Implementar uma rede neural simples (MLP) do zero em Python/NumPy, sem frameworks, para classificar dígitos (MNIST)

---

## Fase 2 — Arquitetura Transformer e LLMs (3-4 semanas)
**Objetivo:** entender como um LLM realmente funciona.

**Tópicos**
- Mecanismo de atenção (self-attention)
- Arquitetura Transformer: encoder/decoder, embeddings, tokenização
- Treinamento: pré-treino, fine-tuning, RLHF/RLAIF
- Leis de escala e por que "mais parâmetros" nem sempre é a resposta
- Conceitos práticos: context window, temperature, top-p, quantização

**Recursos**
- "Attention is All You Need" (paper original)
- "The Illustrated Transformer" — Jay Alammar (blog)
- Hugging Face NLP Course (gratuito)

**Projeto prático**
- Implementar um mini-Transformer (nanoGPT do Karpathy) e treinar em um dataset pequeno de texto

---

## Fase 3 — Engenharia de Prompt e Uso Prático (2 semanas)
**Tópicos**
- Técnicas: few-shot, chain-of-thought, ReAct
- Estruturação de prompts (XML tags, delimitadores, instruções claras)
- Avaliação de outputs (evals, benchmarks)

**Projeto prático**
- Construir um conjunto de 20-30 prompts para uma mesma tarefa (ex: extração de dados de texto) e comparar variações sistematicamente, medindo qualidade de forma objetiva

---

## Fase 4 — RAG (Retrieval-Augmented Generation) (2-3 semanas)
**Tópicos**
- Embeddings e busca semântica
- Bancos vetoriais (Pinecone, Chroma, Weaviate)
- Chunking, re-ranking, hybrid search
- Limitações do RAG (o problema geralmente não é "buscar", é "buscar bem")

**Projeto prático**
- Construir um RAG caseiro: indexar um conjunto de documentos seus (PDFs, notas), fazer busca semântica e gerar respostas com citação de fonte

---

## Fase 5 — Agentes de IA (4-6 semanas)
**Objetivo:** o núcleo do roteiro — como LLMs se tornam sistemas que agem.

**5.1 — Harness (agent scaffolding)** *— comece por aqui*
- O que é: a infraestrutura de código que envolve o LLM e o transforma em agente
- Componentes: loop de execução, parsing de saída, gerenciamento de contexto, tratamento de erros, limites de execução (steps, timeout, budget)
- Por que importa: separa "o que o modelo faz" de "o que o código ao redor faz" — a maioria dos bugs de agente está no harness, não no modelo

**5.2 — Tool use / function calling**
- Como o LLM "sugere" uma ação e o harness a executa
- Design de ferramentas: nomes claros, schemas bem definidos, mensagens de erro úteis

**5.3 — Padrões de arquitetura**
- ReAct (raciocínio + ação intercalados)
- Planejamento hierárquico
- Reflexão / self-critique

**5.4 — Frameworks**
- LangChain, LangGraph, CrewAI, AutoGen
- Entender que cada framework é uma opinião diferente sobre como construir um harness

**5.5 — Multi-agent systems**
- Orquestração, comunicação entre agentes
- Quando vale a pena (nem sempre — geralmente adiciona complexidade e custo)

**5.6 — Memória de agentes**
- Curto prazo (contexto da sessão) vs. longo prazo (persistente entre sessões)
- Memória episódica

**5.7 — Avaliação de agentes**
- Por que é mais difícil que avaliar um LLM isolado (trajetórias, não só outputs finais)

**Projetos práticos (em ordem)**
1. Harness do zero: loop em Python sem frameworks, que chama a API, faz parsing de uma tool call, executa uma função real (ex: API do tempo) e devolve o resultado ao modelo
2. Reimplementar o mesmo agente usando LangGraph, comparando a experiência com o harness manual
3. Um agente multi-step com memória (ex: assistente de pesquisa que mantém contexto entre perguntas)

---

## Fase 6 — Skills e Especialização de Agentes (2-3 semanas)
**Tópicos**
- Skills como módulos reutilizáveis de conhecimento/procedimento
- Diferença entre fine-tuning, RAG e skills como formas de especializar comportamento
- Como projetar uma skill: quando encapsular conhecimento vs. dar acesso a ferramentas
- MCP (Model Context Protocol) como padrão emergente de integração

**Projeto prático**
- Criar uma skill própria para um caso de uso real (ex: gerar relatórios num formato específico da sua empresa) e testar como ela muda o comportamento do agente

---

## Fase 7 — Segurança, Limitações e Produção (contínuo)
**Tópicos**
- Alucinações, jailbreaks, prompt injection
- Custos, latência, trade-offs de arquitetura
- Deploy real: rate limits, observabilidade, versionamento de prompts

**Projeto prático**
- Pegar um dos agentes construídos nas fases anteriores e colocá-lo em produção simples (ex: um bot que roda 24/7), monitorando custo e falhas

---

## Como usar este roteiro
- Marque o progresso em cada fase conforme avança
- Não pule os projetos práticos — eles fixam o conteúdo muito mais que leitura passiva
- É normal voltar a fases anteriores conforme os projetos revelam lacunas