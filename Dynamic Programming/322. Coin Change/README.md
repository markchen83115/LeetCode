# Leetcode - 322. Coin Change (M)

[Leetcode](https://leetcode.com/problems/coin-change/)

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return _the fewest number of coins that you need to make up that amount_. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

**Example 1:**
```
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```
**Example 2:**
```
Input: coins = [2], amount = 3
Output: -1
```
**Example 3:**
```
Input: coins = [1], amount = 0
Output: 0
```
**Constraints:**

-   `1 <= coins.length <= 12`
-   `1 <= coins[i] <= 231 - 1`
-   `0 <= amount <= 104`

---
```java
// Java 11ms(Beats 96.22%), Time O(N*K), Space O(N) 
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1);
        dp[0] = 0;
        for (int i = 1; i <= amount; i++)
            for (int coin : coins)
                if (i - coin >= 0)
                    dp[i] = Math.min(dp[i], dp[i - coin] + 1);
        
        return dp[amount] < amount + 1 ? dp[amount] : -1;
    }
}
```
---
`dp[i]` = 數字`i`可用最少幾次硬幣組成

動態轉移方程: 
`dp[i] = dp[i - coins[j]] + 1`



###### tags: `Leetcode` `Dynamic Programming` `背包型`