# Crawler Log Folder
[Problem Link](https://leetcode.com/problems/crawler-log-folder/description/)

## Problem Description

The Leetcode file system keeps a log each time some user performs a change folder operation.

The operations are described below:
- `../` : Move to the parent folder of the current folder. (If you are already in the main folder, remain in the same folder).
- `./` : Remain in the same folder.
- `x/` : Move to the child folder named x (This folder is guaranteed to always exist).
You are given a list of strings logs where `logs[i]` is the operation performed by the user at the ith step.

The file system starts in the main folder, then the operations in logs are performed.

Return the minimum number of operations needed to go back to the main folder after the change folder operations.

## Approach

1. Initialize Counter:
    - Start by initializing a counter `count` to `0`. This counter will keep track of the current depth of the folder structure.
2. Iterate Through Logs:
    - Loop through each operation in the logs list:
        - If the operation is `"./"` (remain in the same folder), do nothing and continue to the next operation.
        - If the operation is `"../"` (move to the parent folder):
            - Check if the `count` is greater than `0`. If so, decrement `count` by `1` (move up one level).
            - If `count` is already `0`, it means you are in the main folder, so no further action is needed.
        - If the operation is of the form `"x/"` (move to a child folder):
            - Increment `count` by `1` (move down one level).
3. Return Result:
    - After processing all operations, the value of `count` will be the minimum number of operations needed to return to the main folder from the current folder.

### Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Code

```cpp
class Solution {
public:
    int minOperations(vector<string>& logs) {
        int count = 0;
        for (auto& c : logs) {
            if (c == "./")
                continue;
            else {
                if (c == "../") {
                    if (count)
                        count--;
                } else
                    count++;
            }
        }
        return count;
    }
};