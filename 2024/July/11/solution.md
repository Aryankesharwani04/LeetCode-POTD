# Reverse Substrings Between Each Pair of Parentheses
[Problem Link](https://leetcode.com/problems/reverse-substrings-between-each-pair-of-parentheses/description/)

## Problem Description

You are given a string `s` that consists of lower case English letters and brackets.

Reverse the strings in each pair of matching parentheses, starting from the innermost one.

Your result should not contain any brackets.

## Approach

1. Initialize Variables:
    - Use a stack `indices` to store the indices where we encounter the opening brackets `(`.
    - Use a string `res` to store the result as we process the input string `s`.
2. Iterate Through the String:
    - Loop through each character in the string `s`:
        - If the character is an opening bracket `(`:
            - Push the current size of `res` onto the `indices` stack. This helps track where to start reversing when we encounter the closing bracket as if I push the `i` indices directly it will count the brackets of the string `s` also. So for correct reversal we track from `res` size.
        - If the character is a closing bracket `)`:
            - Reverse the substring in `res` starting from the index stored at the top of the `indices` stack to the current end of `res`.
            - Pop the top value from the `indices` stack as it is no longer needed.
        - If the character is a lowercase letter:
            - Append it directly to `res`.
3. Return the Result:
    - The result stored in `res` after processing all characters is the required string with reversed substrings for each pair of matching parentheses.

### Complexity

- Time complexity: O(n)
- Space complexity: O(n)

## Code

```cpp
class Solution {
public:
    string reverseParentheses(string s) {
        stack<int> indices;
        string res;
        for(int i = 0; i<s.size(); i++){
            if(s[i] == '(')
                indices.push(res.size());
            else{
                if(s[i] == ')'){
                    reverse(res.begin() + indices.top(), res.end());
                    indices.pop();
                }else
                    res += s[i];
            }
        }
        if(!indices.empty()) reverse(res.begin(), res.end());
        return res;
    }
};