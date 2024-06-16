# Question 1: Find the Missing Number

- You are given an array containing n-1 numbers from 1 to n. Find the missing number

### js
```js
function findMissingNumber(arr, n) {
    const totalSum = (n * (n + 1)) / 2;
    const arraySum = arr.reduce((acc, num) => acc + num, 0);
    return totalSum - arraySum;
}

// Example usage:
const arr = [1, 2, 4, 5, 6];
const n = 6;
console.log(findMissingNumber(arr, n)); // Output: 3

```
### C++
```C++
#include <iostream>
#include <vector>
using namespace std;

int findMissingNumber(vector<int>& arr, int n) {
    int totalSum = (n * (n + 1)) / 2;
    int arraySum = 0;
    for (int num : arr) {
        arraySum += num;
    }
    return totalSum - arraySum;
}

// Example usage:
int main() {
    vector<int> arr = {1, 2, 4, 5, 6};
    int n = 6;
    cout << findMissingNumber(arr, n) << endl; // Output: 3
    return 0;
}
```

# Question 2: Find Duplicates in an Array

### JS
```JS
function findDuplicates(arr) {
    const duplicates = [];
    const seen = new Set();
    
    for (let num of arr) {
        if (seen.has(num)) {
            duplicates.push(num);
        } else {
            seen.add(num);
        }
    }
    
    return duplicates;
}

// Example usage:
const arr = [1, 2, 3, 4, 2, 3, 5, 6];
console.log(findDuplicates(arr)); // Output: [2, 3]

```
### C++
```c++
#include <iostream>
#include <vector>
#include <unordered_set>
using namespace std;

vector<int> findDuplicates(vector<int>& arr) {
    vector<int> duplicates;
    unordered_set<int> seen;
    
    for (int num : arr) {
        if (seen.find(num) != seen.end()) {
            duplicates.push_back(num);
        } else {
            seen.insert(num);
        }
    }
    
    return duplicates;
}

// Example usage:
int main() {
    vector<int> arr = {1, 2, 3, 4, 2, 3, 5, 6};
    vector<int> duplicates = findDuplicates(arr);
    for (int num : duplicates) {
        cout << num << " ";
    }
    cout << endl; // Output: 2 3
    return 0;
}

```

# Question 3: Maximum Subarray Sum (Kadane’s Algorithm)

### js
```js
function maxSubArraySum(arr) {
    let maxSoFar = arr[0];
    let maxEndingHere = arr[0];
    
    for (let i = 1; i < arr.length; i++) {
        maxEndingHere = Math.max(arr[i], maxEndingHere + arr[i]);
        maxSoFar = Math.max(maxSoFar, maxEndingHere);
    }
    
    return maxSoFar;
}

// Example usage:
const arr = [-2, 1, -3, 4, -1, 2, 1, -5, 4];
console.log(maxSubArraySum(arr)); // Output: 6 (subarray is [4, -1, 2, 1])

```

### C++

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int maxSubArraySum(vector<int>& arr) {
    int maxSoFar = arr[0];
    int maxEndingHere = arr[0];
    
    for (int i = 1; i < arr.size(); i++) {
        maxEndingHere = max(arr[i], maxEndingHere + arr[i]);
        maxSoFar = max(maxSoFar, maxEndingHere);
    }
    
    return maxSoFar;
}

// Example usage:
int main() {
    vector<int> arr = {-2, 1, -3, 4, -1, 2, 1, -5, 4};
    cout << maxSubArraySum(arr) << endl; // Output: 6 (subarray is [4, -1, 2, 1])
    return 0;
}


```
#  Question 4: Rotate Array

```js
function rotateArray(arr, k) {
    k = k % arr.length;
    reverse(arr, 0, arr.length - 1);
    reverse(arr, 0, k - 1);
    reverse(arr, k, arr.length - 1);
    
    return arr;
}

function reverse(arr, start, end) {
    while (start < end) {
        [arr[start], arr[end]] = [arr[end], arr[start]];
        start++;
        end--;
    }
}

// Example usage:
const arr = [1, 2, 3, 4, 5, 6, 7];
const k = 3;
console.log(rotateArray(arr, k)); // Output: [5, 6, 7, 1, 2, 3, 4]

```
```C++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

void reverse(vector<int>& arr, int start, int end) {
    while (start < end) {
        swap(arr[start], arr[end]);
        start++;
        end--;
    }
}

void rotateArray(vector<int>& arr, int k) {
    k = k % arr.size();
    reverse(arr, 0, arr.size() - 1);
    reverse(arr, 0, k - 1);
    reverse(arr, k, arr.size() - 1);
}

// Example usage:
int main() {
    vector<int> arr = {1, 2, 3, 4, 5, 6, 7};
    int k = 3;
    rotateArray(arr, k);
    for (int num : arr) {
        cout << num << " ";
    }
    cout << endl; // Output: [5, 6, 7, 1, 2, 3, 4]
    return 0;
}

```
# Question 5: Find the Intersection of Two Arrays

```js
function intersection(arr1, arr2) {
    const set1 = new Set(arr1);
    const set2 = new Set(arr2);
    const intersectionSet = new Set([...set1].filter(x => set2.has(x)));
    return Array.from(intersectionSet);
}

// Example usage:
const arr1 = [1, 2, 2, 1];
const arr2 = [2, 2];
console.log(intersection(arr1, arr2)); // Output: [2]

```

```C++
#include <iostream>
#include <vector>
#include <unordered_set>
#include <algorithm>
using namespace std;

vector<int> intersection(vector<int>& arr1, vector<int>& arr2) {
    unordered_set<int> set1(arr1.begin(), arr1.end());
    unordered_set<int> set2(arr2.begin(), arr2.end());
    vector<int> result;
    
    for (int num : set1) {
        if (set2.find(num) != set2.end()) {
            result.push_back(num);
        }
    }
    
    return result;
}

// Example usage:
int main() {
    vector<int> arr1 = {1, 2, 2, 1};
    vector<int> arr2 = {2, 2};
    vector<int> result = intersection(arr1, arr2);
    for (int num : result) {
        cout << num << " ";
    }
    cout << endl; // Output: [2]
    return 0;
}

```
