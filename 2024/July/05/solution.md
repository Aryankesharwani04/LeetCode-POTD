#  Find the Minimum and Maximum Number of Nodes Between Critical Points
[Problem Link](https://leetcode.com/problems/find-the-minimum-and-maximum-number-of-nodes-between-critical-points/)

## Problem Description

A critical point in a linked list is defined as either a local maxima or a local minima.

A node is a local maxima if the current node has a value strictly greater than the previous node and the next node.

A node is a local minima if the current node has a value strictly smaller than the previous node and the next node.

Note that a node can only be a local maxima/minima if there exists both a previous node and a next node.

Given a linked list head, return an array of length 2 containing [minDistance, maxDistance] where minDistance is the minimum distance between any two distinct critical points and maxDistance is the maximum distance between any two distinct critical points. If there are fewer than two critical points, return [-1, -1].

## Approach

1. Initialize Variables:
    - Use an index i to track the current node position, starting from 2 (since we need at least two previous nodes to identify a critical point).
    - Initialize maxm to 0 for tracking the maximum distance between critical points.
    - Initialize minm to INT_MAX for tracking the minimum distance between critical points.
    - Use start to store the position of the first critical point.
    - Use previndex to store the position of the last found critical point.
    - Use prev to keep track of the previous node.
2. Traverse the List:
    - Start from the second node (since the first node cannot be a critical point).
    - For each node, check if it is a critical point (either a local maxima or a local minima):
        - Local Maxima: The current node's value is greater than both the previous node's value and the next node's value.
        - Local Minima: The current node's value is less than both the previous node's value and the next node's value.
3. Update Critical Points:
    - If a critical point is found:
        - If start is not set, initialize start with the current position i.
        - Otherwise, update minm with the minimum distance between the current and previous critical point.
        - Update maxm with the distance from the first critical point to the current critical point.
        - Update previndex with the current position i.
4. Return Result:
    - If no critical points were found (maxm remains 0), return [-1, -1].
    - Otherwise, return the calculated minm and maxm.

### Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Code

```cpp
class Solution {
public:
    vector<int> nodesBetweenCriticalPoints(ListNode* head) {
        int i = 2, maxm = 0, minm = INT_MAX, start = 0, previndex = 0;
        ListNode* prev = head;
        head = head->next;
        while(head->next){
            if((head->val > prev->val && head->val > head->next->val) || (head->val < prev->val && head->val < head->next->val)){
                if(!start) start = i;
                else{
                    minm = min(i - previndex, minm);
                    maxm = i - start;
                }
                previndex = i;
            }
            i++;
            prev = head;
            head = head->next;
        }
        if(maxm == 0)    return {-1,-1};
        return {minm, maxm};
    }
};