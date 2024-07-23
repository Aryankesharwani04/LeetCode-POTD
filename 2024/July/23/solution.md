# Sort Array by Increasing Frequency
[Problem Link](https://leetcode.com/problems/sort-array-by-increasing-frequency/description/)

## Problem Description

Given an array of integers `nums`, sort the array in increasing order based on the frequency of the values. If multiple values have the same frequency, sort them in `decreasing` order.

Return the sorted array.

## Approach

1. Initialization:
    - Create an unordered map `mp` to store the frequency of each element in `nums`.
    - Create a vector `mv` to store pairs of frequency and corresponding element values.

2. Populate the Map:
    - Iterate over `nums` and populate the frequency map `mp`.

3. Transform Map to Vector:
    - Iterate over the frequency map `mp` and populate the vector `mv` with pairs of frequency and corresponding element values.

4. Sort the Vector:
    - Sort the vector `mv` based on two criteria:
        - Primary: Increasing order of frequency.
        - Secondary: Decreasing order of element value for elements with the same frequency.

5. Construct the Result:
    - Create an empty vector `ans` to store the result.
    - Iterate over the sorted vector `mv` and append each element `freq` times to `ans`.
    
### Complexity

- Time Complexity: O(nlogn)
- Space Complexity: O(n) 

## Code

```cpp
class Solution {
public:
    vector<int> frequencySort(vector<int>& nums) {
        unordered_map<int, int> mp;
        for(const int& i : nums) mp[i]++;

        vector<pair<int, int>> mv;
        for(const auto& [first, second] : mp) mv.emplace_back(second, first);

        sort(mv.begin(), mv.end(), [](const auto& a, const auto& b){
            if(a.first == b.first)
                return a.second > b.second;
            return a.first < b.first;
        });

        vector<int> ans;
        for(const auto& [freq, value] : mv)
            ans.insert(ans.end(), freq, value);
        
        return ans;
    }
};