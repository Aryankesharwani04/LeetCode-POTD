# Sort the People
[Problem Link](https://leetcode.com/problems/sort-the-people/description/)

## Problem Description

You are given an array of strings names, and an array heights that consists of distinct positive integers. Both arrays are of length n.

For each index i, names[i] and heights[i] denote the name and height of the ith person.

Return names sorted in descending order by the people's heights.

## Approach

1. Initialization:
    - Create a map sorted to `store` heights as keys and corresponding names as values in descending order of heights.
2. Populate the Map:
    - Iterate over the names and heights arrays, and insert each name-height pair into the map sorted.
3. Extract Sorted Names:
    - Create a vector `res` to store the names sorted by heights in descending order.
    - Iterate over the map sorted and append each name to `res`. 
    
### Complexity

- Time Complexity: O(nlogn)
- Space Complexity: O(n) 

## Code

```cpp
class Solution {
public:
    vector<string> sortPeople(vector<string>& names, vector<int>& heights) {
        map<int, string, greater<int>> sorted;
        for(int i = 0; i < names.size(); ++i) sorted[heights[i]] = names[i];
        vector<string> res;
        for(auto& [_, n] : sorted) res.push_back(n);
        return res;
    }
};