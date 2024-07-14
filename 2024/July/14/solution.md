# Number of Atoms
[Problem Link](https://leetcode.com/problems/number-of-atoms/description/)

## Problem Description

Given a string formula representing a chemical formula, return the count of each atom.

The atomic element always starts with an uppercase character, then zero or more lowercase letters, representing the name.

One or more digits representing that element's count may follow if the count is greater than `1`. If the count is `1`, no digits will follow.
- For example, "H2O" and "H2O2" are possible, but "H1O2" is impossible.

Two formulas are concatenated together to produce another formula.
- For example, "H2O2He3Mg4" is also a formula.

A formula placed in parentheses, and a count (optionally added) is also a formula.
- For example, "(H2O2)" and "(H2O2)3" are formulas.

Return the count of all elements as a string in the following form: the first name (in sorted order), followed by its count (if that count is more than `1`), followed by the second name (in sorted order), followed by its count (if that count is more than `1`), and so on.

## Approach

1. Initialization:
    - Use a stack `indices` to keep track of maps of element counts at different levels of parentheses.
    - Use a map `temp` to store the current counts of elements.
    - Initialize an index `i` to traverse through the formula string.

2. Traverse the Formula:
    - Opening Parenthesis `(`:
        - Push the current `temp` map onto the stack.
        - Clear `temp` for processing the new sub-formula within the parentheses.
        - Increment `i` to move past the parenthesis.
    - Closing Parenthesis `)`:
        - Increment `i` to move past the parenthesis.
        - Determine the `multiplier` for the enclosed formula:
            - Initialize `start` to the current position of `i`.
            - Move `i` to the end of the number following the parenthesis.
            - Calculate the multiplier `n` based on the digits found, defaulting to `1` if no digits are present.
        - Multiply the counts of elements in the current `temp` map by `n`.
        - Pop the previous map from the stack and merge it with the current `temp` map, applying the multiplier.
    - Element Parsing:
        - Initialize `start` to the current position of `i`.
        - Move `i` to the end of the element name (capital letter followed by lowercase letters).
        - Extract the element name from the `formula` substring.
        - Move `i` to the end of the digits following the element name.
        - Calculate the count of the element based on the digits found, defaulting to `1` if no digits are present.
        - Update the `temp` map with the element `count`.

3. Construct Result:
    - Initialize an empty string `result`.
    - Iterate over the `temp` map (sorted by keys automatically in a map).
    - Append each element name to `result`, followed by its count if the count is greater than `1`.

### Complexity

- Time complexity: O(nlogn)
- Space complexity: O(n)

## Code

```cpp
class Solution {
public:
    string countOfAtoms(string formula) {
        stack<map<string, int>> indices;
        map<string, int> temp;
        int  i = 0;
        while(i < formula.size()){
            if(formula[i] == '('){
                indices.push(temp);
                temp.clear();
                i++;
            }else if(formula[i] == ')'){
                i++;
                int start = i;
                while (i < formula.size() && isdigit(formula[i])) i++;
                int n = (start < i) ? stoi(formula.substr(start, i - start)) : 1;

                if(!indices.empty()){
                    map<string, int> temp2 = temp;
                    temp = indices.top();
                    indices.pop();
                    for(auto& it:temp2){
                        temp[it.first] += it.second * n;
                    }
                }
            }else{
                int start = i;
                i++;
                while (i < formula.size() && islower(formula[i])) i++;
                string element = formula.substr(start, i - start);

                start = i;
                while (i < formula.size() && isdigit(formula[i])) i++;
                int count = (start < i) ? stoi(formula.substr(start, i - start)) : 1;

                temp[element] += count;
            }
        }
        string result;
        for(auto& [element, count] : temp) {
            result += element;
            if(count > 1) result += to_string(count);
        }
        return result;
    }
};