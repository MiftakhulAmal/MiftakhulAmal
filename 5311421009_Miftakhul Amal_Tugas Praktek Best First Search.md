Miftakhul Amal (5311421009)

Dalam praktek Best First Search ini diminta untuk mengimplementasikan permainan Tic-Tac-Toe dalam bahasa pemrograman Java. untuk bisa menjalankan praktek ini dengan membuat folder bernama Tic tac Toe yang didalamnya terdapat 6 file yang masing diberi nama State.Java, Seed.Java, GameState.Java, Cell.Java, Board.Java, dan GameMain.Java seperti dibawah ini :

Enumaration State (State.java) :

package tictactoe;

/**  *  Enumeration for the various states of the game  */ public enum
State {  // to save as "GameState.java"    
 PLAYING, DRAW, CROSS_WON, NOUGHT_WON 
}

Penjelasan : Ini adalah enumerasi yang mendefinisikan berbagai keadaan permainan. Terdapat empat kemungkinan keadaan: PLAYING (permainan sedang berlangsung), DRAW (seri), CROSS_WON (pemain 'X' menang), dan NOUGHT_WON (pemain 'O' menang).

Enumaration Seed (Seed.java) :

package tictactoe;

/**  * Enumeration for the seeds and cell contents  */ public enum Seed {  
// to save as "Seed.java"    
 EMPTY, CROSS, NOUGHT 
}

Penjelasan : Ini adalah enumerasi yang mendefinisikan isi sel di papan permainan. Terdapat tiga kemungkinan isi: EMPTY (kosong), CROSS (simbol 'X'), dan NOUGHT (simbol 'O').

Enumaration GameState (GameState.java) :

package tictactoe;

/**  *  Enumeration for the various states of the game  */ public enum 
GameState {  // to save as "GameState.java"    
 PLAYING, DRAW, CROSS_WON, NOUGHT_WON 
} 

Penjelasan : Enum `GameState` digunakan dalam permainan Tic-Tac-Toe untuk menggambarkan berbagai status permainan, seperti saat permainan berlangsung (`PLAYING`), berakhir dengan hasil imbang (`DRAW`), dimenangkan oleh pemain "X" (`CROSS_WON`), atau dimenangkan oleh pemain "O" (`NOUGHT_WON`). Ini membantu dalam mengelola dan mengkomunikasikan status permainan secara efisien.

Class Cell.java :

package tictactoe;

import java.awt.Graphics;
import java.awt.*;
import java.awt.Graphics2D;

public class Cell { 
 //content of this cell (Seed.EMPTY, Seed.CROSS, or Seed.NOUGHT) 
     Seed content; 
    int row, col; // row and column of this cell
  
 /**Constructor to initialize this cell with the specified row and col */ 
   public Cell(int row, int col) {
      this.row = row;
      this.col = col;
      clear(); // clear content
   }
  
   /** Clear this cell's content to EMPTY */ 
   public void clear() {
      content = Seed.EMPTY;
   }
  
   /**Paint itself on the graphics canvas, given the Graphics context */ 
   public void paint(Graphics g) {
      // Use Graphics2D which allows us to set the pen's stroke 
      Graphics2D g2d = (Graphics2D)g;
      g2d.setStroke(new BasicStroke(GameMain.SYMBOL_STROKE_WIDTH,
              BasicStroke.CAP_ROUND, BasicStroke.JOIN_ROUND)); // Graphics2D only

      // Draw the Seed if it is not empty 
      int x1 = col * GameMain.CELL_SIZE + GameMain.CELL_PADDING;
      int y1 = row * GameMain.CELL_SIZE + GameMain.CELL_PADDING;
      if (content == Seed.CROSS) {
         g2d.setColor(Color.RED);
         int x2 = (col + 1) * GameMain.CELL_SIZE - GameMain.CELL_PADDING;
         int y2 = (row + 1) * GameMain.CELL_SIZE - GameMain.CELL_PADDING;
         g2d.drawLine(x1, y1, x2, y2);
         g2d.drawLine(x2, y1, x1, y2);
      } else if (content == Seed.NOUGHT) {
         g2d.setColor(Color.BLUE);
         g2d.drawOval(x1, y1, GameMain.SYMBOL_SIZE, GameMain.SYMBOL_SIZE);
      }
   }
} 

