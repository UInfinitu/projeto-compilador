A gramatica da Peneira, escrita por extenso no primeiro modulo.
Esta e a forma de partida: ainda tem recursao a esquerda e ainda nao esta fatorada.
O modulo de gramaticas livres de contexto retoma este arquivo e registra cada
transformacao com a forma anterior ao lado da forma final.

--- Gramatica hospedeira (a linguagem que o usuario escreve) ---

program     := decl* ;
decl        := patternDecl | ruleBlock ;
patternDecl := "pattern" ID "=" REGEX ";" ;
ruleBlock   := "rule" "{" action* "}" ;
action      := "on" ID "(" ID ")" ( "where" expr )? "=>" "emit" "(" STRING "," expr ")" ";" ;
expr        := andExpr ( "or" andExpr )* ;
andExpr     := cmpExpr ( "and" cmpExpr )* ;
cmpExpr     := primary ( ("<"|">"|"=="|"!="|">="|"<=") primary )? ;
primary     := ID | NUMBER | STRING | "value" "(" ID ")" | "(" expr ")" ;

--- Mini-linguagem regular (o alvo dos automatos) ---

regex  := alt ;
alt    := concat ( "|" concat )* ;
concat := repeat+ ;
repeat := atom ( "*" | "+" | "?" )? ;
atom   := CHAR | "." | "[" classe "]" | "(" alt ")" ;

--- Onde cada nivel da hierarquia de Chomsky comparece ---

A gramatica hospedeira e livre de contexto (tipo 2): as producoes aninhadas de
expr/andExpr/cmpExpr/primary exigem memoria de pilha, e nenhum automato finito as
reconhece. A mini-linguagem regular tambem e descrita por uma gramatica livre de
contexto — porque a NOTACAO de expressao regular tem parenteses aninhados —, mas a
LINGUAGEM que cada expressao denota e regular (tipo 3). Confundir as duas coisas e
o erro mais frequente deste ponto do percurso: o que e regular e o conjunto de
cadeias descrito pela expressao, nao o texto da expressao.


---
Textos, fotos, artes e vídeos deste site estão protegidos pela legislação sobre direito autoral. Não reproduza o conteúdo em qualquer meio de comunicação, eletrônico ou impresso, sem autorização.