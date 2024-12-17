#include <amp.h>
#include <iostream>
#include <vector>

using namespace concurrency;

const int N = 1024; // Размер матриц

void matrixMultiply(const std::vector<std::vector<int>>& A, const std::vector<std::vector<int>>& B, std::vector<std::vector<int>>& C) {
    // Создаем массивы для передачи на GPU
    array<int, 2> d_A(N, N, &A[0][0]);
    array<int, 2> d_B(N, N, &B[0][0]);
    array<int, 2> d_C(N, N); // Результирующий массив

    // Используем захват, чтобы ссылаться на массивы
    parallel_for_each(d_C.extent, [d_A, d_B, d_C](index<2> idx) restrict(amp) {
        int sum = 0;
        for (int k = 0; k < N; ++k) {
            sum += d_A[idx[0]][k] * d_B[k][idx[1]];
        }
        d_C[idx] = sum;
    });

    // Копируем результат обратно из GPU в CPU
    copy(d_C, C);
}

int main() {
    // Инициализация матриц
    std::vector<std::vector<int>> A(N, std::vector<int>(N, 1)); // Матрица A (1s)
    std::vector<std::vector<int>> B(N, std::vector<int>(N, 2)); // Матрица B (2s)
    std::vector<std::vector<int>> C(N, std::vector<int>(N, 0)); // Результирующая матрица C

    // Перемножение матриц
    matrixMultiply(A, B, C);

    // Печать некоторых элементов для проверки (например, элемент 0,0)
    std::cout << "C[0][0] = " << C[0][0] << std::endl; // Ожидается 2048

    return 0;
}
