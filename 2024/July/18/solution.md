# Number of Good Leaf Nodes Pairs
[Problem Link](https://leetcode.com/problems/number-of-good-leaf-nodes-pairs/description/)

## Problem Description

You are given the `root` of a binary tree and an integer `distance`. A pair of two different leaf nodes of a binary tree is said to be good if the length of the shortest path between them is less than or equal to distance.

Return the number of good leaf node pairs in the tree.

## Approach

1. Initialization:
    - Create an unordered map `graph` to represent the tree as an undirected graph.
    - Create an unordered set `leafNodes` to store all leaf nodes.
2. Traverse Tree:
    - Use a helper function `traverseTree` to traverse the tree and populate the `graph` and `leafNodes`.
    - For each node, if it is a leaf node (no left or right child), add it to `leafNodes`.
    - Add bidirectional `edges` between the current node and its parent in `graph`.
3. Breadth-First Search (BFS):
    - Initialize `ans` to count the number of good leaf node pairs.
    - For each leaf node, perform a BFS up to distance steps to find other leaf nodes within the given distance.
    - Use a queue `bfsQueue` for BFS and an unordered set `seen` to track visited nodes.
    - For each node at the current BFS level, if it is a leaf node and not the starting node, increment `ans`.
    - Push unvisited neighbors to the `bfsQueue` and mark them as `seen`.
4. Count Pairs:
    - Since each pair is counted twice, return `ans / 2`.

### Complexity

- Time Complexity: O(n + m), where `n` is the number of nodes and `m` is the number of edges in the tree. Each node and edge is processed a constant number of times.
- Space Complexity: O(n), for storing the graph, leaf nodes, and BFS queue.

## Code

```cpp
class Solution {
public:
    int countPairs(TreeNode* root, int distance) {
        unordered_map<TreeNode*, vector<TreeNode*>> graph;
        unordered_set<TreeNode*> leafNodes;

        traverseTree(root, nullptr, graph, leafNodes);

        int ans = 0;
        for(TreeNode* leaf : leafNodes){
            queue<TreeNode*> bfsQueue;
            unordered_set<TreeNode*> seen;
            bfsQueue.push(leaf);
            seen.insert(leaf);

            for(int i = 0; i <= distance; i++){
                int size = bfsQueue.size();
                for(int j = 0; j < size; j++){
                    TreeNode* currNode = bfsQueue.front();
                    bfsQueue.pop();
                    if(leafNodes.count(currNode) && currNode != leaf){
                        ans++;
                    }
                    if(graph.count(currNode)){
                        for(TreeNode* neighbor : graph[currNode]){
                            if(!seen.count(neighbor)){
                                bfsQueue.push(neighbor);
                                seen.insert(neighbor);
                            }
                        }
                    }
                }
            }
        }
        return ans / 2;
    }
private:
    void traverseTree(TreeNode* currNode, TreeNode* prevNode, unordered_map<TreeNode*, vector<TreeNode*>>& graph, unordered_set<TreeNode*>& leafNodes) {
        if(!currNode) return;
        if(!currNode->left && !currNode->right){
            leafNodes.insert(currNode);
        }
        if(prevNode){
            graph[prevNode].push_back(currNode);
            graph[currNode].push_back(prevNode);
        }
        traverseTree(currNode->left, currNode, graph, leafNodes);
        traverseTree(currNode->right, currNode, graph, leafNodes);
    }
};