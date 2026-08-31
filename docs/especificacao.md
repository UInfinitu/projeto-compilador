# KomboScript — especificação do que vai ser construído

Este documento responde às sete perguntas da tarefa, na ordem em que ela as faz.
Descreve comportamento — o que existe na cena, o que a pessoa escreve, o que o
sistema aceita, o que recusa e como avisa. Não há aqui nenhuma linha sobre
estrutura de dados, algoritmo ou biblioteca; essas decisões são de outros
documentos e de outros capítulos.

É a este texto que se volta em cada arco, para conferir se o que está sendo
construído continua sendo o que se pretendia construir.

## 1. O domínio e a cena

KomboScript fala sobre a sequência de comandos que um jogador digita num jogo de
luta — cima, baixo, socos, chutes — à medida que ela chega, um comando de cada
vez, sem pausa entre um e outro. A informação existe no fluxo desses comandos e
precisa ser reconhecida enquanto ele ainda está acontecendo: quando o soco chega,
o motor já precisa saber se aquilo fecha um golpe fraco agora ou se é o começo de
um especial.

Quem escreveria uma descrição nessa linguagem é quem projeta os combos do
personagem — sabe quais sequências devem valer como quê, e quer poder mudar o
repertório sem tocar no código do motor de jogo. Hoje essa pessoa tem duas
saídas: pedir para alguém programar cada combo à mão dentro do motor, o que
custa uma reescrita a cada ajuste; ou aceitar que os combos fiquem espalhados em
condicionais dentro do código do jogo, o que torna qualquer mudança de
repertório uma mudança de comportamento do jogo inteiro.

O que essa pessoa quer é escrever, num arquivo à parte, quais sequências de
comandos valem como quais golpes — e mudar esse arquivo sem recompilar o jogo.

A cena, então: a pessoa escreve uma descrição com os combos do personagem;
entrega essa descrição ao KomboScript; durante a partida, o motor gerado lê os
comandos do jogador um a um e emite o golpe correspondente assim que um combo é
reconhecido.

## 2. O que se escreve na linguagem

Uma descrição do KomboScript tem duas partes: declarações (`macro` ou `combo`),
que dão nome a sequências de comandos, e um bloco `arena`, que diz o que emitir
quando um combo nomeado é reconhecido.

**Primeiro — o mínimo que já é útil.**

```
combo Hadouken = Baixo -> Frente -> Jab;

arena {
    golpear Hadouken => emit("COMBO_ATIVADO", "Hadouken", 80);
}
```

**Segundo — reaproveitando uma sequência com `macro`.** Uma macro nomeia um
trecho comum a mais de um combo; ela não aparece na `arena`, só dentro de outras
declarações.

```
macro RolarFrente = [Baixo -> Frente];

combo Hadouken      = RolarFrente -> [Jab];
combo HadoukenForte = RolarFrente -> [SocoForte];

arena {
    golpear Hadouken      => emit("COMBO_ATIVADO", "Hadouken", 80);
    golpear HadoukenForte => emit("COMBO_ATIVADO", "HadoukenForte", 120);
}
```

**Terceiro — as conveniências de repetição.** `+` exige uma ou mais vezes; `?`
torna um trecho opcional.

```
macro Mashing = ChuteFraco+;
combo Rajada  = Mashing -> ChuteForte;
```

**Quarto — composição aninhada.** Um combo pode compor outro combo já
declarado, e não só macros — é o mecanismo que permite repertórios com golpes
que se encadeiam em golpes maiores.

```
combo Especial      = Hadouken -> SocoForte;
combo EspecialDuplo = Especial -> Especial;
```

Os quatro exemplos sobem em degraus deliberados: o primeiro só tem combo e
emissão; o segundo introduz reaproveitamento por macro; o terceiro, as
conveniências de repetição; o quarto, a composição de combo sobre combo. É essa
progressão que orienta a ordem de construção dos capítulos seguintes.

## 3. O que o sistema aceita e o que recusa

É válida a descrição que declara `macro` e `combo` antes de usá-los, escreve
cada expressão com os operadores da linguagem, e cuja `arena` golpeia apenas
nomes declarados. Tudo o mais é recusado antes de qualquer comando de jogador
ser lido — o KomboScript nunca começa a reconhecer uma partida para descobrir no
meio que a descrição do repertório estava errada.

Toda recusa traz a posição do problema, o que foi encontrado e o que se
esperava ali:

