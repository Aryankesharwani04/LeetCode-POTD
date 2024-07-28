## Find the City With the Smallest Number of Neighbors at a Threshold Distance
[Problem Link](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/description/)

## Problem Description

There are `n` cities numbered from `0` to `n-1`. Given the array `edges` where `edges[i] = [fromi, toi, weighti]` represents a bidirectional and weighted edge between cities `fromi` and `toi`, and given the integer `distanceThreshold`.

Return the city with the smallest number of cities that are reachable through some path and whose distance is at most `distanceThreshold`. If there are multiple such cities, return the city with the greatest number.

Notice that the distance of a path connecting cities `i` and `j` is equal to the sum of the edges' weights along that path.

## Approach

1. **Initialization**:
   - Create a 2D matrix `dist` where `dist[i][j]` represents the minimum distance between city `i` and city `j`.
   - Initialize `dist[i][i]` to `0` for all cities.
   - Initialize `dist[i][j]` to the weight of the edge between `i` and `j` if there is an edge, otherwise set it to infinity.

2. **Floyd-Warshall Algorithm**:
   - This algorithm is used to find the shortest paths between all pairs of cities.
   - For each pair of cities `(i, j)`, update the distance `dist[i][j]` to be the minimum of the current distance and the distance through an intermediate city `k`.

3. **Find the City**:
   - Iterate through each city to count the number of cities reachable within the distance threshold.
   - Track the city with the smallest number of reachable cities. If there is a tie, choose the city with the greater index.

### Complexity

- **Time Complexity**: O(n^3)
  - The Floyd-Warshall algorithm requires three nested loops over `n` cities.
- **Space Complexity**: O(n^2)
  - The `dist` matrix stores distances between each pair of cities.

## Code

```cpp
class Solution {
public:
    int findTheCity(int n, vector<vector<int>>& edges, int distanceThreshold) {
        vector<vector<int>> dist(n, vector<int>(n, numeric_limits<int>::max()));
        
        // Initialize distances based on edges
        for (int i = 0; i < n; ++i) {
            dist[i][i] = 0;
        }
        for (const auto& edge : edges) {
            int u = edge[0], v = edge[1], w = edge[2];
            dist[u][v] = w;
            dist[v][u] = w;
        }
        
        // Floyd-Warshall algorithm
        for (int k = 0; k < n; ++k) {
            for (int i = 0; i < n; ++i) {
                for (int j = 0; j < n; ++j) {
                    if (dist[i][k] != numeric_limits<int>::max() && dist[k][j] != numeric_limits<int>::max()) {
                        dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
                    }
                }
            }
        }

        int minReachableCities = numeric_limits<int>::max();
        int bestCity = -1;
        
        for (int i = 0; i < n; ++i) {
            int reachableCities = 0;
            for (int j = 0; j < n; ++j) {
                if (dist[i][j] <= distanceThreshold) {
                    reachableCities++;
                }
            }
            
            if (reachableCities < minReachableCities || (reachableCities == minReachableCities && i > bestCity)) {
                minReachableCities = reachableCities;
                bestCity = i;
            }
        }
        
        return bestCity;
    }
};
```