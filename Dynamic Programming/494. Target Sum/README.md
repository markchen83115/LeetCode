# Leetcode - 494. Target Sum (M+)

[Leetcode](https://leetcode.com/problems/target-sum/)

You are given an integer array `nums` and an integer `target`.

You want to build an **expression** out of nums by adding one of the symbols `'+'` and `'-'` before each integer in nums and then concatenate all the integers.

-   For example, if `nums = [2, 1]`, you can add a `'+'` before `2` and a `'-'` before `1` and concatenate them to build the expression `"+2-1"`.

Return the number of different **expressions** that you can build, which evaluates to `target`.

**Example 1:**

> **Input:** nums = [1,1,1,1,1], target = 3
> **Output:** 5
> **Explanation:** There are 5 ways to assign symbols to make the sum of nums be target 3.
> -1 + 1 + 1 + 1 + 1 = 3
> +1 - 1 + 1 + 1 + 1 = 3
> +1 + 1 - 1 + 1 + 1 = 3
> +1 + 1 + 1 - 1 + 1 = 3
> +1 + 1 + 1 + 1 - 1 = 3

**Example 2:**

> **Input:** nums = [1], target = 1
> **Output:** 1

**Constraints:**

-   `1 <= nums.length <= 20`
-   `0 <= nums[i] <= 1000`
-   `0 <= sum(nums[i]) <= 1000`
-   `-1000 <= target <= 1000`

---
```java
// Java 10ms(Beats 61.23%), Time O(N^2), Space O(N)
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int offset = 1000;
        int n = nums.length;
        int[][] dp = new int[n + 1][2005];
        dp[0][0 + offset] = 1;
        for (int i = 1; i <= n; i++)
            for (int j = -1000; j <= 1000; j++)
            {
                int k = nums[i - 1];
                if (dp[i - 1][j + offset] > 0)
                {
                    if (-1000 <= j + k && j + k <= 1000)
                        dp[i][j + offset + k] += dp[i - 1][j + offset];
                    if (-1000 <= j - k && j - k <= 1000)
                        dp[i][j + offset - k] += dp[i - 1][j + offset];
                }
            }
        
        return dp[n][target + offset];
    }
}
```
---

`dp[i][j]` = nums的前`i`個數字加總為`j`的組合數
動態轉移方程:
```java
dp[i][j + nums[i-1]] = dp[i-1][j]
dp[i][j - nums[i-1]] = dp[i-1][j]
```

需注意j的範圍為`-1000~1000`
因此利用`offset = 1000`來使得array的`index >= 0`


###### tags: `Leetcode` `Dynamic Programming` `背包型`