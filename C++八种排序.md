常见的八种排序
=============

## 需要的头文件

```c++
#include <iostream>
#include <vector>
#include <queue>
#include <string>
#include <sstream>
```

## 前置函数

其实这些函数在 *std* 命名空间中就已经定义好了，这里算是重复造轮子了。

```c++
// Act as helper
int max(int num1, int num2) {
    if (num1 > num2) {
        return num1;
    }
    return num2;
}

// Act as helper
int min(int num1, int num2) {
    if (num1 < num2) {
        return num1;
    }
    return num2;
}
```

你当然也可以写成：

```c++
int max(int n1, int n2) {return n1 < n2 ? n2 : n1;}

int min(int n1, int n2) {return n1 < n2 ? n1 : n2;}
```

## 插入排序

+ #### 时间复杂度： $O(n^2)$

+ #### 空间复杂度：$O(1)$

```c++
// O(n^2) time complexity
// If not act as a helper in merge sort and quick sort, we don't need parameter "start" and "end"
bool insertionSort(std::vector<int>& nums, int start, int end) {
    for (int i = start; i < end; i++) {
        int j = i;
        while (j > 0 && nums[j] < nums[j - 1]) {
            std::swap(nums[j], nums[j - 1]);
            j--;
        }
    }
    return true;
}
```

## 冒泡排序

+ #### 时间复杂度： $O(n^2)$

+ #### 空间复杂度：$O(1)$

```c++
// O(n^2) time complexity
// If not act as a helper in merge sort, we don't need parameter "start" and "end"
bool bubbleSort(std::vector<int>& nums, int start, int end) {
    for (int i = start; i < end; i++) {
        for (int j = i + 1; j < end; j++) {
            if (nums[i] > nums[j]) {
                std::swap(nums[i], nums[j]);
            }
        }
    }
    return true;
}
```

## 选择排序

+ #### 时间复杂度： $O(n^2)$

+ #### 空间复杂度：$O(1)$

```c++
// O(n^2) time complexity
bool selectionSort(std::vector<int>& nums) {
    for (int i = 0; i < nums.size(); i++) {
        int min_p = i;
        for (int j = nums.size(); j > i; j--) {
            if (nums[j] < nums[min_p]) {
                min_p = j;
            }
        }
        std::swap(nums[min_p], nums[i]);
    }
    return true;
}
```

## 归并排序

+ #### 时间复杂度： $O(n log n)$

+ #### 空间复杂度：$O(n)$

```c++
// Act as helper in merge sort
bool merge(std::vector<int>& nums, int start, int mid, int end) {
    std::vector<int> left(nums.begin() + start, nums.begin() + mid);
    std::vector<int> right(nums.begin() + mid, nums.begin() + end);
    int pLeft = 0;
    int pRight = 0;
    int p = start;
    while (pLeft < left.size() && pRight < right.size()) {
        if (left[pLeft] < right[pRight]) {
            nums[p] = left[pLeft];
            pLeft++;
        } else {
            nums[p] = right[pRight];
            pRight++;
        }
        p++;
    }
    if (pLeft == left.size() && pRight == right.size()) {
        return true;
    } else if (pLeft == left.size()) {
        while (pRight < right.size()) {
            nums[p] = right[pRight];
            p++;
            pRight++;
        }
    } else {
        while (pLeft < left.size()) {
            nums[p] = left[pLeft];
            p++;
            pLeft++;
        }
    }
    return true;
}

// O(n log n) time complexity
bool mergeSort(std::vector<int>& nums, int start, int end) {
    if (end - start <= 3) {
        insertionSort(nums, start, end);
    } else {
        int mid = start + (end - start) / 2;
        mergeSort(nums, start, mid);
        mergeSort(nums, mid, end);
        merge(nums, start, mid, end);
    }
    return true;
}
```

## 堆排序

+ #### 时间复杂度： $O(nlogn)$

+ #### 空间复杂度：$O(n)$

```c++
// O(n log n) time complexity
bool heapSort(std::vector<int>& nums) {
    std::priority_queue<int> q;
    for (int i: nums) {
        q.push(i);
    }
    for (int i = nums.size() - 1; i >= 0; i--) {
        nums[i] = q.top();
        q.pop();
    }
    return true;
}
```

