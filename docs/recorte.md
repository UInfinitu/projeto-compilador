# Que classe de padrões o sistema aceita

**Decisão:** ataques e movimentos que podem ser unificados em golpes, tornando-se combos.

**Núcleo mínimo:** encadeamento `->` (x -> x vira y).

# Que forma tem a descrição escrita pelo usuário

**Decisão:** um programa é uma sequência de declarações `pattern` seguida de um bloco `rule`. Cada
`pattern` associa um nome a uma expressão regular; cada ação dentro de `rule` reage ao casamento de
um `pattern` nomeado, opcionalmente condicionada por um `where`, e produz saída por `emit`.

# O que o sistema produz

**Decisão:** o objeto gerado tem duas partes — um vetor de autômatos finitos determinísticos, um por
`macro`, `ataque` ou `movimento`, na forma de tabelas de transição; e, para cada `arena`, um bytecode
de máquina de pilha que executa o `emit`. Uma máquina virtual própria varre a entrada, aplica os
autômatos com desempate por casamento mais longo e executa o bytecode.
