# Average Waiting Time
[Problem Link](https://leetcode.com/problems/average-waiting-time/description/)

## Problem Description

There is a restaurant with a single chef. You are given an array customers, where `customers[i] = [arrivali, timei]`:

arrival `i` is the arrival time of the `ith` customer. The arrival times are sorted in non-decreasing order.
time `i` is the time needed to prepare the order of the ith customer.
When a customer arrives, he gives the chef his order, and the chef starts preparing it once he is idle. The customer waits till the chef finishes preparing his order. The chef does not prepare food for more than one customer at a time. The chef prepares food for customers in the order they were given in the input.

Return the average waiting time of all customers. Solutions within 10<sup>-5</sup> from the actual answer are considered accepted.

## Approach

1. Initialize Variables:
    - Start by initializing `start` to the arrival time of the first customer `customers[0][0]`.
    - Calculate `end` as the sum of the `start` time and the time needed to prepare the first order `customers[0][1]`.
    - Initialize `waitingTime` to store the total waiting time, starting with the waiting time for the first customer `(end - start)`.
2. Iterate Through Customers:
    - For each subsequent customer, determine whether the chef is still busy when the customer arrives:
        - If the arrival time of the current customer `(customers[i][0])` is less than or equal to the `end` time of the last order, the chef continues from the `end` time of the last order. Calculate the waiting time as `end - customers[i][0]` and add it to `waitingTime`.
        - If the arrival time of the current customer is greater than the `end` time of the last order, update `start` to the arrival time of the current customer `(customers[i][0])`.
    - Update the `end` time to the sum of the `start` time and the time needed to prepare the current order `(customers[i][1] + start)`.
    - Add the total time taken to serve the current customer `(end - start)` to `waitingTime`.
3. Calculate Average Waiting Time:
    - Divide and return the total `waitingTime` by the `number of customers` to get the `average waiting time`.

### Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Code

```cpp
class Solution {
public:
    double averageWaitingTime(vector<vector<int>>& customers) {
        int start = customers[0][0];
        int end = start + customers[0][1];
        double waitingTime = end - start;

        for(int i = 1; i<customers.size(); i++){
            if(customers[i][0] <= end){
                start = end;
                waitingTime += end - customers[i][0];
            }else
                start = customers[i][0];
            end = customers[i][1] + start;
            waitingTime += (end - start);
        }

        return waitingTime/customers.size();
    }
};