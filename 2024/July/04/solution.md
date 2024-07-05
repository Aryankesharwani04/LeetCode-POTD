#  Merge Nodes in Between Zeros
[Problem Link](https://leetcode.com/problems/merge-nodes-in-between-zeros/description/)

## Problem Description

You are given the head of a linked list, which contains a series of integers separated by 0's. The beginning and end of the linked list will have Node.val == 0.

For every two consecutive 0's, merge all the nodes lying in between them into a single node whose value is the sum of all the merged nodes. The modified list should not contain any 0's.

Return the head of the modified linked list.

## Approach

1. Skip the Initial Zero:
    - Start by moving to the next node to skip the initial zero, as it doesn't contribute to any sums.

2. Initialize New List and Sum:
    - Create a new linked list to store the results.
    - Use a pointer temp to keep track of the current node in the new list. 
    - Initialize a variable sum to accumulate the values between zeros.

3. Traverse the List and Calculate Sums:
    - Traverse through the original list starting from the first non-zero node.
    - Add each node's value to sum.
    - When a zero is encountered:
        - Create a new node with the accumulated sum and link it to the result list.
        - Move the temp pointer to this new node.
        - Reset sum to zero for the next group of values.

4. Return the Result List:
    - Return the new linked list starting from the first node after the dummy head.


### Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Code

```cpp
class Solution {
public:
    ListNode* mergeNodes(ListNode* head) {
        head = head->next;
        ListNode* newhead = new ListNode(0);
        ListNode* temp = newhead;
        int sum = 0;
        while(head){
            sum += head->val;
            if(head->val == 0){
                temp->next = new ListNode(sum);
                temp = temp->next;
                sum = 0;
            }
            head = head->next;
        }
        return newhead->next;
    }
};
