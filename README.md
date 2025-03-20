#include <iostream>
#include <vector>
#include <string>
#include <random>
#include <cmath>
#include <functional>
#include <iomanip>

using namespace std;

vector<string> generate_urls(int m) {
    vector<string> urls;
    for (int i = 0; i < m; ++i) {
        urls.push_back("url_" + to_string(i));
    }
    return urls;
}

uint32_t h(const string& url) {
    hash<string> hasher;
    return static_cast<uint32_t>(hasher(url));
}

int compute_rank(uint32_t y) {
    if (y == 0) return 32;
    int rank = 0;
    while ((y & 1) == 0) {
        rank++;
        y >>= 1;
    }
    return rank;
}

vector<int> precompute_ranks(const vector<string>& urls) {
    vector<int> ranks;
    ranks.reserve(urls.size());
    for (const auto& url : urls) {
        uint32_t hash_val = h(url);
        ranks.push_back(compute_rank(hash_val));
    }
    return ranks;
}

double run_experiment(int m, int s, const vector<int>& ranks) {
    random_device rd;
    mt19937 gen(rd());
    uniform_int_distribution<> dist(0, m-1);
    int max_rank = 0;

    for (int i = 0; i < s; ++i) {
        int rank = ranks[dist(gen)];
        max_rank = max(max_rank, rank);
    }

    return pow(2, max_rank) / 0.77351;
}

void run_simulation(int m, int s, double& avg_E, double& avg_D) {
    auto urls = generate_urls(m);
    auto ranks = precompute_ranks(urls);
    double total_E = 0.0, total_D = 0.0;

    for (int i = 0; i < 1000; ++i) {
        double m_e = run_experiment(m, s, ranks);
        double E = m - m_e;
        total_E += E;
        total_D += E / m;
    }

    avg_E = total_E / 1000;
    avg_D = total_D / 1000;
}

int main() {
    vector<int> m_values = {10, 20, 30, 40, 50, 100, 500, 1000};
    vector<int> s_coeffs = {2, 5, 10, 100};

    cout << fixed << setprecision(2);
    for (int m : m_values) {
        for (int coeff : s_coeffs) {
            int s = coeff * m;
            double avg_E, avg_D;
            run_simulation(m, s, avg_E, avg_D);
            cout << "m = " << m << ", s = " << s 
                 << ", E = " << avg_E << ", D = " << avg_D << endl;
        }
    }
    return 0;
}