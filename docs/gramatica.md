# Gramática hospedeira (a linguagem que o usuário escreve)

```
program     := decl* ;
decl        := macroDecl | comboDecl | arenaBlock ;

macroDecl   := "macro" ID "=" expressao ";" ;
comboDecl   := "combo" ID "=" expressao ";" ;

expressao   := termo ( "->" termo )* ;
termo       := fator ( "|" fator )* ;
fator       := atomo "*"? ;
atomo       := SIMBOLO | ID | "[" expressao "]" ;

arenaBlock  := "arena" "{" acao* "}" ;
acao        := "golpear" ID "=>" "emit" "(" STRING "," STRING "," INT ")" ";" ;
```

Onde:

- `SIMBOLO` é um terminal de movimento ou ataque (`Cima`, `Baixo`, `Tras`, `Frente`,
  `Jab`, `SocoForte`, `Chute`, `ChuteForte`, `ChuteFraco`, `Agarrar`, ...).
- `ID`, dentro de `atomo`, referencia uma `macro` ou `combo` já declarado, é o
  ponto da gramática onde um combo pode compor outro.
- Em `acao`, o primeiro `STRING` é a categoria do evento (ex.: `"COMBO_ATIVADO"`),
  o segundo é o nome de exibição do combo, e `INT` é o dano.

Vocabulário: `macro` e `combo` são as duas formas de declaração nomeada; `arena` é
o único bloco de reação; `golpear` é a única palavra-chave de ação dentro dele.
