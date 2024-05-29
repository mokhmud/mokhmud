#include <iostream>
#include <vector>
#include <algorithm>
#include <random>

// Функция для вывода игрового поля на экран
void printBoard(const std::vector<std::vector<int>>& board) {
    for (const auto& row : board) {
        for (int num : row) {
            if (num == 0) {
                std::cout << "  ";
            }
            else {
                std::cout << num << " ";
            }
        }
        std::cout << std::endl;
    }
}

// Функция для перемешивания чисел на игровом поле
void shuffleBoard(std::vector<std::vector<int>>& board) {
    std::vector<int> numbers;
    for (int i = 0; i < board.size() * board[0].size(); ++i) {
        numbers.push_back(i);
    }

    std::random_device rd;
    std::mt19937 g(rd());

    std::shuffle(numbers.begin(), numbers.end(), g);

    int k = 0;
    for (int i = 0; i < board.size(); ++i) {
        for (int j = 0; j < board[i].size(); ++j) {
            board[i][j] = numbers[k++];
        }
    }
}

int main() {
    const int SIZE = 4; // Размер игрового поля
    std::vector<std::vector<int>> board(SIZE, std::vector<int>(SIZE, 0));

    shuffleBoard(board);

    while (true) {
        printBoard(board);

        int move;
        std::cout << "Enter number to move (0 to quit): ";
        std::cin >> move;

        if (move == 0) {
            break;
        }

        // Найти позицию числа, которое нужно переместить
        int row = -1, col = -1;
        for (int i = 0; i < SIZE; ++i) {
            for (int j = 0; j < SIZE; ++j) {
                if (board[i][j] == move) {
                    row = i;
                    col = j;
                    break;
                }
            }
            if (row != -1) {
                break;
            }
        }

        // Проверить возможность перемещения и сделать ход
        if (row != -1 && col != -1) {
            // Проверка возможности перемещения
            if (row > 0 && board[row - 1][col] == 0) { // Вверх
                std::swap(board[row][col], board[row - 1][col]);
            }
            else if (row < SIZE - 1 && board[row + 1][col] == 0) { // Вниз
                std::swap(board[row][col], board[row + 1][col]);
            }
            else if (col > 0 && board[row][col - 1] == 0) { // Влево
                std::swap(board[row][col], board[row][col - 1]);
            }
            else if (col < SIZE - 1 && board[row][col + 1] == 0) { // Вправо
                std::swap(board[row][col], board[row][col + 1]);
            }
            else {
                std::cout << "Invalid move! Try again." << std::endl;
            }
        }
        else {
            std::cout << "Number not found! Try again." << std::endl;
        }

        // Проверка на завершение игры
        bool win = true;
        for (int i = 0; i < SIZE * SIZE - 1; ++i) {
            if (board[i / SIZE][i % SIZE] != i + 1) {
                win = false;
                break;
            }
        }

        if (win) {
            printBoard(board);
            std::cout << "Congratulations! You won!" << std::endl;
            break;
        }
    }

    return 0;
}
