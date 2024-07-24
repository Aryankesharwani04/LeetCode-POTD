## Sort the Jumbled Numbers
[Problem Link](https://leetcode.com/problems/sort-the-jumbled-numbers/description/)

## Problem Description

You are given a 0-indexed integer array `mapping` which represents the mapping rule of a shuffled decimal system. `mapping[i] = j` means digit `i` should be mapped to digit `j` in this system.

The mapped value of an integer is the new integer obtained by replacing each occurrence of digit `i` in the integer with `mapping[i]` for all `0 <= i <= 9`.

You are also given another integer array `nums`. Return the array `nums` sorted in non-decreasing order based on the mapped values of its elements.

Notes:

- Elements with the same mapped values should appear in the same relative order as in the input.
- The elements of `nums` should only be sorted based on their mapped values and not be replaced by them.

## Approach

1. Initialization:
    - Create a vector `number` to store pairs of mapped values and their original indices.

2. Map Each Number:
    - For each number in `nums`, calculate its mapped value using the `mapping` array. 
    - Store each mapped value along with its original index in the `number` vector.

3. Sort Based on Mapped Values:
    - Sort the `number` vector based on the mapped values. This will ensure that elements with the same mapped values retain their relative order from the original `nums` array (stable sort).

4. Construct the Result:
    - Create an empty vector `ans` to store the result.
    - Append the original values from `nums` to `ans` based on the sorted order of their mapped values.

### Complexity

- **Time Complexity**: O(n log n)
- **Space Complexity**: O(n)

## Code

```cpp
class Solution {
public:
    vector<int> sortJumbled(vector<int>& mapping, vector<int>& nums) {
        vector<pair<int, int>> number;
        for(int i = 0; i<nums.size(); i++){
            int newNum = 0, place = 1, n = nums[i];
            if(n == 0) newNum = mapping[0];
            while(n){
                int d = n%10;
                newNum += place*mapping[d];
                place *= 10;
                n /= 10;
            }
            number.emplace_back(newNum, i);
        }

        sort(number.begin(), number.end());

        vector<int> ans;
        for(const auto& [_,index] : number)
            ans.push_back(nums[index]);

        return ans;
    }
};