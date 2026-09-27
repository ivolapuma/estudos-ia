## Fase 5 — Agentes de IA (4-6 semanas)

Esta é a fase mais densa do roteiro — vou dividir em semanas, com o harness logo no início, como conversamos.

### Semana 1 — Harness (agent scaffolding)

**O que é**
- A infraestrutura de código que envolve o LLM e o transforma de "prevê o próximo token" em "age no mundo"
- Componentes essenciais:
  - **Loop de execução**: chamar o modelo → interpretar a resposta → decidir a próxima ação → repetir
  - **Parsing de saída**: extrair chamadas de ferramentas, JSON, comandos da resposta do modelo (que é só texto)
  - **Gerenciamento de contexto**: o que entra na próxima chamada — histórico, resultados de ferramentas, memória, e o que é descartado quando o contexto fica muito longo
  - **Tratamento de erros**: o que fazer quando o modelo alucina uma ferramenta inexistente, passa argumentos errados, ou trava em loop repetindo a mesma ação
  - **Limites de execução**: número máximo de passos, timeout, orçamento de tokens/custo — sem isso, um agente pode rodar indefinidamente

**Por que entender isso primeiro importa**
- A maioria dos "bugs de agente" na prática não é o modelo sendo burro — é o harness mal projetado (contexto mal gerenciado, erros não tratados, sem limite de passos)
- Frameworks (LangChain, LangGraph, etc.) são, no fundo, opiniões diferentes sobre como construir um harness. Entender o conceito nu primeiro evita tratar o framework como caixa-preta

### Semana 2 — Tool use / function calling

- Como o modelo "sugere" uma ação: ele não executa nada, apenas gera texto/JSON estruturado dizendo "eu gostaria de chamar a ferramenta X com os argumentos Y" — quem executa de fato é o harness
- **Design de ferramentas**: nomes descritivos, schemas de parâmetros bem definidos (JSON Schema), descrições claras do que a ferramenta faz e quando usá-la — isso afeta diretamente a taxa de acerto do modelo ao escolher e usar ferramentas
- **Mensagens de erro úteis**: quando uma ferramenta falha, a mensagem de erro que volta para o modelo importa tanto quanto o design da ferramenta em si — um erro genérico ("failed") ajuda muito menos que um erro específico ("arquivo não encontrado: caminho X não existe")
- Paralelização: alguns modelos conseguem sugerir múltiplas chamadas de ferramentas de uma vez — quando isso ajuda e quando complica o harness

### Semana 3 — Padrões de arquitetura de agente

**ReAct (Reasoning + Acting)**
- Padrão que intercala explicitamente raciocínio ("penso que preciso buscar X") com ação (chamar uma ferramenta) e observação (o resultado da ferramenta) — o padrão mais fundamental, base de praticamente tudo que veio depois

**Planejamento**
- Planejamento direto: o agente monta um plano de passos antes de começar a executar
- Planejamento hierárquico: quebrar uma tarefa grande em subtarefas, cada uma podendo ter seu próprio sub-agente ou sub-plano
- Trade-off: planejar tudo de antemão é mais previsível, mas planejar e replanejar dinamicamente conforme novas informações chegam é mais robusto em tarefas com incerteza

**Reflexão / self-critique**
- O agente avalia sua própria saída antes de considerá-la final ("essa resposta realmente responde à pergunta? falta algo?")
- Quando ajuda: tarefas onde erros são caros e há tempo/orçamento para uma segunda passada
- Quando não ajuda: tarefas simples, onde a reflexão só adiciona custo e latência sem ganho real

### Semana 4 — Frameworks

- **LangChain / LangGraph**: o mais popular, LangGraph em particular modela agentes como grafos de estados — bom para fluxos complexos com múltiplos caminhos possíveis
- **CrewAI**: foco em orquestração de múltiplos agentes com papéis definidos (ex: "pesquisador", "escritor", "revisor")
- **AutoGen** (Microsoft): forte em conversação entre múltiplos agentes
- **Abordagem recomendada**: não comece por um framework. Construa o harness manual da Semana 1 primeiro, depois reimplemente a mesma coisa em um framework — você vai reconhecer exatamente o que o framework está fazendo por você, em vez de aceitar como mágica

### Semana 5 — Multi-agent systems e memória

**Multi-agent systems**
- Orquestração: um agente "gerente" delega subtarefas a agentes especializados
- Comunicação entre agentes: mensagens diretas vs. um "quadro compartilhado" (blackboard) que todos podem ler/escrever
- **Quando vale a pena**: tarefas genuinamente paralelizáveis ou que se beneficiam de especialização clara (ex: um agente pesquisa, outro escreve, outro revisa)
- **Quando não vale a pena**: a maioria dos casos — multi-agent adiciona complexidade, custo (múltiplas chamadas de LLM) e novas formas de falha (agentes que se contradizem, loops de comunicação) sem necessariamente melhorar o resultado. Um agente único bem projetado geralmente resolve mais do que a intuição sugere

**Memória de agentes**
- **Curto prazo**: o contexto da sessão atual — geralmente é só a janela de contexto do modelo
- **Longo prazo**: informação persistida entre sessões (preferências do usuário, fatos aprendidos anteriormente) — geralmente implementada como uma forma de RAG (Fase 4) sobre um banco de "memórias"
- **Memória episódica**: guardar não só fatos, mas *experiências passadas* (ex: "da última vez que tentei X, Y não funcionou") — ainda uma área de pesquisa ativa, sem solução padronizada

### Semana 6 — Avaliação de agentes

- Por que é mais difícil que avaliar um LLM isolado: um agente produz uma **trajetória** (sequência de ações), não só uma resposta final — dois agentes podem chegar ao mesmo resultado certo por caminhos muito diferentes, um eficiente e outro desperdiçando chamadas
- Métricas: taxa de sucesso na tarefa, número de passos até completar, custo total (tokens/dinheiro), taxa de recuperação de erros (o agente consegue se corrigir quando uma ferramenta falha?)
- Avaliação automatizada: usar outro LLM para julgar se a trajetória final foi razoável — mesma técnica de "LLM como juiz" vista na Fase 3, agora aplicada a sequências inteiras de ações, não a uma resposta única

---

### Projetos práticos (nesta ordem)

1. **Harness do zero**: loop em Python puro, sem framework, que chama a API, faz parsing de uma tool call, executa uma função real (ex: consulta a uma API de clima) e devolve o resultado ao modelo — com limite de passos e tratamento de erro
2. **O mesmo agente em LangGraph**: reimplementar o projeto 1 usando o framework, comparando explicitamente o que ele automatiza
3. **Agente com múltiplas ferramentas e memória**: um assistente que usa 2-3 ferramentas diferentes (ex: busca na web, RAG sobre seus documentos da Fase 4, uma calculadora) e mantém memória entre perguntas na mesma sessão
4. **Harness de avaliação de agente**: reaproveitando a estrutura do harness de avaliação da Fase 3, agora medindo trajetórias completas — taxa de sucesso, número de passos, custo — em vez de uma única resposta