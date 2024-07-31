## Filling Bookcase Shelves
[Problem Link](https://leetcode.com/problems/filling-bookcase-shelves/description/)

## Problem Description

You are given an array `books` where `books[i] = [thickness_i, height_i]` indicates the thickness and height of the `i-th` book. You are also given an integer `shelfWidth`.

We want to place these books in order onto bookcase shelves that have a total width of `shelfWidth`.

We choose some of the books to place on this shelf such that the sum of their thickness is less than or equal to `shelfWidth`, then build another level of the shelf of the bookcase so that the total height of the bookcase has increased by the maximum height of the books we just put down. We repeat this process until there are no more books to place.

Note that at each step of the above process, the order of the books we place is the same order as the given sequence of books.

For example, if we have an ordered list of 5 books, we might place the first and second book onto the first shelf, the third book on the second shelf, and the fourth and fifth book on the last shelf.
Return the minimum possible height that the total bookshelf can be after placing shelves in this manner.

## Approach

1. **Initialization**:
    - Use a dynamic programming array `dp` where `dp[i]` represents the minimum height needed to place the first `i` books.

2. **Dynamic Programming**:
    - For each book `i`, consider placing books from `j` to `i` on the same shelf and compute the minimum height of the shelf.
    - Update `dp[i]` by considering the minimum height achievable by placing books `j` to `i` on the same shelf and adding it to `dp[j-1]` which represents the height of the previous shelves.

### Complexity

- **Time Complexity**: O(n^2)
- **Space Complexity**: O(n)

## Code

```cpp
class Solution {
public:
    int minHeightShelves(vector<vector<int>>& books, int shelfWidth) {
        int n = books.size();
        vector<int> dp(n + 1, INT_MAX);
        dp[0] = 0;
        
        for (int i = 1; i <= n; ++i) {
            int total_width = 0;
            int max_height = 0;
            for (int j = i; j > 0; --j) {
                total_width += books[j-1][0];
                if (total_width > shelfWidth) {
                    break;
                }
                max_height = max(max_height, books[j-1][1]);
                dp[i] = min(dp[i], dp[j-1] + max_height);
            }
        }
        
        return dp[n];
    }
};
```