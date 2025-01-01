# Leetcode - 213. House Robber II (M+)

[Leetcode](https://leetcode.com/problems/house-robber-ii/)

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return _the maximum amount of money you can rob tonight **without alerting the police**_.

**Example 1:**

> **Input:** nums = [2,3,2]
> **Output:** 3
> **Explanation:** You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.

**Example 2:**

> **Input:** nums = [1,2,3,1]
> **Output:** 4
> **Explanation:** Rob house 1 (money = 1) and then rob house 3 (money = 3).
> Total amount you can rob = 1 + 3 = 4.

**Example 3:**

> **Input:** nums = [1,2,3]
> **Output:** 3

**Constraints:**

-   `1 <= nums.length <= 100`
-   `0 <= nums[i] <= 1000`

---
```java
// Java 0ms(Beats 100.00%), Time O(N), Space O(1)
class Solution {
    public int rob(int[] nums) {
        int ret = 0;
        int n = nums.length;
        if (n == 1)
            return nums[0];
        int rob = nums[1], notRob = 0;
        // 第一家 not rob, 最後一家 rob
        for (int i = 2; i < n; i++)
        {
            int tmpRob = rob, tmpNotRob = notRob;
            rob = tmpNotRob + nums[i];
            notRob = Math.max(tmpRob, tmpNotRob);
        }
        ret = Math.max(rob, notRob);

        // 第一家 rob, 最後一家 not rob
        rob = nums[0];
        notRob = 0;
        for (int i = 1; i < n - 1; i++)
        {
            int tmpRob = rob, tmpNotRob = notRob;
            rob = tmpNotRob + nums[i];
            notRob = Math.max(tmpRob, tmpNotRob);
        }
        ret = Math.max(ret, Math.max(rob, notRob));

        return ret;
    }
}
```
```java
// Java 0ms(Beats 100.00%), Time O(N), Space O(N)
class Solution {
    public int rob(int[] nums) {
        int ret = 0;
        int n = nums.length;
        if (n == 1)
            return nums[0];
        int[][] dp = new int[n][2];
        // 第一家 not rob, 最後一家 rob
        dp[1][1] = nums[1];
        for (int i = 2; i < n; i++)
        {
                dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1]);
                dp[i][1] = dp[i-1][0] + nums[i];
        }
        ret = Math.max(dp[n-1][0], dp[n-1][1]);

        // 第一家 rob, 最後一家 not rob
        dp = new int[n][2];
        dp[0][1] = nums[0];
        for (int i = 1; i < n - 1; i++)
        {
            dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1]);
            dp[i][1] = dp[i-1][0] + nums[i];
        }
        ret = Math.max(ret, Math.max(dp[n-2][0], dp[n-2][1]));

        return ret;
    }
}
```
---

參考[【每日一题】213. House Robber II, 3/8/2020 - YouTube](https://youtu.be/5NsRK9TDCRo)

因為環的關係, 第一家跟最後一家不能同時選
1. 如果第一家不選, 則從`nums[1]~nums[n-1]`的選擇就沒有環形的影響, 也就是`198.House Robber I`的問題
2. 如果最後一家不選, 則從`nums[0]~nums[n-2]`的選擇就沒有環形的影響, 也同樣是`198.House Robber I`的問題

我們將這兩種狀況的最佳解取最大值, 就是本題的最佳解


###### tags: `Leetcode` `Dynamic Programming` `基本型 I`