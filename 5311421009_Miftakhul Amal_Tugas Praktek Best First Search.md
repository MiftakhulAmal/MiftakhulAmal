Miftakhul Amal (5311421009)

Dalam praktek Best First Search ini mengimplementasikan permainan Tic-Tac-Toe dalam bahasa pemrograman Java. untuk bisa menjalankan praktek ini dengan membuat folder bernama Tic tac Toe yang didalamnya terdapat 6 file yang masing diberi nama State.Java, Seed.Java, GameState.Java, Cell.Java, Board.Java, dan GameMain.Java seperti dibawah ini :

Enumaration State (State.java):

package tictactoe;

/**  *  Enumeration for the various states of the game  */ public enum
State {  // to save as "GameState.java"    
 PLAYING, DRAW, CROSS_WON, NOUGHT_WON 
}

Penjelasan : Ini adalah enumerasi yang mendefinisikan berbagai keadaan permainan. Terdapat empat kemungkinan keadaan: PLAYING (permainan sedang berlangsung), DRAW (seri), CROSS_WON (pemain 'X' menang), dan NOUGHT_WON (pemain 'O' menang).
