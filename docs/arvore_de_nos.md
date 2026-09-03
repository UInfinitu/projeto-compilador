## 1. Tabela de Peças

| Nome da Peça     | Trecho Real da Especificação | Padrão com Açúcar | Padrão no Núcleo                                                          | Forma Linear da Árvore                                    | Nós  |
| :--------------- | :--------------------------- | :---------------- | :------------------------------------------------------------------------ | :-------------------------------------------------------- | :--- |
| **Comando**      | `Baixo -> Frente -> Jab`     | `[A-Za-z]+`       | `concat(alt(D, alt(L, alt(l, eps))), fecho(alt(D, alt(L, alt(l, eps)))))` | `concat(alt('A',alt('B',...)), fecho(alt('A',...)))`      | 126  |
| **Sinal**        | `->`                         | `->`              | `concat('-', '>')`                                                        | `concat('-', '>')`                                        | 3    |
| **Número**       | `80`                         | `[0-9]+`          | `concat(D, fecho(D))`                                                     | `concat(alt('0',alt('1',...)), fecho(alt('0',...)))`      | 40   |
| **Opcional**     | `?`                          | `?`               | `alt(x, eps)`                                                             | `alt(x, eps)`                                             | 3    |
| **Palavra Fixa** | `arena`                      | `arena`           | `concat('a', concat('r', ...))`                                           | `concat('a', concat('r', concat('e', concat('n', 'a'))))` | 9    |

---

## 2. Cobertura Mínima

A linguagem KomboScript utiliza operadores de repetição e opcionalidade em sua especificação de macros e combos. O operador de fecho positivo (`+`) é aplicado diretamente na construção de sequências repetidas de comandos, como demonstrado na regra `Mashing = ChuteFraco+;`, onde a repetição de um comando de forma arbitrária é obrigatória. 

Adicionalmente, a opcionalidade (`?`) encontra seu lugar natural na definição de modificadores de golpes que podem ou não ser incluídos pelo jogador em uma sequência de execução rápida, como em entradas de comandos em que um direcional intermediário pode ser omitido pelo jogador sem invalidar o reconhecimento do combo pelo motor.

---

## 3. Um Par que Converge

Para verificar a consistência da redução ao núcleo, duas escritas diferentes que denotam o mesmo conjunto de caracteres devem resultar na mesma árvore e, consequentemente, na mesma contagem de nós e forma linear idêntica.

* **Expressão A:** `[0-9]+`
* **Expressão B:** `[0-9][0-9]*`

Ambas as expressões descrevem sequências de dígitos numéricos com ocorrência de um ou mais elementos. Quando reduzidas ao núcleo utilizando estritamente a concatenação, alternância e o fecho, ambas produzem a forma linear equivalente correspondente a 40 nós na árvore reduzida, confirmando a consistência do processo de tradução.

---

## 4. Um Par que Não Concorre

A comparação estrutural de árvores sintáticas reduzidas possui limitações inerentes à forma como os operadores são distribuídos, o que significa que expressões semanticamente equivalentes podem gerar estruturas de nós totalmente distintas.

* **Expressão A:** `(a|b)*`
* **Expressão B:** `(a*b*)*`

Ambas as expressões denotam exatamente o mesmo conjunto de cadeias formadas por combinações arbitrárias dos símbolos `a` e `b`. No entanto, a árvore gerada pela primeira expressão resulta em um fecho aplicado diretamente sobre uma alternância simples, enquanto a segunda gera um encadeamento aninhado de fechos sobre subcadeias independentes. 

isso demonstra que a análise puramente baseada na árvore reduzida não serve como um provador de equivalência de linguagens. O comparador estrutural apontará uma divergência de formas, uma vez que a igualdade detectada restringe-se estritamente à sintaxe imediata da redução, exigindo a construção formal de autômatos mínimos para validar a equivalência semântica completa.

---

## 5. Dois Requisitos, Um de Cada Lado

### Requisito Resolvido pela Classe
A validação de intervalos numéricos específicos que parecem exigir lógica aritmética complexa pode, muitas vezes, ser resolvida diretamente por meio de padrões regulares estruturados. Um exemplo prático na especificação de motores de jogo é a validação de quadros de animação ou frames de tolerância de input, que podem ser confinados a um escopo estrito através de expansão de classes e concatenações direcionadas, sem a necessidade de recorrer a estruturas de controle de fluxo externas ou contadores dinâmicos durante a fase de análise léxica.

### Requisito Fora da Classe
O reconhecimento de combos baseados na contagem estrita e ilimitada de um número variável de socos e chutes precedentes excede a capacidade de um autômato finito. Para que o sistema verifique se o número de comandos acumulados em uma sequência arbitrária atende a uma condição de igualdade dinâmica, a máquina precisaria dispor de uma memória com capacidade de crescimento ilimitado, característica incompatível com a estrutura de estados finitos descrita pelas expressões regulares e árvores de análise léxica tradicionais.

---

## 6. Quatro Recusas com Posição

1. **Falta de fechamento de grupo:**
   * *Mensagem:* `posição 18: colchete aberto na posição 12 nunca é fechado`
2. **Repetição sem operando:**
   * *Mensagem:* `posição 6: "*" repete o que vem antes, e não há nada antes dele`
3. **Classe sem colchete final:**
   * *Mensagem:* `posição 22: esperava "]" para fechar a classe de caracteres`
4. **Símbolo sobrando após o fim da expressão:**
   * *Mensagem:* `posição 45: token inesperado encontrado após o término da declaração válida`
