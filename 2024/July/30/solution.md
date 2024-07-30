## Minimum Deletions to Make String Balanced
[Problem Link](https://leetcode.com/problems/minimum-deletions-to-make-string-balanced/description/)

## Problem Description

You are given a string `s` consisting only of characters 'a' and 'b'.

You can delete any number of characters in `s` to make `s` balanced. `s` is balanced if there is no pair of indices `(i, j)` such that `i < j` and `s[i] = 'b'` and `s[j] = 'a'`.

Return the minimum number of deletions needed to make `s` balanced.

## Approach

1. Iterate through the string, keeping track of the number of 'b's encountered.
2. For each 'a' encountered, calculate the minimum deletions needed by either deleting the current 'a' or deleting the previous 'b's.

### Complexity

- **Time Complexity**: O(n)
- **Space Complexity**: O(1)

## Code

```cpp
class Solution {
public:
    int minimumDeletions(string s) {
        int countB = 0, minDeletions = 0;
        for (char& c : s) {
            if (c == 'b') {
                countB++;
            } else {
                minDeletions = min(minDeletions + 1, countB);
            }
        }
        return minDeletions;
    }
};
```