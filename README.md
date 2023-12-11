#include <iostream>
#include <fstream>
#include <vector>
#include <chrono>

using namespace std;

void printArray(const vector<int>& arr) {
    for (int num : arr) {
        cout << num << " ";
    }
    cout << endl;
}

bool checkSort(vector<int>& arr, int N) {
    bool bRet = true;
    for (int i = 0; i < N - 1; ++i) {
        if (arr[i] > arr[i + 1]) {
            bRet = false;
            break;
        }
    }
    return bRet;
}

void bubble(vector<int>& arr, int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}

int cmp(const void* pi1, const void* pi2) {
    return (*(int*)pi1 - *(int*)pi2);
}

int bin_insert(vector<int>& arr, int left, int right, int i)
{
    if (left > right)
        return left;

    int mid = (left + right) / 2;

    if (arr[i] > arr[mid]) {
        bin_insert(arr, mid + 1, right, i);
    }

    else {
        bin_insert(arr, left, mid - 1, i);
    }
}

void shift_right(vector<int>& arr, int i, int index)
{
    int var_to_swap = arr[i];
    for (int k = i; k > index; k--) {
        arr[k] = arr[k - 1];
    }
    arr[index] = var_to_swap;
}

int main()
{
    vector<int> arr;
    vector<int> b;
    int q[1000] = {};
    int f;
    int i = 0;
    int index = 0, size = 1000;
    ifstream input("d5000.txt");

    if (input.is_open()) {
        while (input >> f) {
            arr.push_back(f);
            b.push_back(f);
            q[i] = f;
            i++;
        }
        input.close();
    }
    else {
        cout << "Unable to open the input file." << endl;
    }

    cout << "Array before sorting: \n";
    //printArray(arr);

    auto start0 = chrono::steady_clock::now();
    for (int i = 1; i < size; i++) {
        if (arr[i] < arr[i - 1]) {
            shift_right(arr, i, bin_insert(arr, 0, i - 1, i));
        }
    }
    auto end0 = chrono::steady_clock::now();
    long duration = chrono::duration_cast<chrono::microseconds>(end0 - start0).count();
    cout << endl << "Binary sorting time: " << duration << endl;

    auto start1 = chrono::steady_clock::now();
    bubble(arr, size);
    auto end1 = chrono::steady_clock::now();
    long duration1 = chrono::duration_cast<chrono::microseconds>(end1 - start1).count();
    cout << endl << "\nBubble sorting time:" << " " << duration1 << endl;

    auto start2 = chrono::steady_clock::now();
    qsort(q, size, sizeof(int), cmp);
    auto end2 = chrono::steady_clock::now();
    long duration2 = chrono::duration_cast<chrono::microseconds>(end2 - start2).count();
    cout << endl << "\nQuick sorting time:" << " " << duration2 << endl;

    if (checkSort(arr, size)) {
        cout << "Sorting was completed successfully.\n";
    }
    else {
        cout << "Sorting was performed with errors.\n";
    }
    cout << "Array after sorting: \n";
    //printArray(arr);

    ofstream output("output5000.txt");
    if (output.is_open()) {
        for (int num : arr) {
            output << num << " ";
        }
        output.close();
        cout << "Sorted array has been written to output.txt." << endl;
    }
    else {
        cout << "Unable to write to the file." << endl;
    }

    return 0;
}

