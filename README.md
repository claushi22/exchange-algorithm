# exchange-algorithm
#include <iostream>
#include <vector>
#include <chrono>

using namespace std;
using namespace chrono;

struct SortStats {
    long long comparisons = 0;
    long long swaps = 0;
};

SortStats exchangeSortDescending(vector<int>& arr) {
    SortStats stats;
    int n = arr.size();
    
    for (int i = 0; i < n - 1; i++) {
        for (int j = i + 1; j < n; j++) {
            stats.comparisons++;
            if (arr[i] < arr[j]) {  // Descending: swap if left < right
                stats.swaps++;
                int temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
    }
    return stats;
}

void printArray(const vector<int>& arr) {
    for (int val : arr) cout << val << " ";
    cout << endl;
}

int main() {
    vector<int> sizes = {1, 2, 3, 4, 5, 10, 250, 999, 9999, 89786, 789300, 1780000};
    
    for (int size : sizes) {
        cout << "\n--- Size: " << size << " ---\n";
        vector<int> arr(size);
        
        // Fill with random numbers for average-case test
        for (int i = 0; i < size; i++) {
            arr[i] = rand() % 1000000;
        }
        
        auto start = high_resolution_clock::now();
        SortStats stats = exchangeSortDescending(arr);
        auto end = high_resolution_clock::now();
        
        auto duration = duration_cast<milliseconds>(end - start).count();
        
        cout << "Sorted (first 10): ";
        for (int i = 0; i < min(10, size); i++) cout << arr[i] << " ";
        cout << endl;
        cout << "Comparisons: " << stats.comparisons << endl;
        cout << "Swaps: " << stats.swaps << endl;
        cout << "Time: " << duration << " ms" << endl;
    }
    
    // Example with fixed small list
    cout << "\n=== Example Walkthrough ===" << endl;
    vector<int> example = {3, 1, 4, 1, 5, 9, 2};
    cout << "Original: ";
    printArray(example);
    SortStats exStats = exchangeSortDescending(example);
    cout << "Sorted (descending): ";
    printArray(example);
    cout << "Comparisons: " << exStats.comparisons << ", Swaps: " << exStats.swaps << endl;
    
    return 0;
}
