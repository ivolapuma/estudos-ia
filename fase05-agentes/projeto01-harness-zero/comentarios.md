Ferramentas e tratamento de erro validados. Agora vou testar o loop do harness com um cliente simulado (mock) da API, cobrindo os cenários: chamada de ferramenta bem-sucedida, ferramenta inexistente e limite de passos.

Todos os três cenários funcionam corretamente: sucesso, erro tratado (a ferramenta fantasma não travou o loop, o resultado de erro voltou ao modelo, que respondeu normalmente), e limite de passos respeitado. Agora vou montar o notebook completo.Agora vou executar as células offline (ferramentas + dispatcher + loop + os 3 cenários mock) exatamente como estão no notebook, para garantir que não houve erro de transcrição.

Todos os cenários offline passaram, incluindo os `assert` de validação. O erro no final é esperado — é a célula que usa a API real, que requer `pip install anthropic` (não disponível neste sandbox). Copiando para outputs.

O notebook implementa o harness completo sem nenhum framework:

1. **Ferramentas reais**: calculadora com parsing seguro via AST (nunca `eval()` puro — importante quando o input pode vir de conteúdo influenciado por prompt injection) e uma ferramenta de clima
2. **Dispatcher** com tratamento de erro (ferramenta desconhecida, argumento faltando, exceção)
3. **O loop do harness** em si — chama modelo, interpreta `tool_use`, executa, devolve resultado, respeita `max_steps`
4. **Testes com cliente mock**: validei aqui mesmo nesta sessão os três cenários (caminho feliz, ferramenta "alucinada" que não trava o sistema, e loop infinito interrompido pelo limite de passos) — todos passaram, inclusive os `assert`
5. **A mesma lógica plugada na API real** da Anthropic — o ponto central do exercício: o `run_agent` não muda nada entre mock e real, só a interface do cliente muda

A seção com API real não pôde ser executada aqui (sandbox sem internet/pacote `anthropic`), mas a lógica que ela reutiliza foi validada de ponta a ponta com o mock.