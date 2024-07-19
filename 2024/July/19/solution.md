# Lucky Numbers in a Matrix
[Problem Link](https://leetcode.com/problems/lucky-numbers-in-a-matrix/description/)

## Problem Description

Given an m x n matrix of distinct numbers, return all lucky numbers in the matrix in any order.

A lucky number is an element of the matrix such that it is the `minimum` element in its row and `maximum` in its column.

## Approach

1. Initialization:
    - Initialize an empty vector `ans` to store the lucky numbers.
2. Find Minimum Elements in Rows:
    - For each row in the matrix, find the minimum element and its index.
    - Initialize `minm` to INT_MAX and `index` to 0.
    - Traverse through each element in the row to update `minm` and `index` with the minimum element and its `index`, respectively.
3. Check if the Minimum Element is the Maximum in its Column:
    - For the identified minimum element in each row, check if it is the maximum element in its column.
    - Use a flag `flag` to determine if it is the maximum.
    - Traverse through each element in the column
        - If any element is greater than the minimum element, set the `flag` to 0 and `break` the loop.
        - else continue traversing.
4. Add to Result:
    - If the `flag` is still 1 after checking the column, add the minimum element to `ans`.
5. Return Result:
    - Return the vector `ans` containing all lucky numbers.

### Complexity

- Time Complexity: O(m * n)
    - Where `m` is the number of rows and `n` is the number of columns. 
    - We traverse each element in the matrix once to find the minimum in each row and again to check if it is the maximum in its column.
- Space Complexity: O(1)
    - Excluding the space used for the output vector `ans`.

## Code

```cpp
class Solution {
public:
    vector<int> luckyNumbers (vector<vector<int>>& matrix) {
        vector<int> ans;
        for(int i = 0; i<matrix.size(); i++){
            int minm = INT_MAX, index = 0, flag = 1;
            for(int j = 0; j<matrix[i].size(); j++){
                if(matrix[i][j] < minm){
                    minm = matrix[i][j];
                    index = j;
                }
            }
            for(int k = 0; k < matrix.size(); k++){
                if(minm < matrix[k][index]){
                    flag = 0;
                    break;
                }
            }
            if(flag)
                ans.push_back(minm);
        }
        return ans;
    }
};