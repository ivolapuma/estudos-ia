Núcleo do harness de avaliação validado. Agora vou testar o teste de consistência (mesma tarefa múltiplas vezes, incluindo um caso que falha) antes de montar o notebook.

Toda a lógica validada. Agora vou montar o notebook completo.Agora executo as células offline (ferramentas + harness rastreado + dataset + mock + agregação + consistência) exatamente como no notebook.Ponta a ponta funcionando exatamente como esperado. Copiando para outputs.Com este notebook você fecha os 4 projetos da Fase 5. Ele mede **trajetórias inteiras**, não respostas isoladas:

- `run_agent_traced` registra cada chamada de ferramenta e os tokens consumidos por passo
- Dataset com critério de sucesso flexível (`verificar()` por padrão, já que respostas de agente variam mais em forma que extrações estruturadas)
- Agregação em tabela/gráfico (taxa de sucesso, passos médios, custo total)
- Teste de consistência: rodei 3 execuções da mesma tarefa aqui mesmo, com uma delas propositalmente "errando" — o harness detectou a queda para 67% corretamente
- Bônus: LLM como juiz da trajetória inteira (não só da resposta final), útil para pegar casos onde o agente acerta por sorte mas com um caminho ruim

Tudo isso rodou de ponta a ponta neste sandbox com o cliente mock. A seção de LLM-juiz precisa da API real para rodar de fato.