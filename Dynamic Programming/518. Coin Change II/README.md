# Leetcode - 518. Coin Change II (H-)

[Leetcode](https://leetcode.com/problems/coin-change-ii/)

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return _the number of combinations that make up that amount_. If that amount of money cannot be made up by any combination of the coins, return `0`.

You may assume that you have an infinite number of each kind of coin.

The answer is **guaranteed** to fit into a signed **32-bit** integer.

**Example 1:**

> **Input:** amount = 5, coins = [1,2,5]
> **Output:** 4
> **Explanation:** there are four ways to make up the amount:
> 5=5
> 5=2+2+1
> 5=2+1+1+1
> 5=1+1+1+1+1

**Example 2:**

> **Input:** amount = 3, coins = [2]
> **Output:** 0
> **Explanation:** the amount of 3 cannot be made up just with coins of 2.

**Example 3:**

> **Input:** amount = 10, coins = [10]
> **Output:** 1

**Constraints:**

-   `1 <= coins.length <= 300`
-   `1 <= coins[i] <= 5000`
-   All the values of `coins` are **unique**.
-   `0 <= amount <= 5000`

---
```java
// Java 3ms(Beats 100.00%), Time O(N^2), Space O(N)
class Solution {
    public int change(int amount, int[] coins) {
        int[] dp = new int[amount + 1];
        dp[0] = 1;
        for (int coin : coins)
            for (int i = coin; i <= amount; i++)
                dp[i] += dp[i - coin];

        return dp[amount];
    }
}
```
---

[518. Coin Change 2 - YouTube](https://youtu.be/KkBwcRBhSyM)
01背包: 每個物品只能用一次
```
for i in obj:			// 遍歷物品
	for c in capcity:	// 遍歷容量		
        dp[c] = max(dp[c], dp[c-cost[i]]+val[i];
```
完全背包: 每個物品可以用無限次
```
for c in capcity:		// 遍歷容量	
    for i in obj:		// 遍歷物品
        dp[c] = max(dp[c], dp[c-cost[i]]+val[i];
```

此題若使用完全背包解題則會有錯誤, 因為會重複計算數量
3 = 2 + 1
3 = 1 + 2
順序不同, 造成重複計算, 正確只能計算為一次


###### tags: `Leetcode` `Dynamic Programming` `背包型`