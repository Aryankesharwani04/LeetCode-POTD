# Pass the Pillow
[Problem Link](https://leetcode.com/problems/pass-the-pillow/description/)

## Problem Description

There are n people standing in a line labeled from 1 to n. The first person in the line is holding a pillow initially. Every second, the person holding the pillow passes it to the next person standing in the line. Once the pillow reaches the end of the line, the direction changes, and people continue passing the pillow in the opposite direction.

For example, once the pillow reaches the nth person they pass it to the n - 1th person, then to the n - 2th person and so on.
Given the two positive integers n and time, return the index of the person holding the pillow after time seconds.

## Approach

1. Understanding the Movement:
    - The pillow starts with the first person and moves down the line.
    - Once it reaches the last person, the direction reverses, and the pillow moves back up the line.
    - This cycle repeats every (n-1) seconds (going down takes n-1 seconds and going up takes another n-1 seconds).
2. Determine the Cycle:
    - Calculate how many complete cycles of (n-1) seconds have passed by using time / (n-1).
    - Use the remainder time % (n-1) to find out how far along the pillow is in the current cycle.
3. Determine the Direction:
    - If the number of complete cycles (time / (n-1)) is even, the pillow is moving down the line.
    - If the number of complete cycles (time / (n-1)) is odd, the pillow is moving up the line.
4. Calculate the Position:
    - If the pillow is moving down, the position is 1 + time % (n-1).
    - If the pillow is moving up, the position is n - time % (n-1).

### Complexity

- Time complexity: O(1)
- Space complexity: O(1)

## Code

```cpp
class Solution {
public:
    int passThePillow(int n, int time) {
        return  ((time/(n-1))%2 != 0) ? n - time%(n-1) : 1 + time%(n-1);
    }
};