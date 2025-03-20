#include <iostream>
#include <vector>
#include <string>
#include <functional>
#include <algorithm>
#include <cmath>
#include <random>

// Хеш-функция
size_t hash_url(const std::string& url, size_t N) {
    std::hash<std::string> hasher;
    return hasher(url) % N;
}

// Функция вычисления ранга
int compute_rank(size_t y) {
    int rank = 0;
    while (y % 2 == 0 && y != 0) {
        y = y >> 1;
        rank++;
    }
    return rank;
}

// Генерация случайного URL
std::string generate_random_url(size_t m) {
    std::string domain = "http://example.com/";
    std::string chars = "abcdefghijklmnopqrstuvwxyz0123456789";
    std::random_device rd;
    std::mt19937 gen(rd());
    std::uniform_int_distribution<> dist(0, chars.size() - 1);

    std::string random_part;
    for (int i = 0; i < 10; ++i) {
        random_part += chars[dist(gen)];
    }
    return domain + random_part;
}

int main() {
    // Параметры эксперимента
    std::vector<size_t> m_values = {10, 20, 30, 40, 50, 100, 500, 1000};
    std::vector<size_t> s_factors = {2, 5, 10, 100};
    size_t N = 1000; // Размер хеш-пространства

    // Результаты экспериментов
    std::vector<std::vector<double>> E_results(m_values.size(), std::vector<double>(s_factors.size(), 0.0));
    std::vector<std::vector<double>> D_results(m_values.size(), std::vector<double>(s_factors.size(), 0.0));

    // Проведение экспериментов
    for (size_t mi = 0; mi < m_values.size(); ++mi) {
        size_t m = m_values[mi];
        for (size_t si = 0; si < s_factors.size(); ++si) {
            size_t s = m * s_factors[si];
            double total_E = 0.0;
            double total_D = 0.0;

            for (int exp = 0; exp < 1000; ++exp) {
                std::vector<int> R(N, 0);

                for (size_t i = 0; i < s; ++i) {
                    std::string url = generate_random_url(m);
                    size_t y = hash_url(url, N);
                    int rank = compute_rank(y);
                    if (rank > R[y]) {
                        R[y] = rank;
                    }
                }

                double R_avg = 0.0;
                for (int r : R) {
                    R_avg += r;
                }
                R_avg /= N;

                double m_e = std::pow(2, R_avg) / 0.77351; // Оценка количества уникальных элементов
                double E = m - m_e;
                double D = E / m;

                total_E += E;
                total_D += D;
            }

            E_results[mi][si] = total_E / 1000;
            D_results[mi][si] = total_D / 1000;
        }
    }

    // Вывод результатов
    std::cout << "Absolute Errors (E):\n";
    for (size_t mi = 0; mi < m_values.size(); ++mi) {
        for (size_t si = 0; si < s_factors.size(); ++si) {
            std::cout << "m: " << m_values[mi] << ", s: " << s_factors[si] << "m, E: " << E_results[mi][si] << "\n";
        }
    }

    std::cout << "\nRelative Errors (D):\n";
    for (size_t mi = 0; mi < m_values.size(); ++mi) {
        for (size_t si = 0; si < s_factors.size(); ++si) {
            std::cout << "m: " << m_values[mi] << ", s: " << s_factors[si] << "m, D: " << D_results[mi][si] << "\n";
        }
    }

    return 0;
}