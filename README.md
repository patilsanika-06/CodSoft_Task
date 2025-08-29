class TicTacToe:
    def __init__(self):
        self.board = [[' ' for _ in range(3)] for _ in range(3)]
        self.current_player = 'X'  # Human player

    def print_board(self):
        for row in self.board:
            print('|'.join(row))
            print('-' * 5)

    def minimax(self, depth, is_maximizing):
        score = self.evaluate_board()
        if score != 0:
            return score
        if self.is_full():
            return 0

        if is_maximizing:
            best_score = float('-inf')
            for i in range(3):
                for j in range(3):
                    if self.board[i][j] == ' ':
                        self.board[i][j] = 'O'  # AI's move
                        score = self.minimax(depth + 1, False)
                        self.board[i][j] = ' '  # Undo move
                        best_score = max(score, best_score)
            return best_score
        else:
            best_score = float('inf')
            for i in range(3):
                for j in range(3):
                    if self.board[i][j] == ' ':
                        self.board[i][j] = 'X'  # Human's move
                        score = self.minimax(depth + 1, True)
                        self.board[i][j] = ' '  # Undo move
                        best_score = min(score, best_score)
            return best_score

    def find_best_move(self):
        best_score = float('-inf')
        best_move = (-1, -1)
        for i in range(3):
            for j in range(3):
                if self.board[i][j] == ' ':
                    self.board[i][j] = 'O'  # AI's move
                    score = self.minimax(0, False)
                    self.board[i][j] = ' '  # Undo move
                    if score > best_score:
                        best_score = score
                        best_move = (i, j)
        return best_move

    def evaluate_board(self):
        # Check rows, columns, and diagonals for a win
        for row in range(3):
            if self.board[row][0] == self.board[row][1] == self.board[row][2] != ' ':
                return 1 if self.board[row][0] == 'O' else -1
        for col in range(3):
            if self.board[0][col] == self.board[1][col] == self.board[2][col] != ' ':
                return 1 if self.board[0][col] == 'O' else -1
        if self.board[0][0] == self.board[1][1] == self.board[2][2] != ' ':
            return 1 if self.board[0][0] == 'O' else -1
        if self.board[0][2] == self.board[1][1] == self.board[2][0] != ' ':
            return 1 if self.board[0][2] == 'O' else -1
        return 0

    def is_full(self):
        return all(cell != ' ' for row in self.board for cell in row)

    def play_game(self):
        while True:
            self.print_board()
            if self.is_full():
                print("It's a draw!")
                break

            # Human's turn
            row, col = map(int, input("Enter your move (row and column): ").split())
            if self.board[row][col] == ' ':
                self.board[row][col] = 'X'
            else:
                print("Invalid move. Try again.")
                continue

            if self.evaluate_board() == -1:
                self.print_board()
                print("You win!")
                break

            # AI's turn
            ai_move = self.find_best_move()
            if ai_move != (-1, -1):
                self.board[ai_move[0]][ai_move[1]] = 'O'
            if self.evaluate_board() == 1:
                self.print_board()
                print("AI wins!")
                break

if __name__ == "__main__":
    game = TicTacToe()
    game.play_game()
