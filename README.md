# Design Snakes and Ladders

Snakes and ladders is an ancient Indian board game that's regarded today as a worldwide classic. It requires two or more players and takes place on a board with numbered, gridded squares. Throughout the board, there are snakes and ladders which connect different squares. Players roll a die and navigate the board. Landing on a ladder advances a player to a square further up the board, while landing on a snake means they have to go back to a previous square.

The aim of the game is to reach the final square.
![Snakes and Ladders](https://cdn.shopify.com/s/files/1/0876/1176/files/i984_pimgpsh_fullsize_distr.png?v=1525140332)


## Problem requirements

* A game can be between multiple players.
* A game will only have human players
* Each player can have multiple pieces
* A board can be of any varying size decided by the client
* A board will different types of cells
* There can be a normal cell and cells with snakes and ladders
* Position of snakes and ladders is random and decided at the start of the game
* The number of snakes and ladders is random and also decided at the start of the game
* The size of snakes and ladders is also random and decided at the start of the game
* A player will move on the basis of a dice
* A player will enter the game only if they get a 1 or maximum face value of the dice
* A player will win if they reach the last cell
* The game will end when all players expect one reach the last cell
* For each game maintain a leaderboard which has the rankings of each player


## Entities
1. Game
   1. start() - start from zero
2. Board
   1. size
   2. register()
   3. Player[]
3. Dice
   1. noOfSides
   2. roll() : int
4. Player - Only human Player
   1. name
   2. email
   3. color
   4. Play(Board)
5. Snakes
   1. startCell
   2. endCell
   3. fall(startCell, endCell)
6. Ladder
   1. startCell
   2. endCell
   3. jump(startCell, endCell)

```mermaid
classDiagram
    class Game{
        -Board board
        -Player[] players
        -Dice dice
        -Leaderboard leaderboard
        -Status status
        +createGame(createGameRequest) void
        +roll(Dice) int
        +makeMove(Player, Piece) 
        +getLeaderboard()
    }
    class Board{
        -int size
        -Cell[] cells
        -Snake[] snakes
        -Ladder[] ladders
    }
    class Cell{
        -int position
        -Piece[] pieces
        -int number
    }
    class Dice{
        -int faceValue
        +roll() int
    }
    class Player{
        -String name
        -String email
        -Piece piece
        +play(Board, piece) Board
    }
    class Piece{
        -String color
    }
    class Snake{
        -Cell start
        -Cell end
        +fall(Cell start, Cell end) int
    }
    class Ladder{
        -Cell start
        -Cell end
        +jump(Cell start, Cell end) int
    }
    class LeaderBoard{
        -Map<Player, Integer> ranking
        +getRanking(Player, int score) ranking
    }

    Game "1" --* "1" Board
    Game "1" --o "1" Dice
    Game "1" --* "M" Player
    Game "1" --* "1" LeaderBoard

    Player "1" --o "M" Piece

    Cell "1" --* "M" Piece

    Board "1" --* "M" Cell
    Board "1" --* "M" Snake
    Board "1" --* "M" Ladder

```

## Problems
* OCP violation in board class


## adding a new Parent class for Snake and Ladder

```mermaid
classDiagram
    class Game{
        -Board board
        -Player[] players
        -Dice dice
        -Leaderboard leaderboard
        -Status status
        +createGame(createGameRequest) void
        +roll(Dice) int
        +makeMove(Player, Piece) 
        +getLeaderboard()
    }
    class Board{
        -int size
        -Cell[] cells
        -Obstacle[] obstacles
    }
    class Cell{
        -int position
        -Piece[] pieces
        -int number
    }
    class Dice{
        -int faceValue
        +roll() int
    }
    class Player{
        -String name
        -String email
        -Piece piece
        +play(Board, piece) Board
    }
    class Piece{
        -String color
    }
    class Obstacle{
        <<abstract>>
        -Cell start
        -Cell end
        +nextLocation(Cell start, Cell end)* int
    }
    class Snake{
        
        +nextLocation(Cell start, Cell end) int
    }
    class Ladder{
        +nextLocation(Cell start, Cell end) int
    }
    class LeaderBoard{
        -Map<Player, Integer> ranking
        +getRanking(Player, int score) ranking
    }
    

    Game "1" --* "1" Board
    Game "1" --o "1" Dice
    Game "1" --* "M" Player
    Game "1" --* "1" LeaderBoard

    Player "1" --o "M" Piece

    Cell "1" --* "M" Piece

    Obstacle <|-- Snake
    Obstacle <|-- Ladder

    Board "1" --* "M" Cell
    Board "1" --* "M" Snake
    Board "1" --* "M" Ladder
    Board "1" --* "M" Obstacle

```

## Optimized Design

```mermaid
classDiagram
    class Game{
        -Board board
        -Player[] players
        -Dice dice
        -Leaderboard leaderboard
        -Status status
        +createGame(createGameRequest) void
        +roll(Dice) int
        +makeMove(Player, Piece) 
        +getLeaderboard()
    }
    class Board{
        -int size
        -Cell[] cells
        
    }
    class Cell{
        -int position
        -Piece[] pieces
        -int number
        -Obstacle[] obstacles
    }
    class Dice{
        -int faceValue
        +roll() int
    }
    class Player{
        -String name
        -String email
        -Piece piece
        +play(Board, piece) Board
    }
    class Piece{
        -String color
    }
    class Obstacle{
        <<abstract>>
        -Cell start
        -Cell end
        +nextLocation(Cell start, Cell end)* int
    }
    class Snake{
        
        +nextLocation(Cell start, Cell end) int
    }
    class Ladder{
        +nextLocation(Cell start, Cell end) int
    }
    class LeaderBoard{
        -Map<Player, Integer> ranking
        +getRanking(Player, int score) ranking
    }
    

    Game "1" --* "1" Board
    Game "1" --o "1" Dice
    Game "1" --* "M" Player
    Game "1" --* "1" LeaderBoard

    Player "1" --o "M" Piece

    Cell "1" --* "M" Piece
    Cell "1" --* "M" Obstacle

    Obstacle <|-- Snake
    Obstacle <|-- Ladder

    Board "1" --* "M" Cell
    Board "1" --* "M" Snake
    Board "1" --* "M" Ladder

```