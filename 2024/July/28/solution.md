## Second Minimum Time to Reach Destination
[Problem Link](https://leetcode.com/problems/second-minimum-time-to-reach-destination/)

## Problem Description

A city is represented as a bi-directional connected graph with `n` vertices where each vertex is labeled from 1 to `n` (inclusive). The edges in the graph are represented as a 2D integer array `edges`, where each `edges[i] = [ui, vi]` denotes a bi-directional edge between vertex `ui` and vertex `vi`. Every vertex pair is connected by at most one edge, and no vertex has an edge to itself. The time taken to traverse any edge is `time` minutes.

Each vertex has a traffic signal which changes its color from green to red and vice versa every `change` minutes. All signals change at the same time. You can enter a vertex at any time but can leave a vertex only when the signal is green. You cannot wait at a vertex if the signal is green.

The second minimum value is defined as the smallest value strictly larger than the minimum value.

For example, the second minimum value of `[2, 3, 4]` is `3`, and the second minimum value of `[2, 2, 4]` is `4`.

Given `n`, `edges`, `time`, and `change`, return the second minimum time it will take to go from vertex `1` to vertex `n`.

## Approach

1. **Graph Representation**:
    - Use an adjacency list to represent the graph.

2. **Priority Queue**:
    - Use a priority queue to perform a modified Dijkstra’s algorithm.

3. **Tracking Visits**:
    - Use two vectors to track the shortest and second shortest times to reach each vertex.

4. **BFS with Priority Queue**:
    - Implement BFS using the priority queue to explore paths.

5. **Traffic Signal Handling**:
    - Adjust the waiting time based on the traffic signal's state at each vertex.

### Complexity

- **Time Complexity**: O(n log n)
- **Space Complexity**: O(n)

## Code

```cpp
class Solution {
public:
    int secondMinimum(int n, vector<vector<int>>& edges, int time, int change) {
        // Graph representation using adjacency list
        unordered_map<int, list<int>> graph;
        for (const auto& edge : edges) {
            graph[edge[0]].push_back(edge[1]);
            graph[edge[1]].push_back(edge[0]);
        }

        // Priority queue for Dijkstra-like BFS: {current_time, current_node}
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
        pq.push({0, 1}); // Start from node 1 at time 0

        // Two arrays to track the shortest and second shortest times to reach each node
        vector<int> shortestTime(n + 1, INT_MAX);
        vector<int> secondShortestTime(n + 1, INT_MAX);

        // Process the queue
        while (!pq.empty()) {
            auto [currentTime, node] = pq.top();
            pq.pop();

            // Determine if we are waiting for the green light
            if ((currentTime / change) % 2 == 1) {
                currentTime = (currentTime / change + 1) * change;
            }

            // Process neighbors
            for (int neighbor : graph[node]) {
                int newTime = currentTime + time;

                // Update shortest or second shortest time
                if (newTime < shortestTime[neighbor]) {
                    secondShortestTime[neighbor] = shortestTime[neighbor];
                    shortestTime[neighbor] = newTime;
                    pq.push({newTime, neighbor});
                } else if (newTime > shortestTime[neighbor] && newTime < secondShortestTime[neighbor]) {
                    secondShortestTime[neighbor] = newTime;
                    pq.push({newTime, neighbor});
                }
            }
        }

        // Return the second shortest time to reach node n
        return secondShortestTime[n];
    }
};
```
