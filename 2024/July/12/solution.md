# Maximum Score From Removing Substrings
[Problem Link](https://leetcode.com/problems/maximum-score-from-removing-substrings/description/)

## Problem Description

You are given a string `s` and two integers `x` and `y`. You can perform two types of operations any number of times.

- Remove substring `"ab"` and gain `x` points.
For example, when removing `"ab"` from `"cabxbae"` it becomes `"cxbae"`.
- Remove substring `"ba"` and gain `y` points.
For example, when removing `"ba"` from `"cabxbae"` it becomes `"cabxe"`.

Return the maximum points you can gain after applying the above operations on `s`.

## Approach

1. Initial Setup:
    - Initialize a variable `count` to 0. This will store the total points gained from removing substrings.
    - Use a string `c` to help process the characters in `s`.
2. First Pass:
    - Determine which operation yields more points by comparing `x` and `y`.
    - If `x` (points for removing "ab") is greater than `y` (points for removing "ba"), prioritize removing "ab" first.
    - If `y` is greater than or equal to `x`, prioritize removing "ba" first.
3. Iterate through each character in the string `s`:
    - If x > y:
        - Append characters to `c`.
        - If a "b is encountered and the last character in `c` is "a, remove "ab" and add `x` points to `count`.
    - If y >= x:
        - Append characters to c.
        - If an "a" is encountered and the last character in `c` is "b", remove "ba" and add `y` points to `count`.
4. Second Pass:
    - After the first pass, process the remaining string `c` to remove the other type of substring.
    - Use another string remaining to process the leftover characters in `c`.
    - Again, prioritize based on whether x > y or y >= x:
    - If x > y:
        - Remove "ba" and add `y` points to `count` because now we append all the smaller from x and y as we have already appended all the larger ones.
    - If y >= x:
        - Remove "ab" and add `x` points to `count`.
5. Return Result:
    - The final value of `count` will be the maximum points gained from removing the substrings.

### Complexity

- Time complexity: O(n)
- Space complexity: O(n)

## Code

```cpp
class Solution {
public:
    int maximumGain(string s, int x, int y) {
        int count = 0;
        string c;
        for(int i = 0; i<s.size(); i++){
            if(x > y){
                if(c.empty()) c += s[i];
                else{
                    if(s[i] == 'b' && c.back() == 'a'){
                        count += x;
                        c.pop_back();
                    }else{
                        c += s[i];
                    }
                }
            }else{
                if(c.empty()) c += s[i];
                else{
                    if(s[i] == 'a' && c.back() == 'b'){
                        count += y;
                        c.pop_back();
                    }else{
                        c += s[i];
                    }
                }
            }
        }
        string remaining;
        for(char ch : c){
            if(x > y){
                if(!remaining.empty() && remaining.back() == 'b' && ch == 'a'){
                    count += y;
                    remaining.pop_back();
                }else{
                    remaining += ch;
                }
            }else{
                if(!remaining.empty() && remaining.back() == 'a' && ch == 'b'){
                    count += x;
                    remaining.pop_back();
                }else{
                    remaining += ch;
                }
            }
        }
        return count;
    }
};