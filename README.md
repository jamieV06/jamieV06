import tkinter as tk
from tkinter import messagebox

class TicTacToe:
    def __init__(self, root):
        self.root = root
        self.root.title("Tic-Tac-Toe")
        self.board = [[None]*3 for _ in range(3)]  # 3x3 board
        self.current_player = 'X'
        self.create_widgets()
        self.moves = 0

    def create_widgets(self):
        """Creates the 3x3 grid of buttons."""
        self.buttons = [[None]*3 for _ in range(3)]
        for i in range(3):
            for j in range(3):
                self.buttons[i][j] = tk.Button(self.root, text='', font='Arial 20 bold', width=5, height=2,
                                               command=lambda i=i, j=j: self.on_click(i, j))
                self.buttons[i][j].grid(row=i, column=j)

        # Reset button
        self.reset_button = tk.Button(self.root, text="Reset", font='Arial 15 bold', command=self.reset_game)
        self.reset_button.grid(row=3, column=0, columnspan=3, sticky="nsew")

    def on_click(self, i, j):
        """Handles a player's move when a button is clicked."""
        if not self.buttons[i][j].cget('text') and not self.check_winner():
            self.buttons[i][j].config(text=self.current_player)
            self.board[i][j] = self.current_player
            self.moves += 1

            if self.check_winner():
                messagebox.showinfo("Game Over", f"Player {self.current_player} wins!")
            elif self.moves == 9:
                messagebox.showinfo("Game Over", "It's a draw!")
            else:
                self.switch_player()

    def switch_player(self):
        """Switches the turn to the other player."""
        self.current_player = 'O' if self.current_player == 'X' else 'X'

    def check_winner(self):
        """Checks if the current player has won the game."""
        # Check rows and columns
        for i in range(3):
            if all(self.board[i][j] == self.current_player for j in range(3)) or \
               all(self.board[j][i] == self.current_player for j in range(3)):
                return True

        # Check diagonals
        if all(self.board[i][i] == self.current_player for i in range(3)) or \
           all(self.board[i][2-i] == self.current_player for i in range(3)):
            return True

        return False

    def reset_game(self):
        """Resets the game to its initial state."""
        self.current_player = 'X'
        self.moves = 0
        self.board = [[None]*3 for _ in range(3)]
        for i in range(3):
            for j in range(3):
                self.buttons[i][j].config(text='')

# Create the game window
root = tk.Tk()
game = TicTacToe(root)
root.mainloop()
