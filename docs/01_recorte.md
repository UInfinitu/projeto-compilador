# O recorte da Peneira — decisões fixadas no primeiro módulo

Registro das três decisões que a Tarefa 1 pede, na forma em que ficarão travadas para todo o
percurso. Cada uma vem acompanhada da alternativa descartada, porque é a comparação que torna a
decisão compreensível quando ela precisar ser revisitada.

## Que classe de padrões o sistema aceita

**Decisão:** expressões regulares com concatenação, alternância (`|`), fecho (`*`), fecho positivo
(`+`), opcional (`?`), classe de caracteres (`[...]`), coringa (`.`) e agrupamento por parênteses.

**Núcleo mínimo:** concatenação, alternância e fecho. Os outros três são conveniência de escrita e
serão **reduzidos ao núcleo** antes de qualquer processamento — `a+` vira `aa*`, `a?` vira `(a|ε)`,
e uma classe `[abc]` vira `(a|b|c)`. A redução acontece uma única vez, logo depois da leitura, e
tudo o que vem depois trabalha só com três operadores.

**Descartado:** grupos de captura e retrovisores (*backreferences*). Não é economia de esforço — é
teoria: retrovisor sai da classe das linguagens regulares, e um sistema que o aceitasse não poderia
ser compilado para autômato finito. A decisão de recusá-lo é o que mantém o artefato coerente com o
que a obra demonstra.

## Que forma tem a descrição escrita pelo usuário

**Decisão:** um programa é uma sequência de declarações `pattern` seguida de um bloco `rule`. Cada
`pattern` associa um nome a uma expressão regular; cada ação dentro de `rule` reage ao casamento de
um `pattern` nomeado, opcionalmente condicionada por um `where`, e produz saída por `emit`.

A gramática completa está em `docs/01_gramatica.txt`, e o exemplo canônico em
`exemplos/exemplo01.pen`.

**Descartado:** sintaxe sem nomes, em que a expressão apareceria direto na ação. Nomear o padrão
custa uma declaração a mais e paga em três lugares: a tabela de símbolos passa a ter o que registrar,
a verificação semântica passa a ter o que checar (`on x` com `x` inexistente), e a mesma expressão
pode ser reusada em mais de uma ação sem ser recompilada.

## O que o sistema produz

**Decisão:** o objeto gerado tem duas partes — um vetor de autômatos finitos determinísticos, um por
`pattern`, na forma de tabelas de transição; e, para cada `rule`, um bytecode de máquina de pilha que
avalia o `where` e executa o `emit`. Uma máquina virtual própria varre a entrada, aplica os autômatos
com desempate por casamento mais longo e executa o bytecode.

**Descartado:** interpretar a árvore diretamente, sem emitir objeto. Seria mais curto e apagaria a
etapa que a obra existe para demonstrar: é na emissão que o autômato deixa de ser estrutura interna
do reconhecedor e vira **o próprio código-alvo**, que é o que faz a teoria de autômatos aparecer
duas vezes no artefato.


---
Textos, fotos, artes e vídeos deste site estão protegidos pela legislação sobre direito autoral. Não reproduza o conteúdo em qualquer meio de comunicação, eletrônico ou impresso, sem autorização.