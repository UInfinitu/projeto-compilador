# Que classe de padrões o sistema aceita

**Decisão:** ataques e movimentos que podem ser unificados em golpes, tornando-se combos.

**Núcleo mínimo:** encadeamento `->` (x -> x vira y).

# Que forma tem a descrição escrita pelo usuário

**Decisão:** um programa é uma sequência de declarações `macro` ou `combo` seguida de um bloco `arena` (main). Cada declaração associa o nome do combo a uma expressão regular; Cada ação dentro de `arena` reage à criação de uma nova declaração nomeada que produz a saída por `emit`. As declarações podem opcionalmente serem um "dicionário" para outros combos, envolvendo um combo ou ataque já criado.

# O que o sistema produz

**Decisão:** o objeto gerado tem duas partes — um vetor de autômatos finitos determinísticos, um por
`macro`, `ataque` ou `movimento`, na forma de tabelas de transição; e, para cada `arena`, um bytecode
de máquina de pilha que executa o `emit`. Uma máquina virtual própria varre a entrada, aplica os
autômatos com desempate por casamento mais longo e executa o bytecode.
