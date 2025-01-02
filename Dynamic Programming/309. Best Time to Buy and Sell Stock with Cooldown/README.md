# Leetcode - 309. Best Time to Buy and Sell Stock with Cooldown (H-)

[Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:

-   After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example 1:**

> **Input:** prices = mod[1,2,3,0,2mod]
> **Output:** 3
> **Explanation:** transactions = mod[buy, sell, cooldown, buy, sellmod]

**Example 2:**

> **Input:** prices = mod[1mod]
> **Output:** 0

**Constraints:**

-   `1 <= prices.length <= 5000`
-   `0 <= prices[i] <= 1000`

---
```java
// Java 1ms(Beats 81.80%), Time O(N), Space O(1)
class Solution {
    public int maxProfit(int[] prices) {
        int hold = Integer.MIN_VALUE, sold = 0, cooldown = 0;
        for (int i = 0; i < prices.length; i++)
        {
            int preSold = sold, preHold = hold, preCooldown = cooldown;
            hold = Math.max(preHold, preCooldown - prices[i]);
            sold = preHold + prices[i];
            cooldown = Math.max(preCooldown, preSold);
        }

        return Math.max(sold, cooldown);
    }
}
```
---

此題比較特殊的情況是，僅有hold和sold兩個狀態是不夠的
考慮hold表示手上有股票時候的收益，sold表示手上已經賣出了股票的收益

動態轉移方程:
```
hold = max(hold, cooldown - prices[i])
sold = hold + prices[i]
cooldown = max(cooldown, sold)
```


###### tags: `Leetcode` `Dynamic Programming` `基本型 I`