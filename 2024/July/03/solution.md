# Minimum Difference Between Largest and Smallest Value in Three Moves
[Problem Link](https://leetcode.com/problems/minimum-difference-between-largest-and-smallest-value-in-three-moves/)

## Problem Description

You are given an integer array nums.
In one move, you can choose one element of nums and change it to any value.
Return the minimum difference between the largest and smallest value of nums after performing at most three moves.

## Approach

1. Sort the Array:
    - To easily find the largest and smallest numbers in the array, we first sort the array. After sorting, the smallest number will be at the beginning (index 0), and the largest will be at the end (last index).
2. Base Case:
    - If the array size is less than 5, we can make all elements equal with at most 3 moves. Hence, the minimum difference will be 0.
    - Example:
    `arr = [2, 3, 4, 4]
    Step 1: Change 2 -> 3, 4 -> 3, 4 -> 3
    Result: arr = [3, 3, 3, 3]
    Difference: 3 - 3 = 0`
3. Minimize the Difference:
    - When the array size is 5 or more, we need to consider making changes in 3 moves to minimize the difference between the largest and smallest numbers.
    - After sorting, the array will look like this: arr = [a1, a2, a3, ..., an]
    - We consider the following 4 scenarios:
        - Change the first three smallest numbers to the fourth smallest: `nums[n] - nums[3]`
        - Change the smallest two and the largest one to match the third and second largest respectively: `nums[n-1] - nums[2]`
        - Change the smallest one and the largest two to match the second and third largest respectively: `nums[n-2] - nums[1]`
        - Change the last three largest numbers to the fourth largest: `nums[n-3] - nums[0]`
    - We then return the minimum difference from these four scenarios.

### Complexity

- Time complexity: O(nlogn)
- Space complexity: O(1)

## Code

```cpp
class Solution {
public:
    int minDifference(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        if(nums.size() < 5)   return 0;
        int n = nums.size() - 1;
        return min(nums[n] - nums[3], min(nums[n - 1] - nums[2], min(nums[n - 2] - nums[1], nums[n - 3] - nums[0])));
    }
};
