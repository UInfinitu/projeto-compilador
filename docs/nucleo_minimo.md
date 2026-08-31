## O núcleo

Três operadores e duas folhas — os operadores de expressão regular.

| Construção        | Papel                                                        |
| ----------------- | ------------------------------------------------------------ |
| concatenação `->` | núcleo — sequencia um termo depois do outro                  |
| alternância `\|`  | núcleo — casa um termo ou outro                              |
| fecho `*`         | núcleo — repete um termo zero ou mais vezes                  |
| símbolo           | folha — um símbolo literal do alfabeto (movimento ou ataque) |
| cadeia vazia      | folha — produzida pelas reduções do opcional e do fecho      |

Os três operadores são necessários juntos: sem alternância não há como escrever um
combo que aceita soco ou chute; sem fecho não há como escrever "pressionado várias
vezes"; sem concatenação não há sequência nenhuma.

## As reduções, em pares

| O usuário escreve | A árvore recebe                       |
| ----------------- | ------------------------------------- |
| `x+`              | `concat(x, fecho(x))`                 |
| `x?`              | `alt(x, ε)`                           |
| `[x]` (grupo)     | `x` — o grupo não sobrevive à leitura |

Cada linha troca uma notação de conveniência pela expressão equivalente já escrita
com os três operadores do núcleo — nenhuma delas introduz um operador novo.
