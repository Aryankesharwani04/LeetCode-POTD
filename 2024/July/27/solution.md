## Minimum Cost to Convert String I
[Problem Link](https://leetcode.com/problems/minimum-cost-to-convert-string-i/)

## Problem Description

You are given two 0-indexed strings `source` and `target`, both of length `n` and consisting of lowercase English letters. You are also given two 0-indexed character arrays `original` and `changed`, and an integer array `cost`, where `cost[i]` represents the cost of changing the character `original[i]` to the character `changed[i]`.

You start with the string `source`. In one operation, you can pick a character `x` from the string and change it to the character `y` at a cost of `z` if there exists any index `j` such that `cost[j] == z`, `original[j] == x`, and `changed[j] == y`.

Return the minimum cost to convert the string `source` to the string `target` using any number of operations. If it is impossible to convert `source` to `target`, return -1.

Note that there may exist indices `i, j` such that `original[j] == original[i]` and `changed[j] == changed[i]`.

## Approach

1. **Graph Initialization**:
    - Create a graph `g` with 26 vertices (representing each letter 'a' to 'z') and initialize the weights to infinity (`inf`), except for the diagonal elements, which should be zero since the cost of changing a character to itself is zero.

2. **Build Graph with Given Costs**:
    - Update the graph with the given costs from `original` to `changed` characters.

3. **Floyd-Warshall Algorithm**:
    - Use the Floyd-Warshall algorithm to find the shortest path (minimum cost) between all pairs of vertices (characters).

4. **Calculate the Minimum Cost**:
    - For each character in `source` that needs to be changed to match `target`, accumulate the minimum cost. If a conversion is not possible (i.e., the cost remains infinity), return -1.

### Complexity

- **Time Complexity**: O(n + 26^3) ≈ O(26^3) since the Floyd-Warshall algorithm runs in O(26^3) and is independent of `n`.
- **Space Complexity**: O(26^2) for storing the graph.

## Code

```cpp
class Solution {
public:
    long long minimumCost(string source, string target, vector<char>& original, vector<char>& changed, vector<int>& cost) {
        const int inf = 1 << 29; // a large value to represent infinity
        int g[26][26]; // graph for storing the minimum costs

        // Initialize the graph with infinity
        for (int i = 0; i < 26; ++i) {
            fill(begin(g[i]), end(g[i]), inf);
            g[i][i] = 0; // cost to convert a character to itself is 0
        }

        // Populate the graph with the given costs
        for (int i = 0; i < original.size(); ++i) {
            int x = original[i] - 'a';
            int y = changed[i] - 'a';
            int z = cost[i];
            g[x][y] = min(g[x][y], z);
        }

        // Apply Floyd-Warshall algorithm to find the shortest paths
        for (int k = 0; k < 26; ++k) {
            for (int i = 0; i < 26; ++i) {
                for (int j = 0; j < 26; ++j) {
                    g[i][j] = min(g[i][j], g[i][k] + g[k][j]);
                }
            }
        }

        // Calculate the total minimum cost to convert source to target
        long long ans = 0;
        int n = source.length();
        for (int i = 0; i < n; ++i) {
            int x = source[i] - 'a';
            int y = target[i] - 'a';
            if (x != y) {
                if (g[x][y] >= inf) { // if conversion is not possible
                    return -1;
                }
                ans += g[x][y];
            }
        }
        return ans;
    }
};
```