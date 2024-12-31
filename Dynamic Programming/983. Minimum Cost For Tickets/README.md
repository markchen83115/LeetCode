# Leetcode - 983. Minimum Cost For Tickets (H-)

[Leetcode](https://leetcode.com/problems/minimum-cost-for-tickets/)

You have planned some train traveling one year in advance. The days of the year in which you will travel are given as an integer array `days`. Each day is an integer from `1` to `365`.

Train tickets are sold in **three different ways**:

-   a **1-day** pass is sold for `costs[0]` dollars,
-   a **7-day** pass is sold for `costs[1]` dollars, and
-   a **30-day** pass is sold for `costs[2]` dollars.

The passes allow that many days of consecutive travel.

-   For example, if we get a **7-day** pass on day `2`, then we can travel for `7` days: `2`, `3`, `4`, `5`, `6`, `7`, and `8`.

Return _the minimum number of dollars you need to travel every day in the given list of days_.

**Example 1:**

> **Input:** days = [1,4,6,7,8,20], costs = [2,7,15]
> **Output:** 11
> **Explanation:** For example, here is one way to buy passes that lets you travel your travel plan:
> On day 1, you bought a 1-day pass for costs[0] = $2, which covered day 1.
> On day 3, you bought a 7-day pass for costs[1] = $7, which covered days 3, 4, ..., 9.
> On day 20, you bought a 1-day pass for costs[0] = $2, which covered day 20.
> In total, you spent $11 and covered all the days of your travel.

**Example 2:**

> **Input:** days = [1,2,3,4,5,6,7,8,9,10,30,31], costs = [2,7,15]
> **Output:** 17
> **Explanation:** For example, here is one way to buy passes that lets you travel your travel plan:
> On day 1, you bought a 30-day pass for costs[2] = $15 which covered days 1, 2, ..., 30.
> On day 31, you bought a 1-day pass for costs[0] = $2 which covered day 31.
> In total, you spent $17 and covered all the days of your travel.

**Constraints:**

-   `1 <= days.length <= 365`
-   `1 <= days[i] <= 365`
-   `days` is in strictly increasing order.
-   `costs.length == 3`
-   `1 <= costs[i] <= 1000`

---
```java
// Java 1ms(Beats 79.45%), Time O(N), Space O(N)
class Solution {
    public int mincostTickets(int[] days, int[] costs) {
        int n = days[days.length - 1];
        int[] dp = new int[n + 1];
        int j = 0;
        for (int i = 1; i <= n; i++)
        {
            if (i == days[j])
            {
                int one = dp[i - 1] + costs[0];
                int seven = (i < 7 ? 0 : dp[i - 7]) + costs[1];
                int thirty = (i < 30 ? 0 : dp[i - 30]) + costs[2];
                dp[i] = Math.min(one, Math.min(seven, thirty));
                j++;
            }
            else
            {
                dp[i] = dp[i-1];
            }
        }

        return dp[n];
    }
}
```
---

參考[【每日一题】983. Minimum Cost For Tickets, 5/6/2020 - YouTube](https://youtu.be/BgWDlQy5JR0)

`dp[i]` = 前`i`天的最小乘車總花費
如果`j < i`, 則`dp[j] <= dp[i]`, 因為前`j`天的花費肯定<=前`i`天的花費

因此
如果第i天不需要乘車, `dp[i] = dp[i-1]`
如果第i天需要乘車, `dp[i] = min(dp[i-1] + costs[0], dp[i-7] + costs[1], dp[i-30] + costs[2])`

注意, 如果`dp[i-7]`或`dp[i-15]`越界的話, 以`dp[0]`做計算
因為若7天票比1天票便宜, 那肯定購買7天票


###### tags: `Leetcode` `Dynamic Programming` `基本型II`