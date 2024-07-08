# Find the Winner of the Circular Game
[Problem Link](https://leetcode.com/problems/find-the-winner-of-the-circular-game/description/)

## Problem Description

There are n friends that are playing a game. The friends are sitting in a circle and are numbered from 1 to n in clockwise order. More formally, moving clockwise from the ith friend brings you to the (i+1)th friend for 1 <= i < n, and moving clockwise from the nth friend brings you to the 1st friend.

The rules of the game are as follows:

1. Start at the 1st friend.
2. Count the next k friends in the clockwise direction including the friend you started at. The counting wraps around the circle and may count some friends more than once.
3. The last friend you counted leaves the circle and loses the game.
4. If there is still more than one friend in the circle, go back to step 2 starting from the friend immediately clockwise of the friend who just lost and repeat.
5. Else, the last friend in the circle wins the game.

Given the number of friends, n, and an integer k, return the winner of the game.

## Approach

1. Initialize a vector `circle` with integers from 1 to n representing initiall indices.
    - Set the initial index `cur` to 0.
    - Begin a while loop that continues until the length of `circle` is 1 (we've found a winner):
        - Calculate the index `next` of the next person to be removed.
        - Remove the element at `next` from `circle`.
        - Update `cur` to `next`.
2. Return the last remaining element in `circle` (winner original index).

### Complexity

- Time complexity: O(n^2)
- Space complexity: O(n)

## Code

```cpp
class Solution {
public:
    int findTheWinner(int n, int k) {
        vector<int> circle;
        for(int i = 1; i <= n; ++i) circle.push_back(i);
        int cur = 0;
        while (circle.size() > 1) {
            int next = (cur+ k - 1) % circle.size();
            circle.erase(circle.begin() + next);
            cur = next;
        }

        return circle[0];
    }
};