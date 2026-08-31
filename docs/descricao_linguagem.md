# KomboScript

- É uma linguagem de combos para um jogo de luta: cada combo é reconhecido em
  fluxo, à medida que os comandos chegam, e a decisão de emitir uma saída é
  irrevogável assim que ocorre.

- Comandos básicos (movimentos e ataques) são combinados em `macro` ou `combo`
  através de três operadores de expressão regular: concatenação (`->`),
  alternância (`|`) e fecho (`*`).

Tipos de declaração: `macro`, `combo`

Comandos permitidos (símbolos de movimento/ataque):

- Cima
- Baixo
- Tras
- Frente
- Jab
- SocoForte
- Chute
- ChuteForte
- ChuteFraco
- Agarrar

Exemplo:

- `combo Voadora = Cima -> Frente -> Chute;`
