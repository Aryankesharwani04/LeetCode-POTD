# Intersection of Two Arrays II
[Problem Link](https://leetcode.com/problems/intersection-of-two-arrays-ii)

## Problem Description

Given two integer arrays nums1 and nums2, return an array of their intersection. Each element in the result must appear as many times as it shows in both arrays and you may return the result in any order.

## Approach

The approach utilizes a frequency array `freq` to count occurrences of each element in `nums1`. Here's a step-by-step breakdown:

1. **Initialize Frequency Array**:
   - Create an array `freq` with size 1001 to accommodate integers ranging from 0 to 1000, assuming this range covers all elements in `nums1` and `nums2`.

2. **Count Frequencies**:
   - Iterate through `nums1` and increment `freq[i]` for each element `i`. This records how many times each number appears in `nums1`.

3. **Intersection Check**:
   - Iterate through `nums2` and check each element `i`. If `freq[i] > 0`, it means `i` exists in `nums1` at least as many times as it appears in `nums2`. Add `i` to `ans` vector and decrement `freq[i]` to account for one occurrence.

4. **Return `ans`**:
   - `ans` contains all elements that appear in both `nums1` and `nums2`, respecting their frequencies.

### Complexity

- Time Complexity : O(m + n), where m and n are the lengths of `nums1` and `nums2`, respectively.

- Space Complexity : O(1) extra space (excluding input space).

## Code

```cpp
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        int freq[1001];
        vector<int> ans;
        for(int& i : nums1)
            freq[i]++;
        for(int& i:nums2){
            if(freq[i]){
                ans.push_back(i);
                freq[i]--;
            }
        }
        return ans;
    }
};
