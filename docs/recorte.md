# Que classe de padrões o sistema aceita

**Decisão:** ataques e movimentos que podem ser unificados em golpes, tornando-se combos.

**Núcleo mínimo:** encadeamento `->` (x -> x vira y).

--- Hugo parou aqui ---

# Que forma tem a descrição escrita pelo usuário

**Decisão:** um programa é uma sequência de declarações `macro` ou `combo` seguida de um bloco `arena` (main). Cada declaração associa o nome do combo a uma expressão regular; Cada ação dentro de `arena` reage à criação de uma nova declaração nomeada que produz a saída por `emit`. As declarações podem opcionalmente serem um "dicionário" para outros combos, envolvendo um combo ou ataque já criado.

**Descartado:** sintaxe sem nomes, em que a expressão apareceria direto na ação. Nomear o padrão
custa uma declaração a mais e paga em três lugares: a tabela de símbolos passa a ter o que registrar,
a verificação semântica passa a ter o que checar (`on x` com `x` inexistente), e a mesma expressão
pode ser reusada em mais de uma ação sem ser recompilada.

# O que o sistema produz

**Decisão:** o objeto gerado tem duas partes — um vetor de autômatos finitos determinísticos, um por
`pattern`, na forma de tabelas de transição; e, para cada `rule`, um bytecode de máquina de pilha que
avalia o `where` e executa o `emit`. Uma máquina virtual própria varre a entrada, aplica os autômatos
com desempate por casamento mais longo e executa o bytecode.

**Descartado:** interpretar a árvore diretamente, sem emitir objeto. Seria mais curto e apagaria a
etapa que a obra existe para demonstrar: é na emissão que o autômato deixa de ser estrutura interna
do reconhecedor e vira **o próprio código-alvo**, que é o que faz a teoria de autômatos aparecer
duas vezes no artefato.
