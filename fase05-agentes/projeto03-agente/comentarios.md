Todas as três ferramentas funcionam. Agora vou testar a sessão com memória (histórico curto prazo entre turnos + memória longo prazo persistida) usando um cliente mock com conversa multi-turno.

Tudo validado, incluindo a persistência de memória entre turnos. Agora vou montar o notebook completo.Agora vou executar as células offline (ferramentas + dispatcher + loop + sessão com memória + teste multi-turno) exatamente como estão no notebook.Ponta a ponta funcionando, incluindo o `assert` de memória. Copiando para outputs.

O notebook combina três ferramentas de tipos diferentes (calculadora, RAG sobre políticas internas — reaproveitando a Fase 4, e busca na web) com uma classe `Session` que mantém:

- **Memória de curto prazo**: o histórico de mensagens cresce ao longo da sessão, em vez de reiniciar a cada pergunta
- **Memória de longo prazo**: fatos extraídos explicitamente (ex: "meu nome é João") são guardados e reinjetados no system prompt em turnos futuros

Testei o cenário completo aqui mesmo com um cliente mock: 3 turnos, onde o terceiro só pode ser respondido corretamente se o agente lembrar o que foi dito no primeiro — passou, incluindo o `assert` final.

A seção 7 (mesma sessão com API real) não pôde ser executada neste sandbox sem internet, mas reutiliza exatamente a mesma classe `Session` — só troca o cliente mock pelo real, com o `system_prompt()` dinâmico agora sendo de fato enviado à API.

Nas notas finais deixei claro que a extração de fatos por regex aqui é só ilustrativa — em produção isso normalmente seria uma chamada de LLM dedicada, com armazenamento via RAG sobre um banco de memórias.