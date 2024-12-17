#include <amp.h>
#include <iostream>
#include <vector>

using namespace concurrency;
using namespace std;

const int SIZE = 256; // Размерность матриц (должна быть одинаковой для квадратных матриц)

void matrixMultiplication(const array_view<int, 2>& A, const array_view<int, 2>& B, array_view<int, 2>& C) {
    parallel_for_each(C.extent, [=](index<2> idx) restrict(amp) {
        int sum = 0;
        for (int k = 0; k < SIZE; ++k) {
            sum += A[idx[0]][k] * B[k][idx[1]];
        }
        C[idx] = sum;
    });
}

int main() {
    // Инициализация матриц A и B вручную
    int A[SIZE][SIZE];
    int B[SIZE][SIZE];

    // Заполнение матрицы A
    cout << "Введите элементы матрицы A (" << SIZE << "x" << SIZE << "):" << endl;
    for (int i = 0; i < SIZE; ++i) {
        for (int j = 0; j < SIZE; ++j) {
            cin >> A[i][j];
        }
    }

    // Заполнение матрицы B
    cout << "Введите элементы матрицы B (" << SIZE << "x" << SIZE << "):" << endl;
    for (int i = 0; i < SIZE; ++i) {
        for (int j = 0; j < SIZE; ++j) {
            cin >> B[i][j];
        }
    }

    // Объявление массивов для AMP
    array<int, 2> A_array(SIZE, SIZE, reinterpret_cast<int*>(A));
    array<int, 2> B_array(SIZE, SIZE, reinterpret_cast<int*>(B));
    array<int, 2> C_array(SIZE, SIZE);

    // Использование array_view для передачи массивов на GPU
    array_view<int, 2> A_view = A_array;
    array_view<int, 2> B_view = B_array;
    array_view<int, 2> C_view = C_array;

    // Выполнение сопоставления матриц на GPU
    matrixMultiplication(A_view, B_view, C_view);

    // Синхронизация и получение результата
    C_view.synchronize();

    // Вывод результата
    cout << "Результат матричного умножения C = A * B:" << endl;
    for (int i = 0; i < SIZE; ++i) {
        for (int j = 0; j < SIZE; ++j) {
            cout << C_view[i][j] << " ";
        }
        cout << endl;
    }

    return 0;
}