Penjelasan : Ini adalah kelas yang merepresentasikan setiap sel di papan permainan. Beberapa metodenya termasuk:

- 'clear()': Mengosongkan isi sel.
- 'paint(Graphics g)': Menggambar isi sel berdasarkan enum Seed (kosong, 'X', atau 'O').
  
Class Board.java :

package tictactoe;

import java.awt.Color;
import java.awt.Graphics;

/**
 * The Board class models the ROWS-by-COLS game-board.
 */ 
public class Board {
   // package access 
   // composes of 2D array of ROWS-by-COLS Cell instances 
   Cell[][] cells; 
  
   /** Constructor to initialize the game board */ 
   public Board() { 
 
 // allocate the array 
      cells = new Cell[GameMain.ROWS][GameMain.COLS]; 
      for (int row = 0; row < GameMain.ROWS; ++row) {
         for (int col = 0; col < GameMain.COLS; ++col) { 
     // allocate element of array 
            cells[row][col] = new Cell(row, col); 
         }
 
     }
   }
  
   /** Initialize (or re-initialize) the game board */ 
   public void init() {
      for (int row = 0; row < GameMain.ROWS; ++row) {
         for (int col = 0; col < GameMain.COLS; ++col) { 
     // clear the cell content 
            cells[row][col].clear(); 
         }
      }
   }
  
   /** Return true if it is a draw (i.e., no more EMPTY cell) */ 
   public boolean isDraw() {
      for (int row = 0; row < GameMain.ROWS; ++row) {
         for (int col = 0; col < GameMain.COLS; ++col) {
            if (cells[row][col].content == Seed.EMPTY) { 
   // an empty seed found, not a draw, exit 
               return false; 
            }
         }
      }
      return true; // no empty cell, it's a draw
   }
  
   /** Return true if the player with "seed" has won after placing at
       (seedRow, seedCol) */ 
   public boolean hasWon(Seed seed, int seedRow, int seedCol) {
      return (cells[seedRow][0].content == seed   // 3-in-the-row
                 && cells[seedRow][1].content == seed
                 && cells[seedRow][2].content == seed
             || cells[0][seedCol].content == seed // 3-in-the-column
                 && cells[1][seedCol].content == seed 
                 && cells[2][seedCol].content == seed
             || seedRow == seedCol              // 3-in-the-diagonal
                 && cells[0][0].content == seed
                 && cells[1][1].content == seed
                 && cells[2][2].content == seed
             || seedRow + seedCol == 2          // 3-in-the-opposite-diagonal
                 && cells[0][2].content == seed
                 && cells[1][1].content == seed
                 && cells[2][0].content == seed);
   }
  
   /** Paint itself on the graphics canvas, given the Graphics context */ 
   public void paint(Graphics g) {
      // Draw the grid-lines
      g.setColor(Color.GRAY);
      for (int row = 1; row < GameMain.ROWS; ++row) {
         g.fillRoundRect(0, GameMain.CELL_SIZE * row -
GameMain.GRID_WIDHT_HALF,
               GameMain.CANVAS_WIDTH-1, GameMain.GRID_WIDTH,
               GameMain.GRID_WIDTH, GameMain.GRID_WIDTH);
      }
      for (int col = 1; col < GameMain.COLS; ++col) {
         g.fillRoundRect(GameMain.CELL_SIZE * col -
GameMain.GRID_WIDHT_HALF, 0,
               GameMain.GRID_WIDTH, GameMain.CANVAS_HEIGHT - 1,
               GameMain.GRID_WIDTH, GameMain.GRID_WIDTH);
      }
  
      // Draw all the cells 
      for (int row = 0; row < GameMain.ROWS; ++row) {
         for (int col = 0; col < GameMain.COLS; ++col) {
            cells[row][col].paint(g);  // ask the cell to paint itself
         }
      }
   }
} 

Penjelasan : Ini adalah kelas yang merepresentasikan papan permainan secara keseluruhan. Beberapa metodenya termasuk:

