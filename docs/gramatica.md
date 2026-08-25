# Gramatica hospedeira (a linguagem que o usuario escreve)

program       := decl* ;
decl          := macroDecl | arenaBlock ;
macroDecl     := "macro" ID "=" MOVIMENTO | ATAQUE | encadeamento ";" ;
encadeamento  := "[" MOVIMENTO | ATAQUE "->" MOVIMENTO | ATAQUE * "]"
ruleBlock     := "arena" "{" action* "}" ;
action        := "golpear" ID "=>" "emit" "(" STRING "," expr ")" ";" ;
expr          := "{" "range" ":" INT "," "dano" ":" INT "}"
