# Water Bottles
[Problem Link](https://leetcode.com/problems/water-bottles/description/)

## Problem Description

There are `numBottles` water bottles that are initially full of water. You can exchange numExchange empty water bottles from the market with one full water bottle.

The operation of drinking a full water bottle turns it into an empty bottle.

Given the two integers `numBottles` and `numExchange`, return the maximum number of water bottles you can drink.

## First Approach (Iterative Exchange Process):

1. Initialize Variables:
    - Start with `numBottles` as full bottles.
    - Initialize `bottlesDrink` to keep track of the total number of bottles drunk, set to `numBottles` as all drinked initially.
    - Initialize bottles to keep track of the current number of empty bottles, initially set to numBottles which are now empty.
2. Exchange Process:
    - While the number of empty bottles are ready to drink and posiible to exchange:
        - Calculate the number of new full bottles obtained by exchanging empty bottles: `numBottles = bottles / numExchange`.
        - Add the new full bottles to bottlesDrink.
        - Update the number of empty bottles: 
            - bottles which are left upon during exchnage
            - and bottles which are drinked after echange 
            - `bottles = bottles % numExchange + numBottles.`
3. Return Result:
    - Return the total number of bottles drunk, bottlesDrink.

### Complexity

    Time complexity: O(n)
    Space complexity: O(1)

## Code

```cpp
class Solution1 {
public:
    int numWaterBottles(int numBottles, int numExchange) {
        int bottlesDrink = numBottles;
        int bottles = numBottles;
        while(bottles >= numExchange){
            numBottles = bottles/numExchange;
            bottlesDrink += numBottles;
            bottles = bottles%numExchange + numBottles;
        }
        return bottlesDrink;
    }
};
```

## Second Approach

1. Initial Drinking and Empty Bottles:
    - Initially, you can drink all `numBottles` full bottles of water. This leaves you with `numBottles` empty bottles.
2. Trading Bottles:
    - Every time you trade in `numExchange` empty bottles, you get one new full bottle. This effectively means you are using `numExchange - 1` empty bottles to get one full bottle (because you need one additional empty bottle to receive the full one).
    - Therefore, to calculate how many additional full bottles you can get by trading, you subtract 1 from `numBottles` and then divide by `numExchange - 1`.
3. Combining Results:
    - The total number of full bottles you can drink is the sum of the initial `numBottles` and the additional bottles obtained through trading.
    - This gives us the formula: `numBottles + (numBottles - 1) / (numExchange - 1)`.

### Complexity

    Time complexity: O(1)
    Space complexity: O(1)

```cpp
class Solution {
public:
    int numWaterBottles(int numBottles, int numExchange) {
        return numBottles + (numBottles - 1)/(numExchange - 1);
    }
};