## 快排序

+ #### 时间复杂度： $O(nlogn)$

+ #### 空间复杂度：$O(1)$

```c++
// O(n log n) time complexity
bool quickSort(std::vector<int>& nums, int start, int end) {
    if (end - start <= 6) {
        insertionSort(nums, start, end);
    } else {
        int mid = start + (end - start) / 2;
        int maximum = max(max(nums[mid], nums[start]), max(nums[mid], nums[end - 1]));
        int minimum = min(min(nums[mid], nums[start]), min(nums[mid], nums[end - 1]));
        int pivot = nums[mid] + nums[start] + nums[end - 1] - maximum - minimum;
        nums[mid] = maximum;
        nums[start] = minimum;
        int left = start + 1;
        int right = end - 2;
        while (left < right) {
            if (nums[left] > pivot && nums[right] < pivot) {
                std::swap(nums[left], nums[right]);
            }
            while (left < right && nums[left] <= pivot) {
                left++;
            }
            while (right > left && nums[right] >= pivot) {
                right--;
            }
        }
        nums[end - 1] = nums[left];
        nums[left] = pivot;
        quickSort(nums, start, left);
        quickSort(nums, left, end);
    }
    return true;
}

```
其中的 *insertionSort*函数就是我们前面提到的插入排序。

## 桶排序

+ #### 时间复杂度： $O(n)$

+ #### 空间复杂度：$O(n)$

需要注意的是，这里的 $n$ 指的是这组数字中最大的数字。桶排序仅适用于排序自然数，且当自然数比较分散的时候排序效率并不高。

例子： `[1, 2, 3, 4, 100]`

```c++
// O(n) time complexity, n is the maximum in array
bool bucketSort(std::vector<int>& nums, int minimum, int maximum) {
    if (minimum < 0) {
        return false;
    }
    std::vector<int> bucket(maximum + 1);
    for (int i: nums) {
        bucket[i]++;
    }
    int p = 0;
    for (int i = 0; i < bucket.size(); i++) {
        for (int j = 0; j < bucket[i]; j++) {
            nums[p] = i;
            p++;
        }
    }
    return true;
}
```

## 基数排序

+ #### 时间复杂度： $O(n \cdot m)$

+ #### 空间复杂度：$O(n)$

这里的 $m$ 指的是排序数字的最大长度。这种排序方法在排序大量多位整数的时候比较高效。

### 必要的前置函数

```c++
// Act as helper in radix sort
long long stringToLongLong(std::string n) {
    std::stringstream ss;
    ss << n;
    long long num = 0;
    ss >> num;
    return num;
}

// Act as helper in radix sort
int charToInt(char n) {
    std::stringstream ss;
    ss << n;
    int num = 0;
    ss >> num;
    return num;
}

// Act as helper in radix sort
std::string longLongToString(long long n) {
    std::stringstream ss;
    ss << n;
    std::string num = ss.str();
    return num;
}
```

### 函数的主体部分

```c++
// O(n*m) time complexity, n is the number of elements, m is the number of bits of these elements
bool radixSort(std::vector<long long>& nums) {
    if (nums.size() == 0) {
        return false;
    }
    std::vector<std::string> nums_str;
    for (long long i: nums) {
        nums_str.push_back(longLongToString(i));
    }
    int m = nums_str[0].size();
    for (int i = m - 1; i >= 0; i--) {
        std::vector<std::vector<std::string>> bucket(10);
        for (int j = 0; j < nums_str.size(); j++) {
            bucket[charToInt(nums_str[j][i])].push_back(nums_str[j]);
        }
        int p = 0;
        for (int j = 0; j < bucket.size(); j++) {
            for (int k = 0; k < bucket[j].size(); k++) {
                nums_str[p] = bucket[j][k];
                p++;
            }
        }
    }
    for (int i = 0; i < nums_str.size(); i++) {
        nums[i] = stringToLongLong(nums_str[i]);
    }
    return true;
}
```

## 对测试以上排序函数有帮助的函数

