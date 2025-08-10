# tic-tac-toe-game
A fun two-player Tic-Tac-Toe game built with Java Swing &amp; AWT. Features a clean 3x3 grid, color-coded moves, turn switching, win/draw detection, and automatic game end. Showcases Java GUI design, event handling, and simple game logic—perfect for quick matches and learning Java programming basics
import java.awt.*;
import java.awt.event.*;
import javax.swing.*;

public class TicTacToe implements ActionListener {
    int boardWidth = 600;
    int boardHeight = 650;

    JFrame frame = new JFrame("Tic Tac Toe");
    JLabel textLabel = new JLabel();
    JPanel textPanel = new JPanel();
    JPanel boardPanel = new JPanel();

    JButton[][] buttons = new JButton[3][3];
    boolean playerXTurn = true;
    int movesCount = 0;

    TicTacToe() {
        // Frame setup
        frame.setSize(boardWidth, boardHeight);
        frame.setLocationRelativeTo(null);
        frame.setResizable(false);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLayout(new BorderLayout());

        // Top label setup
        textLabel.setBackground(Color.darkGray);
        textLabel.setForeground(Color.white);
        textLabel.setFont(new Font("Algerian", Font.BOLD, 50));
        textLabel.setHorizontalAlignment(JLabel.CENTER);
        textLabel.setText("TIC-TAC-TOE");
        textLabel.setOpaque(true);

        textPanel.setLayout(new BorderLayout());
        textPanel.add(textLabel, BorderLayout.CENTER);

        frame.add(textPanel, BorderLayout.NORTH);

        // Game board setup
        boardPanel.setLayout(new GridLayout(3, 3));
        initializeButtons();
        frame.add(boardPanel);

        frame.setVisible(true);
    }

    public void initializeButtons() {
        Font font = new Font("Arial", Font.BOLD, 80);
        for (int row = 0; row < 3; row++) {
            for (int col = 0; col < 3; col++) {
                JButton button = new JButton();
                button.setFont(font);
                button.setFocusable(false);
                button.addActionListener(this);
                buttons[row][col] = button;
                boardPanel.add(button);
            }
        }
    }

    public void actionPerformed(ActionEvent e) {
        JButton clicked = (JButton) e.getSource();

        if (!clicked.getText().equals("")) {
            return; // Already marked
        }

        clicked.setText(playerXTurn ? "X" : "O");
        clicked.setForeground(playerXTurn ? Color.blue : Color.red);
        movesCount++;

        if (checkWinner()) {
            textLabel.setText((playerXTurn ? "X" : "O") + " Wins!");
            disableAllButtons();
        } else if (movesCount == 9) {
            textLabel.setText("Draw!");
        } else {
            playerXTurn = !playerXTurn;
            textLabel.setText((playerXTurn ? "X" : "O") + "'s Turn");
        }
    }

    public boolean checkWinner() {
        String symbol = playerXTurn ? "X" : "O";

        // Rows and columns
        for (int i = 0; i < 3; i++) {
            if (
                buttons[i][0].getText().equals(symbol) &&
                buttons[i][1].getText().equals(symbol) &&
                buttons[i][2].getText().equals(symbol)
            ) return true;

            if (
                buttons[0][i].getText().equals(symbol) &&
                buttons[1][i].getText().equals(symbol) &&
                buttons[2][i].getText().equals(symbol)
            ) return true;
        }

        // Diagonals
        if (
            buttons[0][0].getText().equals(symbol) &&
            buttons[1][1].getText().equals(symbol) &&
            buttons[2][2].getText().equals(symbol)
        ) return true;

        if (
            buttons[0][2].getText().equals(symbol) &&
            buttons[1][1].getText().equals(symbol) &&
            buttons[2][0].getText().equals(symbol)
        ) return true;

        return false;
    }

    public void disableAllButtons() {
        for (int i = 0; i < 3; i++)
            for (int j = 0; j < 3; j++)
                buttons[i][j].setEnabled(false);
    }

    public static void main(String[] args) {
        new TicTacToe();
    }
}
