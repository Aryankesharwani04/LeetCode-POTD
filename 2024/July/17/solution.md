# Delete Nodes And Return Forest
[Problem Link](https://leetcode.com/problems/delete-nodes-and-return-forest/description/)

## Problem Description

Given the root of a binary tree, each node in the tree has a distinct value.

After deleting all nodes with a value in `to_delete`, we are left with a forest (a disjoint union of trees).

Return the roots of the trees in the remaining forest. You may return the result in any order.

## Approach

1. Initialization:
    - Create an unordered set `del` to store the nodes to be deleted for quick lookup.
    - Iterate through the `to_delete` list and insert each value into the `del` set.
    - Create a vector `res` to store the roots of the trees in the remaining forest.
2. Check Root:
    - If the root node's value is not in the `del` set, add the root to the `res` vector, as it will be part of the remaining forest.
3. Generate Forests:
    - Define a helper function `generateForests` to recursively traverse the tree and delete nodes as specified.
    - If the current node is null, return.
    - Store the `left` and `right` children of the current node in temporary variables.
    - Check if the `left` child needs to be deleted:
        - If yes, set the `left` child of the `current` node to null and mark `leftFlag` as false.
    - Check if the `right` child needs to be deleted:
        - If yes, set the `right` child of the current node to null and mark `rightFlag` as false.
    - Check if the `current` node itself needs to be deleted:
        - If yes, `delete` the `current` node.
        - If the `left` child is not null and was not marked for deletion, add it to the `res` vector.
        - If the `right` child is not null and was not marked for deletion, add it to the `res` vector.
    - Recursively call generateForests for the `left` and `right` children as stored above correctly.
4. Return Result:
    - Return the `res` vector containing the roots of the trees in the remaining forest.

### Complexity

- Time Complexity: O(n)
- Space Complexity: O(h)

## Code

```cpp
class Solution {
public:
    vector<TreeNode*> delNodes(TreeNode* root, vector<int>& to_delete) {
        unordered_set<int> del;
        for(int& i:to_delete) del.insert(i);
        vector<TreeNode*> res;
        if(del.find(root->val) == del.end())    
            res.push_back(root);
        generateForests(root, del, res);
        return res;
    }
private:
    void generateForests(TreeNode* root, unordered_set<int>& del, vector<TreeNode*>& res){
        if(!root) return;
        TreeNode* left = root->left;
        TreeNode* right = root->right;
        bool leftFlag = true, rightFlag = true;
        if(left && del.find(left->val) != del.end()){
            leftFlag = false;
            root->left = nullptr;
        }
        if(right && del.find(right->val) != del.end()){
            rightFlag = false;
            root->right = nullptr;
        }
        if(del.find(root->val) != del.end()){
            delete root;
            if(left && leftFlag) res.push_back(left);
            if(right && rightFlag) res.push_back(right);
        }
        generateForests(left, del, res);
        generateForests(right, del, res);
    }
};