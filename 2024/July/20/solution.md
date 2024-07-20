# Find Valid Matrix Given Row and Column Sums
[Problem Link](https://leetcode.com/problems/find-valid-matrix-given-row-and-column-sums/description/)

## Problem Description

You are given two arrays `rowSum` and `colSum` of non-negative integers where rowSum[i] is the sum of the elements in the ith row and colSum[j] is the sum of the elements of the jth column of a 2D matrix. In other words, you do not know the elements of the matrix, but you do know the sums of each row and column.

Find any matrix of non-negative integers of size `rowSum.length x colSum.length` that satisfies the `rowSum` and `colSum` requirements.

Return a 2D array representing any matrix that fulfills the requirements. It's guaranteed that at least one matrix that fulfills the requirements exists.

## Approach

1. Initialization:
    - Determine the dimensions of the matrix `m` (number of rows) and `n` (number of columns) from the lengths of `rowSum` and `colSum`.
    - Create a 2D vector `ans` of size m x n initialized with `zeros` to store the result.
    - Initialize two pointers `i` and `j` to `0` to traverse through the rows and columns.
2. Construct the Matrix:
    - While both `i` and `j` are within their respective bounds (i < m and j < n):
    - Calculate the minimum value between `rowSum[i]` and `colSum[j]`.
    - Assign this minimum value to `ans[i][j]`.
    - Subtract this value from both `rowSum[i]` and `colSum[j]`.
    - If `rowSum[i]`becomes zero, increment `i` to move to the next row.
    - If `colSum[j]` becomes zero, increment `j` to move to the next column.
3. Return the Result:
    - Return the 2D vector ans which now contains a valid matrix that satisfies the given row and column sums.

### Complexity

- Time Complexity: O(m + n)
- Space Complexity: O(1)

## Code

```cpp
class Solution {
public:
    vector<vector<int>> restoreMatrix(vector<int>& rowSum, vector<int>& colSum) {
        int m = rowSum.size();
        int n = colSum.size();
        vector<vector<int>> ans(m, vector<int>(n, 0));

        int i = 0, j = 0;

        while(i < m && j < n){
            int value = min(rowSum[i], colSum[j]);
            ans[i][j] = value;
            rowSum[i] -= value;
            colSum[j] -= value;
            
            if(rowSum[i] == 0)  i++;
            if(colSum[j] == 0)  j++;
        }

        return ans;
    }
};