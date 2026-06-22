# ♟️ Chess System Java

![Java](https://img.shields.io/badge/Java-21-orange?style=flat-square&logo=openjdk)
![Terminal](https://img.shields.io/badge/Interface-Terminal%20ANSI-blue?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

Jogo de xadrez para dois jogadores rodando diretamente no terminal, desenvolvido em Java puro. O projeto implementa todas as regras oficiais do xadrez, incluindo os três movimentos especiais, detecção de xeque e xeque-mate, e interface colorida via códigos ANSI.

---

## 🚀 Tecnologias Utilizadas

- **Java 21**
- **ANSI Escape Codes** — cores e destaque visual no terminal
- **Eclipse IDE** — ambiente de desenvolvimento (projeto Java puro, sem Maven ou Gradle)

---

## 🏗️ Arquitetura

O projeto é dividido em quatro pacotes com responsabilidades bem definidas:

```
src/
├── application/
│   ├── Program.java       # Ponto de entrada — loop principal do jogo
│   └── UI.java            # Interface com o usuário (impressão do tabuleiro, leitura de posições)
│
├── boardgame/             # Camada genérica de tabuleiro (independente do xadrez)
│   ├── Board.java
│   ├── Piece.java
│   ├── Position.java
│   └── BoardException.java
│
├── chess/                 # Regras e lógica do xadrez
│   ├── ChessMatch.java    # Orquestrador da partida
│   ├── ChessPiece.java    # Peça base abstrata
│   ├── ChessPosition.java # Notação de xadrez (a1–h8)
│   ├── Color.java         # Enum WHITE / BLACK
│   └── ChessException.java
│
└── chess/pieces/          # Peças concretas com suas regras de movimento
    ├── King.java
    ├── Queen.java
    ├── Rook.java
    ├── Bishop.java
    ├── Knight.java
    └── Pawn.java
```

A camada `boardgame` é completamente genérica — não sabe nada sobre xadrez. A camada `chess` estende essa base com as regras específicas do jogo.

---

## ♟️ Peças Implementadas

| Símbolo | Peça    | Nome em inglês | Movimento especial        |
|---------|---------|----------------|---------------------------|
| `K`     | Rei     | King           | Roque (kingside/queenside) |
| `Q`     | Rainha  | Queen          | —                         |
| `R`     | Torre   | Rook           | Roque                     |
| `B`     | Bispo   | Bishop         | —                         |
| `N`     | Cavalo  | Knight         | —                         |
| `P`     | Peão    | Pawn           | En Passant, Promoção      |

---

## 🎮 Funcionalidades

- Tabuleiro 8×8 com configuração inicial completa de todas as peças
- Alternância de turnos entre os jogadores (brancas e pretas)
- **Destaque de movimentos possíveis** — ao selecionar uma peça, os destinos válidos são marcados com fundo azul
- **Detecção de xeque** — exibe `CHECK!` quando o rei está em xeque
- **Detecção de xeque-mate** — encerra a partida e anuncia o vencedor
- **Exibição de peças capturadas** — listadas separadamente por cor ao longo da partida
- Validação de jogadas ilegais que colocariam o próprio rei em xeque

---

## 📦 Movimentos Especiais

### Roque (Castling)
Disponível quando o rei e a torre escolhida ainda não se moveram e não há peças entre eles, e o rei não está em xeque. Funciona nos dois lados:
- **Kingside** (lado do rei): o rei move-se duas casas para a direita → torre ocupa a casa à esquerda do rei
- **Queenside** (lado da rainha): o rei move-se duas casas para a esquerda → torre ocupa a casa à direita do rei

### En Passant
Quando um peão avança duas casas no primeiro movimento e passa ao lado de um peão adversário, esse adversário pode capturá-lo na diagonal como se ele tivesse avançado apenas uma casa. A captura só é válida imediatamente no turno seguinte.

### Promoção (Promotion)
Quando um peão alcança a última fileira do adversário, ele é promovido automaticamente. O jogo solicita ao jogador que escolha a peça de substituição:

```
Enter piece for promotion (B/N/R/Q):
```

Opções válidas: `B` (Bispo), `N` (Cavalo), `R` (Torre), `Q` (Rainha).

---

## ▶️ Como Executar

### Pré-requisitos

- **Java 21** ou superior instalado ([download](https://www.oracle.com/java/technologies/downloads/))
- Verificar instalação:

```bash
java -version
```

### ⚠️ Terminal Recomendado

> **Use um terminal com suporte a cores ANSI** para visualizar o tabuleiro corretamente, com as peças coloridas e o destaque de movimentos em azul. Terminais recomendados:
>
> - **Git Bash** (Windows) ✅
> - **Terminal do Linux / macOS** ✅
> - **Windows Terminal** ✅
>
> Evite o **Prompt de Comando (cmd.exe)** padrão do Windows — ele não renderiza as cores ANSI corretamente.

### Clonar o repositório

```bash
git clone https://github.com/MarceloJustin/chess-system-java.git
cd chess-system-java
```

### Compilar

```bash
javac -sourcepath src -d bin src/application/Program.java
```

### Executar

```bash
java -cp bin application.Program
```

---

## 🎯 Como Jogar

O jogo roda inteiramente no terminal. A cada turno:

1. O tabuleiro é exibido com o número do turno e qual cor deve jogar
2. Digite a posição de **origem** da peça (ex: `e2`) e pressione Enter
3. Os movimentos possíveis serão destacados em **azul** no tabuleiro
4. Digite a posição de **destino** (ex: `e4`) e pressione Enter
5. Se um peão for promovido, escolha a nova peça (`B`, `N`, `R` ou `Q`)
6. O jogo termina automaticamente ao detectar xeque-mate

### Notação de posições

As posições seguem a notação padrão do xadrez: coluna de `a` até `h` + linha de `1` até `8`.

```
8 R N B Q K B N R
7 P P P P P P P P
6 - - - - - - - -
5 - - - - - - - -
4 - - - - - - - -
3 - - - - - - - -
2 P P P P P P P P
1 R N B Q K B N R
  a b c d e f g h
```

---

## ⚠️ Tratamento de Erros

Erros são exibidos no terminal e o turno é repetido — nenhuma jogada inválida é aceita.

| Situação                                        | Mensagem                                          |
|-------------------------------------------------|---------------------------------------------------|
| Posição de origem sem peça                      | `There is no piece on source position`            |
| Peça pertencente ao adversário selecionada      | `The chosen piece is not yours`                   |
| Peça sem movimentos disponíveis                 | `There is no possible moves for the chosen piece` |
| Destino inválido para a peça escolhida          | `The chosen piece can't move to target position`  |
| Movimento que colocaria o próprio rei em xeque  | `You can't put yourself in check`                 |
| Formato de posição inválido (ex: `z9`, `aa`)    | `Error reading ChessPosition. Valid values are from a1 to h8.` |

---

## 🧪 Testes

O projeto não possui testes automatizados implementados. A validação foi feita de forma manual durante o desenvolvimento.

---

## 👨‍💻 Autor

**Marcelo Justin**

[![GitHub](https://img.shields.io/badge/GitHub-MarceloJustin-black?style=flat-square&logo=github)](https://github.com/MarceloJustin)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-marcelojustin-blue?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/marcelojustin)

---

## 📄 Licença

Este projeto está licenciado sob a [MIT License](LICENSE).
