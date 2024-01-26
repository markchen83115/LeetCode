# Leetcode - 576. Out of Boundary Paths (H)

[Leetcode](https://leetcode.com/problems/out-of-boundary-paths/)

There is an `m x n` grid with a ball. The ball is initially at the position `[startRow, startColumn]`. You are allowed to move the ball to one of the four adjacent cells in the grid (possibly out of the grid crossing the grid boundary). You can apply **at most** `maxMove` moves to the ball.

Given the five integers `m`, `n`, `maxMove`, `startRow`, `startColumn`, return the number of paths to move the ball out of the grid boundary. Since the answer can be very large, return it **modulo** `109 + 7`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/28/out_of_boundary_paths_1.png)
```
Input: m = 2, n = 2, maxMove = 2, startRow = 0, startColumn = 0
Output: 6
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/28/out_of_boundary_paths_2.png)
```
Input: m = 1, n = 3, maxMove = 3, startRow = 0, startColumn = 1
Output: 12
```
**Constraints:**

-   `1 <= m, n <= 50`
-   `0 <= maxMove <= 50`
-   `0 <= startRow < m`
-   `0 <= startColumn < n`

---
```java
// Java DP 10ms (Beats 28.00%), Time O(Nmn), Space O(Nmn)
class Solution {
    public int findPaths(int m, int n, int maxMove, int startRow, int startColumn) {
        int mod = (int)1e9 + 7;
        int[][][] dp = new int[m][n][maxMove + 1];
        int[] dir = new int[] {1, 0, -1, 0, 1};

        for (int k = 1; k <= maxMove; k++)
            for (int i = 0; i < m; i++)
                for (int j = 0; j < n; j++)
                    for (int t = 0; t < 4; t++)
                    {
                        int x = i + dir[t];
                        int y = j + dir[t + 1];

                        if (x < 0 || x >= m || y < 0 || y >=n)
                            dp[i][j][k] = (dp[i][j][k] + 1) % mod;
                        else
                            dp[i][j][k] = (dp[i][j][k] + dp[x][y][k - 1]) % mod;
                    }
        
        return dp[startRow][startColumn][maxMove];

    }
}
```

---

從邊界開始往回算, 
`dp[i][j][k]`代表經過`k`步之後, 到達`(i,j)`的方法有多少,
直接回傳答案即可

###### tags: `Leetcode`