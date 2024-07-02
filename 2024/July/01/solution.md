# Three Consecutive Odds
[Problem Link](https://leetcode.com/problems/three-consecutive-odds/)

## Problem Description

Given an integer array arr, return true if there are three consecutive odd numbers in the array. Otherwise, return false.

## Approach

The solution iterates through the array and checks if there are three consecutive odd numbers by verifying conditions at previous index, current index and its next index elements.

### Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Code

```cpp
class Solution {
public:
    bool threeConsecutiveOdds(vector<int>& arr) {
        for(int i = 1; i < arr.size() - 1; i++) {
            if(arr[i] % 2 && arr[i - 1] % 2 && arr[i + 1] % 2)
                return true;
        }
        return false;
    }
};
