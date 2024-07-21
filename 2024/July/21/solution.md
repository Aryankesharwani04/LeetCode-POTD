# Build a Matrix With Conditions
[Problem Link](https://leetcode.com/problems/build-a-matrix-with-conditions/)

## Problem Description

You are given a positive integer `k`. You are also given:
- a 2D integer array `rowConditions` of size `n` where rowConditions[i] = [abovei, belowi], and
- a 2D integer array `colConditions` of size `m` where colConditions[i] = [lefti, righti].

The two arrays contain integers from `1 to k`.

You have to build a `k x k` matrix that contains each of the numbers from 1 to k exactly once. The remaining cells should have the value `0`.

The matrix should also satisfy the following conditions:
- The number `abovei` should appear in a row that is strictly above the row at which the number `belowi` appears for all i from 0 to n - 1.
- The number `lefti` should appear in a column that is strictly left of the column at which the number `righti` appears for all i from 0 to m - 1.

Return any matrix that satisfies the conditions. If no answer exists, return an empty matrix.

## Approach

1. Topological Sort:
    - Use Kahn's algorithm to perform a topological sort on the nodes (numbers from 1 to k) for both `rowConditions` and `colConditions`.
    - This will help us determine the order in which numbers should appear in the rows and columns.
2. Build the Graph and Calculate Indegrees:
    - Create a graph and calculate the `indegree` of each node for both row and column conditions.
    - Use BFS to process nodes with no dependencies `(indegree 0)`.
3. Check for Cycles:
    - If any node has a non-zero `indegree` after processing, it means there's a cycle, and we return an empty matrix.
4. Build the Result Matrix:
    - Use the orders obtained from the topological sorts to place the numbers in the correct positions in the matrix.
    
### Complexity

- Time Complexity: O(k^2)
    - Building the graph and performing topological sorts for row and column conditions take `O(k + n)` and `O(k + m)` respectively. Constructing the matrix takes `O(k^2)`.
- Space Complexity: O(k^2)
    - Storing the graph and indegree arrays takes `O(k)`. The result matrix takes `O(k^2)`.

## Code

```cpp
class Solution {
public:
//topological sort

    //use BFS Kahn's algorithm
    vector<int> topoSort(int k, vector<vector<int>> edges){
        vector<vector<int>> graph(k+1);
        vector<int> indegree(k+1,0);
        vector<int> res;


        //build graph & calculate indegrees
        for(vector<int>& edge: edges){
            graph[edge[0]].push_back(edge[1]);
            indegree[edge[1]]++;
        }

        queue<int> q;
        //push nodes with no dependency
        for(int i=1; i<=k; i++){
            if(indegree[i]==0){
                q.push(i);
            }
        }
        //BFS
        while(!q.empty()){
            int cur = q.front(); q.pop();
            res.push_back(cur);

            for(int neigh: graph[cur]){
                indegree[neigh]--;
                if(indegree[neigh]==0){
                    q.push(neigh);
                }
            }
        }
        //check for cyle
        for(int i=1; i<=k; i++){
            if(indegree[i]!=0){
                return {};
            }
        }
        return res;
    }
    vector<vector<int>> buildMatrix(int k, vector<vector<int>>& rowConditions, vector<vector<int>>& colConditions) {

        vector<int> rowOrder = topoSort(k, rowConditions);
        vector<int> colOrder = topoSort(k, colConditions);

        //if we encountered a cycle in any condition, return empty array
        if(rowOrder.empty() || colOrder.empty()){
            return {};
        }

        vector<vector<int>> res(k, vector<int>(k,0));

        //build the result matrix
        for(int i=0; i<k; i++){
            int ele = rowOrder[i];
            for(int j=0; j<k; j++){
                if(colOrder[j]==ele){
                    res[i][j] = ele;
                }
            }
        }
        return res;
    }
};