## O núcleo

Um operador e duas folhas.

| Construção     | Papel                                            |
| -------------- | ------------------------------------------------ |
| encadeamento   | núcleo — sequencia movimento com ataque          |
| símbolo        | folha — um símbolo literal do alfabeto           |
| cadeia vazia   | folha — produzida pela redução do opcional       |

## As reduções, em pares

| O usuário escreve | A árvore recebe                        |
| ----------------- | -------------------------------------- |
| `x -> x`          | `x + x`                                |
| `(x)`             | `x` — o grupo não sobrevive à leitura  |
