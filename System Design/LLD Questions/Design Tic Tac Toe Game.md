#system-design-v2 #design-questions


### 1. Problem Scope
Design a turn-based Tic-Tac-Toe game for two players on an N × N grid, supporting:
- Valid move placement
- Win detection (row, column, diagonals)
- Draw detection when board is full

### 2. Assumptions
- Exactly 2 players
- Each player has a unique symbol (X / O)
- Single game instance
- Turn-based, synchronous play
- Board size configurable at game start
### 3. Functional Requirements
- Start a new game
- Accept player moves
- Validate moves
- Detect win
- Detect draw
- End game

(Optional)
- Restart game
- Display board

### 4. Core Entities & Responsibilities

| Class            | Responsibility                  |
| ---------------- | ------------------------------- |
| Player           | Holds player identity & symbol  |
| Piece            | Represents mark (X/O)           |
| Board            | Manages grid state & validation |
| GameOrchestrator | Controls game flow              |

### 4. Design Patterns (If Any)
- Orchestrator (Application Service)
- Strategy (future: win evaluation)
- Observer (future: rendering)

### 5. UML Class Diagram
![[Tic Tac Toe UML.png|650]]

### 6. Code

```java
public enum Piece {
    X, O
}
// --------------------------------------------
public class Player {
    private Piece piece;
    private String name;

    public Player(Piece piece, String name) {
        this.piece = piece;
        this.name = name;
    }
}
// --------------------------------------------
public class Board {
    private Piece[][] board;
    private int size;
    private int freeCells;

    public void initGame(int n){
        size = n;
        board = new Piece[size][size];
        freeCells = size*size;

        System.out.println("Board initialized: " + freeCells);
    }

    public int getSize() {
        return size;
    }

    public Piece getCell(int x, int y){
        return x < size && y < size ? board[x][y] : null;
    }
    public boolean freeCellsExist(){
        return freeCells != 0;
    }

    public boolean addPiece(int x, int y, Piece piece){
        if(x >= size || y >= size){
            System.out.println("Invalid cell entered...");
            return false;
        }
        if(board[x][y] != null){
            System.out.println("Cell already taken...");
            return false;
        }
        freeCells--;
        board[x][y] = piece;
        return true;
    }
}
// --------------------------------------------
public class GameOrchestrator {
    private final Deque<Player> players;
    private final Board board;

    public GameOrchestrator(Player player1, Player player2, int n) {
        players = new ArrayDeque<>();
        players.add(player1);
        players.add(player2);

        board = new Board();
        board.initGame(n);
    }

    public void playGame(){
        System.out.println("Starting the game...");

        Scanner scan = new Scanner(System.in);
        while(board.freeCellsExist()){
            Player currPlayer = players.peek();

            //take input
            System.out.println(currPlayer.getName() + " please make a move: ");
            String[] input = scan.nextLine().split(",");
            int x = Integer.parseInt(input[0]);
            int y = Integer.parseInt(input[1]);

            boolean res = board.addPiece(x, y, currPlayer.getPiece());
            if(!res){   //if invalid operation takes place, let the player take another turn
                continue;
            }

            //evaluate if player has won the game
            if(evaluateBoard(x, y, currPlayer.getPiece())){
                System.out.println(currPlayer.getName() + " WINS !!!!");
                scan.close();
                return;
            }

            //switch turns
            players.addLast(players.pollFirst());
        }

        System.out.println("Game Tied!");
        scan.close();
    }

    private boolean evaluateBoard(int row, int column, Piece piece){
        boolean rowMatch = true;
        boolean columnMatch = true;
        boolean diagonalMatch = true;
        boolean antiDiagonalMatch = true;

        for (int i = 0; i < board.getSize(); i++) {     // Check Row
            Piece currCell = board.getCell(row, i);
            if (currCell == null || currCell != piece) {
                rowMatch = false;
                break;
            }
        }

        for (int i = 0; i < board.getSize(); i++) {     // Check Column
            Piece currCell = board.getCell(i, column);
            if (currCell == null || currCell != piece) {
                columnMatch = false;
                break;
            }
        }

        for (int i = 0, j = 0; i < board.getSize(); i++, j++) {     // Check Diagonally
            Piece currCell = board.getCell(i, j);
            if (currCell == null || currCell != piece) {
                diagonalMatch = false;
                break;
            }
        }

        for (int i = 0, j = board.getSize() - 1; i < board.getSize(); i++, j--) {   // Check Anti-Diagonally
            Piece currCell = board.getCell(i, j);
            if (currCell == null || currCell != piece) {
                antiDiagonalMatch = false;
                break;
            }
        }

        return rowMatch || columnMatch || diagonalMatch || antiDiagonalMatch;
    }
}
```

### 7.  Interviewer Follow-up Questions
1. Be ready to answer:
2. How would you support undo?
3. How would you add AI?
4. How would you generalize win rules?
5. How would you unit test this?
6. What changes if players > 2?