| O que está errado                               | O que a pessoa recebe                                                    |
| ----------------------------------------------- | ------------------------------------------------------------------------ |
| falta o `;` da declaração                       | `posição 34: esperava ";" ao fim da declaração; encontrei "arena"`       |
| colchete aberto e não fechado na expressão      | `posição 18: colchete aberto na posição 12 nunca é fechado`              |
| um operador (`*`, `+`, `?`) sem nada à esquerda | `posição 6: "*" repete o que vem antes, e não há nada antes dele`        |
| `arena` golpeia um nome que não existe          | `posição 8: "Hadoken" não foi declarado; há um combo chamado "Hadouken"` |
| dois `macro`/`combo` com o mesmo nome           | `posição 40: "RolarFrente" já foi declarado antes`                       |

Sobre os comandos do jogador durante a partida, a postura é oposta: uma
sequência que não casa nenhum combo não é erro — é o caso comum, porque a
maior parte do que um jogador digita não é combo nenhum. O motor segue
observando, sem produzir saída.

## 4. Onde a linguagem se aninha

Uma expressão se aninha de duas formas. A primeira é sintática: colchetes
agrupam sem profundidade máxima, e `[[Baixo -> Frente]+ -> Jab]` é escrevível. A
segunda é semântica: um `combo` pode compor outro `combo` já nomeado (seção 2,
quarto exemplo), e essa composição também não tem profundidade fixa.

É essa segunda forma de aninhamento que carrega um pedido que o autômato finito
não atende: um repertório em que um golpe só conta como especial se o número de
socos e de chutes na sequência que o precedeu for igual — uma contagem sem
limite fixo. Nenhum autômato finito reconhece isso, porque contar sem limite
exige memória que cresce com a entrada, e um AFD só tem um número fixo de
estados. O caminho cogitado é a composição de combos nomeados de forma
recursiva (um combo que se refere, direta ou indiretamente, a si mesmo).

## 5. O que se verifica antes de rodar

Antes de compilar a descrição num motor, o sistema confere: todo nome usado
numa expressão foi declarado antes do uso; nenhum nome é declarado duas vezes;
todo nome golpeado na `arena` corresponde a um `combo` declarado (uma `macro`
não pode ser golpeada diretamente — ela existe para compor, não para emitir).
Um `macro`/`combo` declarado e nunca usado gera aviso, não erro.

Há uma tensão ainda não resolvida entre esta seção e a anterior: "declarado
antes do uso" impede exatamente a referência circular que a seção 4 cogita
como caminho para a contagem irrestrita. Registramos a tensão aqui em vez de
escondê-la atrás de uma das duas decisões — ela se resolve quando o requisito
de contagem for de fato implementado, não antes.

## 6. O que o sistema produz, e quem executa

O trabalho é feito em dois momentos separados. O **compilador** lê a descrição,
confere o que a seção anterior enumera e grava um objeto: um vetor de autômatos
finitos determinísticos, um por `macro`/`combo`, mais um bytecode de máquina de
pilha por ação da `arena`. Terminado isso, ele encerra.

O **motor** lê esse objeto e os comandos do jogador, e produz a saída. Ele varre
os comandos da esquerda para a direita e, a cada comando novo, decide
imediatamente — nunca espera o fim de uma sequência para só então resolver qual
combo casou. Essa decisão, uma vez tomada, é irrevogável: mesmo que o próximo
comando revelasse que outro combo "melhor" também teria casado, o motor não
desfaz o que já emitiu. Quando dois combos disputam o mesmo prefixo, a ordem de
declaração decide o desempate.

## 7. A pergunta que vai ser respondida medindo

**A pergunta:** qual o menor número de comandos de antecipação que resolve
todos os conflitos de prefixo do repertório escrito, e que par de golpes é o
que obriga o maior número de comandos de antecipação?

**A grandeza que responde:** o número de comandos de antecipação (lookahead)
que um analisador preditivo precisa para decidir sem ambiguidade, medido sobre
o repertório completo escrito para o personagem.

**A referência de comparação:** um analisador preditivo com um único símbolo de
antecipação, rodando sobre a mesma gramática de combos.

**O resultado que contrariaria a expectativa:** que um único comando de
antecipação baste para resolver todo repertório realista — o que tornaria a
"disputa de prefixo" citada no feedback do módulo anterior um problema raro na
prática, e não o obstáculo central que ele parece ser ao ler a teoria.
