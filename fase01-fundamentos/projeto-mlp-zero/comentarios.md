Funciona bem — 96% de acurácia. Agora vou montar isso como notebook, com explicações em markdown entre cada etapa.

O notebook está pronto e testado (a implementação foi validada rodando de fato — chegou a 96% de acurácia). Ele usa o dataset `load_digits` do scikit-learn (dígitos 8x8) em vez do MNIST completo, porque não depende de download externo; a lógica é idêntica, e há uma nota no final mostrando como trocar para o MNIST real se quiser.

Estrutura: dados → forward pass → loss → backward pass manual (com explicação da regra da cadeia) → treino → avaliação → comparação bônus com PyTorch. Também deixei sugestões de experimentos no final (mudar taxa de aprendizado, adicionar camada, etc.) para você mexer depois de rodar a versão base.