```c++
void show(std::vector<int> nums) {
    std::cout << "Size of Array: " << nums.size() << std::endl;
    for (int i: nums) {
        std::cout << i << " ";
    }
    std::cout << std::endl;
}

void showButLongLong(std::vector<long long> nums) {
    std::cout << "Size of Array: " << nums.size() << std::endl;
    for (long long i: nums) {
        std::cout << i << " ";
    }
    std::cout << std::endl;
}

std::vector<int> randomlyBuildArray(int size, int lower, int upper) {
    std::vector<int> nums;
    for (int i = 0; i < size; i++) {
        nums.push_back(rand() % (upper - lower) + lower);
    }
    return nums;
}

std::vector<long long> randomlyBuildArrayButLongLong(int size, long long lower, long long upper) {
    std::vector<long long> nums;
    for (int i = 0; i < size; i++) {
        nums.push_back(rand() % (upper - lower) + lower);
    }
    return nums;
}
```

## 测试排序函数

```c++
int main() {
    // test bubble sort
    std::vector<int> nums1 = randomlyBuildArray(20, 0, 101);
    std::cout << "Before bubble sort: " << std::endl;
    show(nums1);
    std::cout << "After bubble sort: " << std::endl;
    bubbleSort(nums1, 0, nums1.size());
    show(nums1);
    std::cout << std::endl;
    
    // test selection sort
    std::vector<int> nums2 = randomlyBuildArray(20, -100, 101);
    std::cout << "Before selection sort: " << std::endl;
    show(nums2);
    std::cout << "After selection sort: " << std::endl;
    selectionSort(nums2);
    show(nums2);
    std::cout << std::endl;
    
    // test insertion sort
    std::vector<int> nums3 = randomlyBuildArray(20, -50, 251);
    std::cout << "Before insertion sort: " << std::endl;
    show(nums3);
    std::cout << "After insertion sort: " << std::endl;
    insertionSort(nums3, 0, nums3.size());
    show(nums3);
    std::cout << std::endl;
    
    // test merge sort
    std::vector<int> nums4 = randomlyBuildArray(30, -100, 101);
    std::cout << "Before merge sort: " << std::endl;
    show(nums4);
    std::cout << "After merge sort: " << std::endl;
    mergeSort(nums4, 0, nums4.size());
    show(nums4);
    std::cout << std::endl;
    
    // test heap sort
    std::vector<int> nums5 = randomlyBuildArray(30, -100, 101);
    std::cout << "Before heap sort: " << std::endl;
    show(nums5);
    std::cout << "After heap sort: " << std::endl;
    heapSort(nums5);
    show(nums5);
    std::cout << std::endl;
    
    // test quick sort
    std::vector<int> nums8 = randomlyBuildArray(30, 0, 101);
    std::cout << "Before quick sort: " << std::endl;
    show(nums8);
    std::cout << "After quick sort: " << std::endl;
    quickSort(nums8, 0, nums8.size());
    show(nums8);
    std::cout << std::endl;
    
    // test bucket sort
    std::vector<int> nums6 = randomlyBuildArray(30, 0, 101);
    std::cout << "Before bucket sort: " << std::endl;
    show(nums6);
    std::cout << "After bucket sort: " << std::endl;
    bucketSort(nums6, 0, 100);
    show(nums6);
    std::cout << std::endl;
    
    // test radix sort
    std::vector<long long> nums7 = randomlyBuildArrayButLongLong(30, 1000000, 10000000);
    std::cout << "Before radix sort: " << std::endl;
    showButLongLong(nums7);
    std::cout << "After radix sort: " << std::endl;
    radixSort(nums7);
    showButLongLong(nums7);
    std::cout << std::endl;
    
    return 0;
}
```

## 八种排序的总结

序号 | 排序方法 | 时间复杂度 | 空间复杂度
:---- | :--------: | :----------: | :----------:
1 | 插入排序 | $O(n^2)$ | $O(1)$
2 | 冒泡排序 | $O(n^2)$ | $O(1)$
3 | 选择排序 | $O(n^2)$ | $O(1)$
4 | 归并排序 | $O(n \cdot log n)$ | $O(n)$
5 | 堆排序 | $O(n \cdot log n)$ | $O(n)$
6 | 快排序 | $O(n \cdot log n)$ | $O(1)$
7 | 桶排序 | $O(n)$ | $O(n)$
8 | 基数排序 | $O(n)$ | $O(n)$