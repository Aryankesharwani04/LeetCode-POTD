## Count Number of Teams
[Problem Link](https://leetcode.com/problems/count-number-of-teams/description/)

## Problem Description

There are `n` soldiers standing in a line. Each soldier is assigned a unique rating value.

You have to form a team of 3 soldiers amongst them under the following rules:

Choose 3 soldiers with indices `(i, j, k)` with ratings `(rating[i], rating[j], rating[k])`.
A team is valid if: `(rating[i] < rating[j] < rating[k])` or `(rating[i] > rating[j] > rating[k])` where `(0 <= i < j < k < n)`.
Return the number of teams you can form given the conditions. (Soldiers can be part of multiple teams).

## Approach

1. Iterate through each soldier and count how many soldiers have higher and lower ratings both to the left and to the right.
2. Calculate the number of valid teams by multiplying the counts of valid pairs found to the left and to the right.

### Complexity

- **Time Complexity**: O(n^2)
- **Space Complexity**: O(1)

## Code

```cpp
class Solution {
public:
    int numTeams(vector<int>& rating) {
        int total = 0;
        int n = rating.size();

        for (int i = 0; i < n; ++i) {
            int right_less = 0, right_more = 0, left_less = 0, left_more = 0;

            for (int j = i + 1; j < n; ++j) {
                if (rating[j] < rating[i]) 
                    right_less++;
                else if (rating[j] > rating[i]) 
                    right_more++;
            }

            for (int j = 0; j < i; ++j) {
                if (rating[j] < rating[i]) 
                    left_less++;
                else if (rating[j] > rating[i]) 
                    left_more++;
            }

            total += right_less * left_more + right_more * left_less;
        }

        return total;
    }
};
```