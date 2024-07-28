## Sort an Array
[Problem Link](https://leetcode.com/problems/sort-an-array/description/)

## Problem Description

Given an array of integers `nums`, sort the array in ascending order and return it.

You must solve the problem without using any built-in functions in O(n log n) time complexity and with the smallest space complexity possible.

## Approach

1. **Merge Sort**:
   - We will implement the merge sort algorithm to achieve the required time complexity of O(n log n) and a space complexity of O(n).

2. **Merge Function**:
   - This function merges two sorted subarrays into a single sorted subarray.
   - It takes the original array, along with the indices `low`, `mid`, and `high` that define the two subarrays: `array[low..mid]` and `array[mid+1..high]`.
   - It creates temporary arrays for the left and right parts, then merges them back into the original array in sorted order.

3. **Merge Sort Function**:
   - This function recursively divides the array into two halves until each subarray contains a single element.
   - It then merges the subarrays using the merge function.
   - The base case is when the subarray has less than two elements (`low >= high`).

### Complexity

- **Time Complexity**: O(n log n)
  - Merge sort divides the array into halves log(n) times, and each division involves merging, which takes linear time.
- **Space Complexity**: O(n)
  - Temporary arrays are used for merging, requiring additional space proportional to the size of the array.

## Code

```cpp
void merge(vector<int>& array, int low, int mid, int high) {
    int n1 = mid - low + 1;
    int n2 = high - mid;
    vector<int> left_part(n1), right_part(n2);

    for (int i = 0; i < n1; ++i)
        left_part[i] = array[low + i];
    for (int i = 0; i < n2; ++i)
        right_part[i] = array[mid + 1 + i];

    int p1 = 0, p2 = 0, write_ind = low;
    while (p1 < n1 && p2 < n2) {
        if (left_part[p1] <= right_part[p2]) {
            array[write_ind] = left_part[p1++];
        } else {
            array[write_ind] = right_part[p2++];
        }
        ++write_ind;
    }

    while (p1 < n1)
        array[write_ind++] = left_part[p1++];

    while (p2 < n2)
        array[write_ind++] = right_part[p2++];
}

void merge_sort(vector<int>& array, int low, int high) {
    if (low >= high)
        return;

    int mid = low + (high - low) / 2;
    merge_sort(array, low, mid);
    merge_sort(array, mid + 1, high);
    merge(array, low, mid, high);
}

class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        merge_sort(nums, 0, nums.size() - 1);
        return nums;
    }
};
```