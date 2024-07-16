# Step-By-Step Directions From a Binary Tree Node to Another
[Problem Link](https://leetcode.com/problems/step-by-step-directions-from-a-binary-tree-node-to-another/description/)

## Problem Description

You are given the root of a binary tree with n nodes. Each node is uniquely assigned a value from 1 to n. You are also given an integer startValue representing the value of the start node s, and a different integer destValue representing the value of the destination node t.

Find the shortest path starting from node s and ending at node t. Generate step-by-step directions of such path as a string consisting of only the uppercase letters 'L', 'R', and 'U'. Each letter indicates a specific direction:
- 'L' means to go from a node to its left child node.
- 'R' means to go from a node to its right child node.
- 'U' means to go from a node to its parent node.
Return the step-by-step directions of the `shortest path` from node `s` to node `t`.

## Approach

1. Initialization:
    - Initialize two strings, `result1` and `result2`, to store the paths from the lowest common ancestor (LCA) to `startValue` and `destValue`, respectively.
2. Find the Lowest Common Ancestor (LCA):
    - Use the lowestCommonAncestor function to find the LCA of the nodes with values `startValue` and `destValue`. The LCA is the node from which both `startValue` and `destValue` are reachable in the shortest path.
3. Find Paths from `LCA`:
    - Use the `findPath` function to find the path from the LCA to `startValue` and store it in `result1`.
    - Use the `findPath` function again to find the path from the LCA to `destValue` and store it in `result2`.
4. Convert Path to `'U'`:
    - Convert all characters in `result1` to `'U'`, indicating moving up from the `startValue` to the LCA.
5. Concatenate and Return Result:
    - Concatenate `result1` and `result2` to form the complete path from `startValue` to `destValue`.
6. Return the concatenated result.

### Complexity

- Time Complexity: O(n)
    - Finding the LCA takes O(n) time in the worst case, where n is the number of nodes in the tree.
    - Finding the path from the LCA to `startValue` and `destValue` each takes O(n) time in the worst case.
    - Therefore, the overall time complexity is O(n).
- Space Complexity: O(h)
    - The space complexity is determined by the recursion stack used in the `lowestCommonAncestor` and `findPath` functions, which is proportional to the height `h` of the tree.
    - In the worst case, the height of the tree can be O(n) for a skewed tree.
    - Therefore, the overall space complexity is O(h).

## Code

```cpp
class Solution {
public:
    string getDirections(TreeNode* root, int startValue, int destValue){
        string result1, result2;
        TreeNode* lca = lowestCommonAncestor(root, startValue, destValue);
        findPath(lca, startValue, result1);
        findPath(lca, destValue, result2);
        for(char& c:result1)
            c = 'U';
        return result1 + result2;
    }
private:
    TreeNode* lowestCommonAncestor(TreeNode* root, int start, int dest) {
        if(!root || root->val == start || root->val == dest)
            return root;
        TreeNode* left = lowestCommonAncestor(root->left, start, dest);
        TreeNode* right = lowestCommonAncestor(root->right, start, dest);
        if(left && right)   return root;
        return left ? left : right;
    }
    bool findPath(TreeNode* root, int target, string& path) {
        if(!root) return false;
        if(root->val == target) return true;
        path.push_back('L');
        if(findPath(root->left, target, path))  return true;
        path.pop_back(); 
        path.push_back('R');
        if(findPath(root->right, target, path)) return true;
        path.pop_back(); return false;
    }

};
