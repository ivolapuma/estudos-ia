## Fase 6 — Skills e Especialização de Agentes (2-3 semanas)

### Semana 1 — Formas de especializar um modelo/agente

Antes de entrar em "skills" propriamente ditas, vale mapear as três formas distintas de fazer um modelo se comportar melhor numa tarefa específica — porque "skill" geralmente é uma forma de embalar uma ou mais dessas abordagens.

**Fine-tuning**
- Ajustar os pesos do modelo com exemplos da tarefa específica
- Quando faz sentido: você tem um volume grande de dados de treino de qualidade, e precisa que o comportamento seja consistente sem depender de um prompt longo a cada chamada
- Custo e trade-off: caro, lento de iterar, e "trava" o comportamento no modelo — mudar de ideia depois exige re-treinar

**RAG (revisão da Fase 4)**
- Injetar conhecimento externo no momento da consulta, sem alterar o modelo
- Quando faz sentido: a informação muda com frequência, ou é grande demais para caber num prompt, ou precisa de rastreabilidade (saber de onde veio cada fato)

**Skills (prompting + ferramentas empacotados)**
- Uma skill é, na prática, uma combinação empacotada de: instruções específicas de domínio (como um "prompt system" especializado), opcionalmente ferramentas associadas (scripts, templates, referências), e metadados que dizem *quando* a skill deve ser ativada
- Diferença central em relação a RAG: RAG busca *fatos*; uma skill carrega *procedimento* — "como fazer X direito", não "o que é X"
- Diferença central em relação a fine-tuning: não muda o modelo, é "plugável" e pode ser ativada/desativada por tarefa, sem re-treino

**Recurso específico**: documentação da Anthropic sobre Claude Skills (docs.claude.com) — é a referência mais direta e atualizada do conceito aplicado na prática.

### Semana 2 — Anatomia e design de uma skill

**O que compõe uma skill, tipicamente**
- Um arquivo de instruções (geralmente um `SKILL.md` ou equivalente) descrevendo o que fazer e como fazer
- Metadados de ativação: uma descrição que ajuda o sistema (ou o próprio modelo) a decidir quando aquela skill é relevante para a tarefa em questão
- Opcionalmente: scripts auxiliares, templates de arquivo, exemplos de referência, ou até sub-ferramentas específicas daquele domínio

**Quando encapsular conhecimento vs. dar acesso a ferramentas**
- Encapsular conhecimento faz sentido quando a tarefa é sobre *saber como fazer algo bem* (ex: "como gerar um relatório no formato que esta empresa usa", "convenções de código deste time")
- Dar acesso a ferramentas faz sentido quando a tarefa exige *fazer algo que o modelo não consegue sozinho* (rodar código, consultar um banco de dados, gerar um arquivo binário)
- Na prática, a maioria das skills úteis combina os dois: conhecimento de domínio + as ferramentas necessárias para agir sobre esse conhecimento

**Granularidade: uma skill grande vs. várias pequenas**
- Skills muito amplas ("ajude com qualquer coisa de marketing") tendem a diluir a instrução e competir por ativação com outras skills de forma confusa
- Skills muito específicas e numerosas aumentam o custo de manutenção e a chance de sobreposição confusa entre elas
- Regra prática: uma skill deveria corresponder a um tipo de tarefa que alguém pediria para um colega especialista fazer — nem tão genérica que vira "faça de tudo", nem tão específica que vira uma função de uma linha

**Descrições de ativação importam tanto quanto o conteúdo**
- Se a descrição que determina quando a skill é relevante for vaga, a skill nunca é ativada no momento certo (ou é ativada no momento errado) — esse é um ponto de falha tão comum quanto escrever mal o conteúdo da skill em si

### Semana 3 — MCP (Model Context Protocol) e integração

**O que é MCP**
- Um protocolo padronizado para conectar modelos a fontes de dados e ferramentas externas (bancos de dados, APIs, sistemas de arquivos) de forma uniforme, em vez de cada integração ser construída do zero de um jeito diferente
- Separa claramente dois papéis: **servidores MCP** (expõem dados/ferramentas de um sistema específico) e **clientes MCP** (aplicações, como o Claude, que consomem esses servidores)

**Por que isso importa para skills**
- Skills e MCP resolvem problemas complementares: uma skill diz *como* fazer algo bem; um servidor MCP dá *acesso* a um sistema externo necessário para fazer aquilo
- Exemplo concreto: uma skill de "revisão de contratos segundo o playbook jurídico da empresa" pode usar um servidor MCP que dá acesso ao repositório de contratos da empresa

**Recurso específico**: especificação oficial do MCP (modelcontextprotocol.io) — vale ler a visão geral da arquitetura mesmo sem construir um servidor do zero, só para entender o modelo mental.

---

### Projeto prático da fase

**Criar uma skill própria para um caso de uso real**

1. Escolher uma tarefa que você faz repetidamente e que tem uma forma "certa" de ser feita (ex: gerar um relatório semanal num formato específico, revisar código seguindo convenções do seu time, responder e-mails de um certo tipo seguindo um tom específico)
2. Escrever o conteúdo da skill: instruções claras sobre o procedimento, exemplos de bom e mau resultado, qualquer template ou formato esperado
3. Escrever a descrição de ativação: uma frase que capture precisamente quando essa skill deveria entrar em ação (nem vaga demais, nem restritiva demais)
4. Testar a skill em cenários variados: casos óbvios (deveria ativar e funcionar bem), casos de borda (deveria ativar, mas o conteúdo não cobre bem essa variação — revela lacunas na skill) e casos que não deveriam ativar a skill (testa se a descrição de ativação é precisa o suficiente)
5. Comparar explicitamente o comportamento do modelo com e sem a skill ativa, na mesma tarefa — isso deixa claro o ganho real, em vez de assumir que a skill ajuda

Esse projeto fecha a fase conectando com tudo que veio antes: uma skill bem projetada frequentemente *é* um harness (Fase 5) com instruções de domínio embutidas — ela pode incluir tanto o "como pensar sobre isso" quanto, via MCP, o acesso às ferramentas necessárias para agir.