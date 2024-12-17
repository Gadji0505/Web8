#include <amp.h>
#include <iostream>
#include <vector>

using namespace concurrency;

// Функция для перемножения двух квадратных матриц с использованием AMP
void multiplyMatrices(const std::vector<float>& A, const std::vector<float>& B, std::vector<float>& C, int N) {
    // Создаем массивы для использования в AMP
    array<float, 2> a(N, N, A.data());
    array<float, 2> b(N, N, B.data());
    array<float, 2> c(N, N);

    parallel_for_each(c.extent, [=](index<2> idx) restrict(amp) {
        float sum = 0.0f;
        for (int k = 0; k < N; k++) {
            sum += a[idx[0]][k] * b[k][idx[1]];
        }
        c[idx] = sum;
    });

    // Копируем результат обратно в вектор C
    copy(c, C.data());
}

int main() {
    const int N = 512; // Размер матриц

    // Инициализация матриц A и B, их размерами N x N
    std::vector<float> A(N * N);
    std::vector<float> B(N * N);
    std::vector<float> C(N * N); // Результирующая матрица

    // Заполнение матриц A и B случайными значениями
    for (int i = 0; i < N; ++i) {
        for (int j = 0; j < N; ++j) {
            A[i * N + j] = static_cast<float>(rand()) / RAND_MAX;
            B[i * N + j] = static_cast<float>(rand()) / RAND_MAX;
        }
    }

    // Перемножение матриц
    multiplyMatrices(A, B, C, N);

    // Вывод результата (лишь часть для проверки)
    for (int i = 0; i < 5; ++i) {
        for (int j = 0; j < 5; ++j) {
            std::cout << C[i * N + j] << " ";
        }
        std::cout << std::endl;
    }

    return 0;
}
