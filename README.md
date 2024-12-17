#include <iostream>
#include <vector>
#include <amp.h>

using namespace concurrency;

void multiplyMatrices(const std::vector<std::vector<int>>& A, 
                      const std::vector<std::vector<int>>& B, 
                      std::vector<std::vector<int>>& C, 
                      int N) 
{
    // Создаем массивы для передачи в GPU
    array<int, 2> A_array(N, N, A[0].data());
    array<int, 2> B_array(N, N, B[0].data());
    array<int, 2> C_array(N, N, C[0].data());

    parallel_for_each(C_array.extent, [&](index<2> idx) restrict(amp) {
        int sum = 0;
        for (int k = 0; k < N; ++k) {
            sum += A_array[idx[0]][k] * B_array[k][idx[1]];
        }
        C_array[idx] = sum;
    });
}

int main() {
    const int N = 512; // Размерность матрицы
    std::vector<std::vector<int>> A(N, std::vector<int>(N));
    std::vector<std::vector<int>> B(N, std::vector<int>(N));
    std::vector<std::vector<int>> C(N, std::vector<int>(N, 0));

    // Инициализация матриц A и B
    for (int i = 0; i < N; ++i) {
        for (int j = 0; j < N; ++j) {
            A[i][j] = rand() % 10; // Заполнение случайными числами
            B[i][j] = rand() % 10; // Заполнение случайными числами
        }
    }

    // Перемножение матриц
    multiplyMatrices(A, B, C, N);

    // Вывод результата (для проверки, комментарий для больших матриц)
    /*
    std::cout << "Результат перемножения матриц:" << std::endl;
    for (int i = 0; i < N; ++i) {
        for (int j = 0; j < N; ++j) {
            std::cout << C[i][j] << " ";
        }
        std::cout << std::endl;
    }
    */

    return 0;
}