# Robot Collisions
[Problem Link](https://leetcode.com/problems/robot-collisions/)

## Problem Description

There are `n` 1-indexed robots, each having a position on a line, health, and movement direction.

You are given 0-indexed integer arrays `positions`, `healths`, and a string `directions` (directions[i] is either `'L'` for left or `'R'` for right). All integers in `positions` are unique.

All robots start moving on the line simultaneously at the same speed in their given directions. If two robots ever share the same position while moving, they will collide.

If two robots collide, the robot with lower health is removed from the line, and the health of the other robot decreases by one. The surviving robot continues in the same direction it was going. If both robots have the same health, they are both removed from the line.

Your task is to determine the health of the robots that survive the collisions, in the same order that the robots were given, i.e. final heath of robot 1 (if survived), final health of robot 2 (if survived), and so on. If there are no survivors, return an empty array.

Return an array containing the health of the remaining robots (in the order they were given in the input), after no further collisions can occur.

`Note`: The `positions` may be unsorted.

## Approach

1. Initialize and Sort:
    - Create a vector `check` to store pairs of (position, index) for each robot.
    - Sort `check` based on `positions` in `descending order`, as we want to handle collisions from `right to left`.
2. Stack for Movement:
    - Initialize a stack `move` to keep track of the `indices` of robots moving to the left ('L').
3. Process Each Robot:
    - Iterate through the sorted `check` vector:
        - For each robot, if it is moving left ('L'), push its `index` onto the stack.
        - If it is moving right ('R'), check for potential collisions with robots moving `left` (from the stack).
        - Handle collisions:
            - While there are robots in the stack and the current robot's health is greater than `0`:
            - Compare the health of the current robot with the robot from the stack.
            - If the current robot's health is greater, `decrement its health` and `remove` the robot from the stack.
            - If the robot from the stack has greater health, `decrement its health` and set the current robot's health to `0`.
            - If both robots have the same health, `set both to 0`and `remove` the robot from the stack.
4. Collect Surviving Robots:
    - After processing all robots, collect the `healths` of the surviving robots `(those with health > 0)` in the order they were given in the input.

### Complexity

- Time complexity: O(nlogn)
- Space complexity: O(n)

## Code

```cpp
class Solution {
public:
    vector<int> survivedRobotsHealths(vector<int>& positions, vector<int>& healths, string directions) {
        vector<pair<int, int>> check;
        for(int i = 0; i<positions.size(); i++)
            check.emplace_back(positions[i], i);
        sort(check.begin(), check.end(), greater<>());
        stack<int> move;
        for(int it = 0; it < check.size(); it++){
            int i = check[it].second;
            if (directions[i] == 'L')   move.push(i);
            else{
                while(!move.empty() && healths[i] > 0){
                    int j = move.top();
                    int x = healths[j] - healths[i];
                    if (x > 0) healths[j]--, healths[i] = 0;
                    else if (x < 0) healths[j] = 0, healths[i]--, move.pop();
                    else healths[i] = 0, healths[j] = 0, move.pop();
                }
            }
        }
        vector<int> ans;
        for(int& i : healths)
            if(i > 0)
                ans.push_back(i);
        return ans;
    }
};