# Leetcode - 62. Unique Paths (E+)

[Leetcode](https://leetcode.com/problems/unique-paths/description/)

There is a robot on an `m x n` grid. The robot is initially located at the **top-left corner** (i.e., `grid[0][0]`). The robot tries to move to the **bottom-right corner** (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

Given the two integers `m` and `n`, return _the number of possible unique paths that the robot can take to reach the bottom-right corner_.

The test cases are generated so that the answer will be less than or equal to `2 * 109`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)
```
Input: m = 3, n = 7
Output: 28
```
**Example 2:**
```
Input: m = 3, n = 2
Output: 3
Explanation: From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Down -> Down
2. Down -> Down -> Right
3. Down -> Right -> Down
```
**Constraints:**

-   `1 <= m, n <= 100`

---
```java
// Java DP 0ms(Beats 100%), Time O(N^2), SpaceO(N^2)
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];

        dp[0][0] = 1;
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++) {
                if (i >= 1)
                    dp[i][j] += dp[i - 1][j];
                if (j >= 1)
                    dp[i][j] += dp[i][j - 1];
            }

        return dp[m - 1][n - 1];
    }
}
```

```java
// Java 排列組合 0ms(Beats 100%), Time O(N), Space O(1) 
class Solution {
    public int uniquePaths(int m, int n) {
        if (m > n) {
            int tmp = m;
            m = n;
            n = tmp;
        }

        long ret = 1;
        for (int i = 0; i < m - 1; i++) {
            ret = ret * (m + n - 2 - i) / (i + 1);
        }

        return (int)ret;
    }
}
```

---

> 動態規劃

動態轉移方程: `dp[i][j] = dp[i-1][j] + dp[i][j-1]`

> 排列組合

`Example 1`想成, 將`DDLLLLLL`做排列組合
因此為`C 8取2` => `C (m+n-2)取m`


###### tags: `Leetcode` `Dynamic Programming`