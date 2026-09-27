## Fase 3 — Engenharia de Prompt e Uso Prático (2 semanas)

### Semana 1 — Técnicas fundamentais de prompting

**Zero-shot vs. few-shot**
- Zero-shot: pedir a tarefa diretamente, sem exemplos
- Few-shot: dar 2-5 exemplos de entrada/saída antes da pergunta real — geralmente melhora muito a consistência do formato de resposta, especialmente em tarefas estruturadas (extração de dados, classificação)
- Quando few-shot ajuda pouco: tarefas que dependem mais de raciocínio do que de formato

**Chain-of-thought (CoT)**
- Pedir ao modelo para "pensar passo a passo" antes de dar a resposta final
- Por que funciona: o modelo gera tokens intermediários que funcionam como "espaço de rascunho" — cada token gerado pode ser usado como contexto para o próximo, então raciocínio explícito melhora a qualidade em tarefas que exigem múltiplos passos lógicos (matemática, lógica, planejamento)
- Variações: CoT "livre" vs. CoT estruturado (pedir etapas numeradas, ou tags específicas como `<thinking>`)

**ReAct (Reasoning + Acting)**
- Padrão que intercala raciocínio com ações (chamadas de ferramentas) — o modelo "pensa", decide agir, observa o resultado, pensa de novo
- É a base conceitual de praticamente todo agente moderno — vale revisar esse conceito quando chegar na Fase 5

**Recurso específico**: guia de prompt engineering da própria Anthropic (docs.claude.com/pt/docs/build-with-claude/prompt-engineering) — é atualizado, prático, e escrito pela empresa que constrói os modelos.

### Semana 2 — Estruturação, avaliação e prática com API

**Estruturação de prompts**
- Uso de tags XML (`<contexto>`, `<tarefa>`, `<formato>`) para separar claramente diferentes partes de um prompt — modelos da Anthropic são treinados para reconhecer bem essa estrutura
- Delimitadores para dados não confiáveis (ex: conteúdo de um documento do usuário) — importante para reduzir risco de prompt injection, tema que reaparece na Fase 7
- System prompts vs. mensagens de usuário: quando usar cada um, e por que instruções no system prompt tendem a ser mais "estáveis" ao longo de uma conversa longa

**Avaliação de outputs (evals)**
- Por que "parece bom" não é uma métrica: a necessidade de critérios objetivos e reproduzíveis
- Tipos de eval: baseado em regras (regex, formato exato), baseado em modelo (usar um LLM para julgar a saída de outro), baseado em humano
- Benchmarks conhecidos (MMLU, HumanEval, GSM8K) — o que medem e por que nenhum benchmark isolado conta a história toda
- A armadilha comum: otimizar prompts "no olho" sem um conjunto de testes, e não perceber que uma mudança que melhorou um caso piorou outros três

**Prática direta com API**
- Sair da interface de chat e chamar a API diretamente (Anthropic, OpenAI, ou ambas) — isso expõe parâmetros que a interface esconde: `temperature`, `max_tokens`, `system` prompt separado, `stop_sequences`
- Entender a estrutura de uma requisição (mensagens em formato `role`/`content`) e como isso se conecta com o que foi visto na Fase 2 (contexto = tudo que entra na próxima chamada, já que o modelo não tem memória entre requisições)

**Recurso específico**: documentação oficial da API da Anthropic (docs.claude.com) — fazer as primeiras chamadas via `curl` ou Python antes de usar qualquer framework ajuda a entender exatamente o que está sendo enviado e recebido.

---

### Projeto prático da fase

**Comparação sistemática de prompts para uma mesma tarefa**

1. Escolher uma tarefa concreta (ex: extrair nome, data e valor de um recibo em texto livre)
2. Criar um pequeno dataset de teste: uns 15-20 exemplos de entrada, com a saída correta anotada manualmente
3. Escrever 4-5 variações de prompt para a mesma tarefa: zero-shot puro, few-shot com 3 exemplos, com chain-of-thought, com tags XML estruturando a saída esperada
4. Rodar cada variação contra o dataset via API (não manualmente na interface de chat) e medir taxa de acerto de forma objetiva (ex: comparação exata de campos extraídos)
5. Comparar os resultados: qual técnica teve melhor acerto? Qual foi mais consistente entre execuções (rodar 3x o mesmo prompt e ver se a resposta varia)?

Esse projeto conecta diretamente com a Fase 5: o "harness" de teste que você vai construir aqui (chamar a API, comparar contra gabarito, agregar métricas) é estruturalmente o mesmo tipo de código usado depois para avaliar agentes — só que mais simples.