# Que classe de padrões o sistema aceita

**Decisão:** movimentos e ataques podem se juntar em combos através dos três
operadores de expressão regular: concatenação (`->`), alternância (`|`) e fecho
(`*`). Um `combo` ou `macro` pode compor outros combos e macros já declarados
dentro da sua própria expressão, sem limite de profundidade.

**Núcleo mínimo:** os três operadores acima — concatenação, alternância e fecho

# Que forma tem a descrição escrita pelo usuário

**Decisão:** um programa é uma sequência de `macro` ou `combo` seguida
de um bloco `arena` (main). Cada declaração associa um nome a uma expressão
regular sobre movimentos e ataques, podendo referenciar macros ou combos já
declarados. Cada ação dentro de `arena` reage a um combo nomeado e produz
saída por `emit`.

# O que o sistema produz

**Decisão:** o objeto gerado tem duas partes — um vetor de autômatos finitos
determinísticos, um por `macro` ou `combo`, na forma de tabelas de transição; e,
para cada `arena`, um bytecode de máquina de pilha que executa o `emit`. Uma
máquina virtual própria varre a entrada, aplica os autômatos com desempate por
casamento mais longo e executa o bytecode.

# Descartado nesta etapa

Timing (combo sincronizado por quadros), personagens assistentes e troca de
personagens foram cogitados, mas nenhum dos três serão feitos a primeiro momento.
Ficam fora do escopo até o núcleo estar fechado.