- 'init()': Menginisialisasi ulang papan permainan.
- 'isDraw()': Mengecek apakah permainan berakhir seri.
- 'hasWon(Seed seed, int seedRow, int seedCol)': Mengecek apakah pemain dengan Seed tertentu menang setelah menempatkan Seed pada baris dan kolom tertentu.
- 'paint(Graphics g)': Menggambar papan permainan dan seluruh selnya.
  
Class GameMain.java :

package tictactoe;

import java.awt.*;
import java.awt.event.*;
import javax.swing.*; 
/**
 * Tic-Tac-Toe: Two-player Graphic version with better OO design.
 * The Board and Cell classes are separated in their own classes.
 */ 
@SuppressWarnings("serial")
public class GameMain extends JPanel { 
   // Named-constants for the game board 
   public static final int ROWS = 3;  // ROWS by COLS cells
   public static final int COLS = 3; 
public static final String TITLE = "Tic Tac Toe";
  
   // Name-constants for the various dimensions used for graphics drawing 
   public static final int CELL_SIZE = 100; // cell width and height (square)
   public static final int CANVAS_WIDTH = CELL_SIZE * COLS;  // the drawing canvas
   public static final int CANVAS_HEIGHT = CELL_SIZE * ROWS; 
   public static final int GRID_WIDTH = 8;  // Grid-line's width 
   public static final int GRID_WIDHT_HALF = GRID_WIDTH / 2; // Grid-line's half-width 
   // Symbols (cross/nought) are displayed inside a cell, with padding from border 
   
   public static final int CELL_PADDING = CELL_SIZE / 6;
   public static final int SYMBOL_SIZE = CELL_SIZE - CELL_PADDING * 2;
   public static final int SYMBOL_STROKE_WIDTH = 8; // pen's stroke width
 
   private Board board;            // the game board
   private GameState currentState;//the current state of the game
   private Seed currentPlayer;     // the current player
   private JLabel statusBar;     // for displaying status message
  
   /** Constructor to setup the UI and game components */ 
   public GameMain() {
  
      // This JPanel fires MouseEvent 
      this.addMouseListener(new MouseAdapter() {
         @Override
         public void mouseClicked(MouseEvent e) {  // mouse-clicked handler
            int mouseX = e.getX();
            int mouseY = e.getY(); 
            // Get the row and column clicked 
            int rowSelected = mouseY / CELL_SIZE;
            int colSelected = mouseX / CELL_SIZE;
 
            if (currentState == GameState.PLAYING) {
               if (rowSelected >= 0 && rowSelected < ROWS
                     && colSelected >= 0 && colSelected < COLS
                     && board.cells[rowSelected][colSelected].content ==
Seed.EMPTY) {
                  board.cells[rowSelected][colSelected].content =
currentPlayer; // move
                  updateGame(currentPlayer, rowSelected, colSelected); // update currentState
                  // Switch player 
                  currentPlayer = (currentPlayer == Seed.CROSS) ?
Seed.NOUGHT : Seed.CROSS;
               }
            } else {        // game over
               initGame();  // restart the game
            } 
            // Refresh the drawing canvas 
            repaint();  // Call-back paintComponent().
         }
      });
  
      // Setup the status bar (JLabel) to display status message 
      statusBar = new JLabel("         ");
      statusBar.setFont(new Font(Font.DIALOG_INPUT, Font.BOLD, 14));
      statusBar.setBorder(BorderFactory.createEmptyBorder(2, 5, 4, 5));
      statusBar.setOpaque(true);
      statusBar.setBackground(Color.LIGHT_GRAY);
 
      setLayout(new BorderLayout()); 
      add(statusBar, BorderLayout.PAGE_END); // same as SOUTH
      setPreferredSize(new Dimension(CANVAS_WIDTH, CANVAS_HEIGHT + 30)); 
            // account for statusBar in height 
 
      board = new Board();   // allocate the game-board
      initGame();  // Initialize the game variables
   }
  
   /** Initialize the game-board contents and the current-state */ 
   public void initGame() {
      for (int row = 0; row < ROWS; ++row) {
         for (int col = 0; col < COLS; ++col) {
            board.cells[row][col].content = Seed.EMPTY; // all cells empty
         }
      }
      currentState = GameState.PLAYING;  // ready to play
      currentPlayer = Seed.CROSS;        // cross plays first
   }
  
