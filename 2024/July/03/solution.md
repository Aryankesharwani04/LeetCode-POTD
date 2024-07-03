# Minimum Difference Between Largest and Smallest Value in Three Moves
[Problem Link](https://leetcode.com/problems/minimum-difference-between-largest-and-smallest-value-in-three-moves/)

## Problem Description

You are given an integer array nums.
In one move, you can choose one element of nums and change it to any value.
Return the minimum difference between the largest and smallest value of nums after performing at most three moves.

## Approach

- As it is saying to find minimum difference of largest and smallest number in the array. To get largest and smallest why don't we sort it first.
- Now as we have largest and smallest at last and first index respectively. To minimise the difference we have to make largest and smallest as close as possible in just 3 moves and also taking care of others while minimising difference.
- So we have the base case here as when array size is less than 5 we are proving that it will be zero because we have atmost 3 operations to make all elements equal to some elements and their maximum and minimum will be same. 
Let's prove above by following example 
    -`arr = [2,3,4,4]` in our first step
    - `2 -> 3 , 4 -> 3, 4 -> 3`
    - `arr = [3,3,3,3]` in our second step
    - which means our difference is `0` for all cases of array size less than 5.
- Now we know we have atmost 3 chances to change the number. 
- Why don't we just take largest element - 3rd index element -> thinking that we have make all the first 3 index `0, 1, 2` same to 3rd index. As array is sorted hence answer will be `last element - 4th index element` 
- But the problem is that we have to explore all the 3 more cases that will be excluding last including 2nd and so on till we reach `0th` element. By this way we would store the minimum of all the possible cases.
- As we have to minimise the difference we think of playing with minimising largest and smallest number to their second largest and second smallest and so on.
- By this way we are sure that we get the minimum difference of largest and smallest number in array as we have minimised the largest and smallest to the most closest to get minimum difference.

### Complexity

- Time complexity: O(nlogn)
- Space complexity: O(1)

## Code

```cpp
class Solution {
public:
    int minDifference(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        if(nums.size() < 5)   return 0;
        int n = nums.size() - 1;
        return min(nums[n] - nums[3], min(nums[n - 1] - nums[2], min(nums[n - 2] - nums[1], nums[n - 3] - nums[0])));
    }
};
