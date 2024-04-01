#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

void minCashFlowRecursion(vector<int>& amount) {
    int maxCredit = 0, maxDebit = 0;
    for (int i = 1; i < amount.size(); ++i) {
        if (amount[i] > amount[maxCredit])
            maxCredit = i;
        if (amount[i] < amount[maxDebit])
            maxDebit = i;
    }

    if (amount[maxCredit] == 0 && amount[maxDebit] == 0)
        return;

    int minimum = min(-amount[maxDebit], amount[maxCredit]);
    amount[maxCredit] -= minimum;
    amount[maxDebit] += minimum;
    cout << "Person " << maxDebit + 1 << " pays " << minimum << " to Person " << maxCredit + 1 << "." << endl;

    minCashFlowRecursion(amount);
}

void minCashFlow(vector<vector<int>>& graph) {
    int n = graph.size();
    vector<int> amount(n, 0);

    for (int p = 0; p < n; ++p) {
        for (int i = 0; i < n; ++i) {
            amount[p] += (graph[i][p] - graph[p][i]);
        }
    }

    minCashFlowRecursion(amount);
}

int main() {
    int n;
    cout << "Enter the number of persons: ";
    cin >> n;

    vector<vector<int>> graph(n, vector<int>(n, 0));
    cout << "Enter the amounts owed between each pair of persons:" << endl;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            if (i != j) {
                cout << "Amount owed from Person " << i + 1 << " to Person " << j + 1 << ": ";
                cin >> graph[i][j];
            }
        }
    }

    minCashFlow(graph);

    return 0;
}
