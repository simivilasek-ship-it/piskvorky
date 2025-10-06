#include <stdio.h>

#define SIZE 3

void printBoard(char board[SIZE][SIZE]) {
    printf("\n");
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            printf(" %c ", board[i][j]);
            if (j < SIZE - 1) printf("|");
        }
        printf("\n");
        if (i < SIZE - 1) printf("---+---+---\n");
    }
    printf("\n");
}

int checkWin(char board[SIZE][SIZE], char player) {
    // Rows and columns
    for (int i = 0; i < SIZE; i++) {
        if ((board[i][0] == player && board[i][1] == player && board[i][2] == player) ||
            (board[0][i] == player && board[1][i] == player && board[2][i] == player))
            return 1;
    }
    // Diagonals
    if ((board[0][0] == player && board[1][1] == player && board[2][2] == player) ||
        (board[0][2] == player && board[1][1] == player && board[2][0] == player))
        return 1;
    return 0;
}

int isDraw(char board[SIZE][SIZE]) {
    for (int i = 0; i < SIZE; i++)
        for (int j = 0; j < SIZE; j++)
            if (board[i][j] == ' ')
                return 0;
    return 1;
}

int main() {
    char board[SIZE][SIZE];
    for (int i = 0; i < SIZE; i++)
        for (int j = 0; j < SIZE; j++)
            board[i][j] = ' ';

    char players[2] = {'X', 'O'};
    int current = 0;
    int row, col;

    while (1) {
        printBoard(board);
        printf("Hráč %c, zadej řádek a sloupec (1-3 1-3): ", players[current]);
        if (scanf("%d %d", &row, &col) != 2) {
            printf("Neplatný vstup.\n");
            while (getchar() != '\n'); // Vyčisti vstup
            continue;
        }
        row--; col--;
        if (row < 0 || row >= SIZE || col < 0 || col >= SIZE || board[row][col] != ' ') {
            printf("Neplatná pozice. Zkus znovu.\n");
            continue;
        }
        board[row][col] = players[current];

        if (checkWin(board, players[current])) {
            printBoard(board);
            printf("Hráč %c vyhrál!\n", players[current]);
            break;
        }
        if (isDraw(board)) {
            printBoard(board);
            printf("Remíza!\n");
            break;
        }
        current = 1 - current;
    }
    return 0;
}