   /** Update the currentState after the player with "theSeed" has placed on (row, col) */ 
   public void updateGame(Seed theSeed, int row, int col) {
      if (board.hasWon(theSeed, row, col)) {  // check for win
         currentState = (theSeed == Seed.CROSS) ? GameState.CROSS_WON :
GameState.NOUGHT_WON;
      } else if (board.isDraw()) {  // check for draw
         currentState = GameState.DRAW;
      } 
      // Otherwise, no change to current state (PLAYING). 
   }
  
   /** Custom painting codes on this JPanel */ 
   @Override
   public void paintComponent(Graphics g){ 
      //invoke via repaint()
      super.paintComponent(g);    // fill background 
      setBackground(Color.WHITE); // set its background color
 
      board.paint(g);  // ask the game board to paint itself 
 
      // Print status-bar message 
      if (currentState == GameState.PLAYING) {
         statusBar.setForeground(Color.BLACK);
         if (currentPlayer == Seed.CROSS) {
            statusBar.setText("X's Turn");
         } else {
            statusBar.setText("O's Turn");
         }
      } else if (currentState == GameState.DRAW) {
         statusBar.setForeground(Color.RED);
         statusBar.setText("It's a Draw! Click to play again.");
      } else if (currentState == GameState.CROSS_WON) {
         statusBar.setForeground(Color.RED);
         statusBar.setText("'X' Won! Click to play again.");
      } else if (currentState == GameState.NOUGHT_WON) {
         statusBar.setForeground(Color.RED);
         statusBar.setText("'O' Won! Click to play again.");
      }
   }
  
   /** The entry "main" method */ 
   public static void main(String[] args) {
      // Run GUI construction codes in Event-Dispatching thread for thread safety 
      javax.swing.SwingUtilities.invokeLater(new Runnable() { 
            public void run() {
            JFrame frame = new JFrame(TITLE); 
     // Set the content-pane of the JFrame to an instance of main JPanel 
            frame.setContentPane(new GameMain());
            frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            frame.pack(); 
            // center the application window
            frame.setLocationRelativeTo(null);  
     frame.setVisible(true);   // show it
         }
      });
   }
}

Penjelasan : Ini adalah kelas utama yang mengontrol permainan dan menangani antarmuka pengguna. Beberapa poin pentingnya termasuk:

- Membuat objek Board dan menginisialisasi permainan.
- Menangani peristiwa klik mouse untuk menempatkan 'X' atau 'O' pada sel yang dipilih.
- Memperbarui keadaan permainan dan menampilkan pesan status seperti "X's Turn," "It's a Draw!", "X Won!", atau "O Won!" pada status bar.
- Method main() digunakan untuk menjalankan aplikasi permainan Tic-Tac-Toe dengan antarmuka grafis.

Dengan kombinasi semua komponen ini, permainan Tic-Tac-Toe dapat berfungsi penuh dalam Java. Permainan ini memungkinkan dua pemain untuk saling bergantian menempatkan 'X' dan 'O' pada papan, dan menentukan pemenang atau hasil seri berdasarkan aturan Tic-Tac-Toe. 

Hasil output kode program diatas ketika kedua pemain belum bermain:

![alt text](https://github.com/MiftakhulAmal/MiftakhulAmal/blob/main/Screenshot%202023-11-05%20221118.png?raw=true)

Hasil output kode program diatas ketika pemain X menang:

![alt text](https://github.com/MiftakhulAmal/MiftakhulAmal/blob/main/Screenshot%202023-11-05%20221319.png?raw=true)

Hasil output kode program diatas ketika pemain O menang:

![alt text](https://github.com/MiftakhulAmal/MiftakhulAmal/blob/main/Screenshot%202023-11-05%20221412.png?raw=true)

Hasil output kode program diatas ketika pemain X dan O imbang:
![alt text](https://github.com/MiftakhulAmal/MiftakhulAmal/blob/main/Screenshot%202023-11-05%20222011.png?raw=true)
