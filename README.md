package TicTacToe;
import java.util.Random;
import java.util.Scanner;

class TicTacToe {
    static char[][] board;
    public TicTacToe() {
        board = new char[3][3];
        initBoard();
    }
    void initBoard() {
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[i].length; j++) {
                board[i][j] = ' ';
            }
        }
    }

    static void dispBoard() {
        System.out.println("-------------");
        for (int i = 0; i < board.length; i++) {
            System.out.print("| ");
            for (int j = 0; j < board[i].length; j++) {
                System.out.print(board[i][j] + " | ");
            }
            System.out.println();
            System.out.println("-------------");
        }
    }

    static void placeMark(int row, int col, char mark) {
        if (row >= 0 && row <= 2 && col >= 0 && col <= 2) {
            board[row][col] = mark;
        } else {
            System.out.println("Invalid Position");
        }
    }

    static boolean checkColWin() {

        for (int j = 0; j <= 2; j++) {
            if (board[0][j] != ' ' && board[0][j] == board[1][j] && board[1][j] == board[2][j]) {

                return true;
            }
        }
        return false;
    }

    static boolean checkRowWin() {
        for (int i = 0; i <= 2; i++) {
            if (board[i][0] != ' ' && board[i][0] == board[i][1] && board[i][1] == board[i][2]) {

                return true;
            }
        }
        return false;
    }

    static boolean checkDiagWin() {
        if (board[0][0] != ' ' && board[0][0] == board[1][1] && board[1][1] == board[2][2] ||
                board[0][2] != ' ' && board[0][2] == board[1][1] && board[1][1] == board[2][0]) {

            return true;
        }
        return false;
    }

    static boolean checkDraw() {
        for (int i = 0; i <= 2; i++) {
            for (int j = 0; j <= 2; j++) {
                if (board[i][j] == ' ') {
                    return false;    //it is not a draw as it is empty so return false
                }
            }
        }
        return true; // it is draw as non of thr block is empty annd nobody wins
    }
}

//part 2//
abstract class Player {
    String name;
    char mark;

    abstract void makeMove();

    boolean isValidMove(int row, int col) {
        if (row >= 0 && row <= 2 && col >= 0 && col <= 2) {                           //within the boundary
            if (TicTacToe.board[row][col] == ' ') {                                  //block u placing move is empty or not
                return true;
            }
        }
        return false;
    }
}


class HumanPlayer extends Player {
    HumanPlayer(String name, char mark) {
        this.name = name;
        this.mark = mark;
    }

    @Override
    void makeMove() {
        int row;
        int col;
        Scanner scan = new Scanner(System.in);
        do {
            System.out.println("Enter the Row and Coloum");
            row = scan.nextInt();
            col = scan.nextInt();
        } while (!isValidMove(row, col));
        TicTacToe.placeMark(row, col, mark);
    }
}

class Ai_Player extends Player {


    Ai_Player(String name, char mark) {
        this.name = name;
        this.mark = mark;
    }

    @Override
    void makeMove() {
        int row;
        int col;
        Scanner scan = new Scanner(System.in);
        do {
            // code for AI//
            Random r = new Random();                                                   //Random has a methodnextInt() which generate a random interger
            row = r.nextInt(3);                                                                         //  just need to tell it the bopundry
            col = r.nextInt(3);


        } while (!isValidMove(row, col));

        TicTacToe.placeMark(row, col, mark);
    }
}


public class Launch_Game {
    public static void main(String[] args) {
        TicTacToe t = new TicTacToe();
        HumanPlayer p1 = new HumanPlayer("Bob", 'x');

        Ai_Player p2 = new Ai_Player("Grin", 'o');

        Player cp;                                                   //parent class is player so for AI logic Player
        cp = p1;                                                     // cp is player1//current player
        while (true) {
            System.out.println(cp.name + " Turn");
            cp.makeMove();

            TicTacToe.dispBoard();

            if (TicTacToe.checkColWin() || TicTacToe.checkRowWin() || TicTacToe.checkDiagWin()) { //1st check for win if its a win announce the winner & break;
                System.out.println(cp.name + " Has Won ");
                break;
            }
            else if(TicTacToe.checkDraw()){ //if check draw is true ., it will print below msg//
                System.out.println(" Game is Draw");
                break;                                               // after announcing a draw condition it should break//
            }
            else {                                                     //if it's not a draw change the player and carry on//
                if (cp == p1) {
                    cp = p2;
                } else {
                    cp = p1;
                }
            }

        }
    }
}

