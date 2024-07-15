# Create Binary Tree From Descriptions
[Problem Link](https://leetcode.com/problems/create-binary-tree-from-descriptions/description/)

## Problem Description

You are given a 2D integer array descriptions where descriptions[i] = [parenti, childi, isLefti] indicates that parenti is the parent of childi in a binary tree of unique values. Furthermore,

- If `isLefti` == 1, then `childi` is the left child of `parenti`.
- If `isLefti` == 0, then `childi` is the right child of `parenti`.

Construct the binary tree described by descriptions and return its root.
The test cases will be generated such that the binary tree is valid.

## Approach

1. Initialization:
    - Create an unordered map `mp` to store the nodes of the tree, where the key is the node value, and the value is the corresponding `*TreeNode*`.
    - Create an unordered set `child` to keep track of all child nodes which helps in to find also root node.
2. Process Descriptions:
    - Iterate through each description in the `descriptions` array:
        - For each description [parenti, childi, isLefti]:
            - Add `childi` to the child set.
            - Check if `parenti` is already in the `mp` map. If not, create a new TreeNode for `parenti` and add it to the map.
            - Check if `childi` is already in the `mp` map. If not, create a new TreeNode for `childi` and add it to the map.
            - Based on the value of `isLefti`, set the left or right child of the `parenti` node to the `childi` node in the `mp` map.
3. Determine the Root:
    - Iterate through the descriptions again to find the `root` node:
    - The root node is a node that is never a `child` node. If `parenti` is not found in the child set, it is the root node.
4. Return the Root:
    - Return the `root` node.
    

### Complexity

- Time complexity: O(n)
- Space complexity: O(n)

## Code

```cpp
class Solution {
public:
    TreeNode* createBinaryTree(vector<vector<int>>& descriptions) {
        unordered_map<int, TreeNode*> mp;
        unordered_set<int> child;
        for(auto& it : descriptions){
            child.insert(it[1]);
            if(mp.find(it[0]) == mp.end())
                mp[it[0]] = new TreeNode(it[0]);
            if(mp.find(it[1]) == mp.end())
                mp[it[1]] = new TreeNode(it[1]);
            if(it[2] == 1){
                mp[it[0]]->left = mp[it[1]];
            }else{
                mp[it[0]]->right = mp[it[1]];
            }
        }
        for(auto& it:descriptions){
            if(child.find(it[0]) == child.end())
                return mp[it[0]];
        }
        return nullptr;
    }
};