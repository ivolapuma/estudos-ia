Tokenizer e batching validados. Agora vou montar o notebook completo com o modelo Transformer (em PyTorch, já que precisa de autograd e álgebra tensorial eficiente — vai rodar localmente, como o bônus da Fase 1).Todas as células agora são Python válido. Vou copiar para outputs e apresentar.

O notebook implementa um Transformer completo do zero (self-attention causal, multi-head, blocos residuais, embeddings posicionais) usando PyTorch, com tokenização e batching já validados nesta sessão. Requer `pip install torch` na sua máquina para rodar o treino — o ambiente aqui não tem acesso à internet para instalar.

Estrutura: tokenização por caractere → `Head` (atenção única) → `MultiHeadAttention` → `Block` (atenção + feed-forward + residual + layer norm) → `MiniGPT` completo → treino → geração de texto. Cada peça tem comentários ligando de volta aos conceitos da Fase 2 (Q/K/V, máscara causal, `/√d`, por que residual connections importam).

O corpus de exemplo é um texto curto original que escrevi só para demonstração — vale trocar por algo seu (livro em domínio público, suas anotações) para resultados mais interessantes, como indicado nas notas finais.