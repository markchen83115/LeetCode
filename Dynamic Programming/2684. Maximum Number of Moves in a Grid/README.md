# Leetcode - 2684. Maximum Number of Moves in a Grid (M)

[Leetcode](https://leetcode.com/problems/maximum-number-of-moves-in-a-grid/)

You are given a **0-indexed** `m x n` matrix `grid` consisting of **positive** integers.

You can start at **any** cell in the first column of the matrix, and traverse the grid in the following way:

-   From a cell `(row, col)`, you can move to any of the cells: `(row - 1, col + 1)`, `(row, col + 1)` and `(row + 1, col + 1)` such that the value of the cell you move to, should be **strictly** bigger than the value of the current cell.

Return _the **maximum** number of **moves** that you can perform._

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2023/04/11/yetgriddrawio-10.png)
> 
> **Input:** grid = [[2,4,3,5],[5,4,9,3],[3,4,2,11],[10,9,13,15]]
> **Output:** 3
> **Explanation:** We can start at the cell (0, 0) and make the following moves:
> - (0, 0) -> (0, 1).
> - (0, 1) -> (1, 2).
> - (1, 2) -> (2, 3).
> It can be shown that it is the maximum number of moves that can be made.

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2023/04/12/yetgrid4drawio.png)
> **Input:** grid = [[3,2,4],[2,1,9],[1,1,7]]
> **Output:** 0
> **Explanation:** Starting from any cell in the first column we cannot perform any moves.

**Constraints:**

-   `m == grid.length`
-   `n == grid[i].length`
-   `2 <= m, n <= 1000`
-   `4 <= m * n <= 105`
-   `1 <= grid[i][j] <= 106`

---
```java
// Java 7ms(Beats 65.38%), Time O(M*N), Space O(M*N)
class Solution {
    public int maxMoves(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][] dp = new int[m][n];
        for (int i = 0; i < m; i++)
            dp[i][0] = 1;

        int ret = 1;
        for (int j = 1; j < n; j++)
            for (int i = 0; i < m; i++)
            {
                if (i > 0 && dp[i - 1][j - 1] > 0 && grid[i - 1][j - 1] < grid[i][j])
                    dp[i][j] = Math.max(dp[i][j], dp[i - 1][j - 1] + 1);
                if (dp[i][j - 1] > 0 && grid[i][j - 1] < grid[i][j])
                    dp[i][j] = Math.max(dp[i][j], dp[i][j - 1] + 1);
                if (i < m - 1 && dp[i+1][j-1] > 0 && grid[i + 1][j - 1] < grid[i][j])
                    dp[i][j] = Math.max(dp[i][j], dp[i+1][j-1] + 1);
                
                ret = Math.max(ret, dp[i][j]);
            }

        return ret - 1;
    }
}
```
---

`dp[i][j]`為以`(i, j)`為終點的最大步數
動態轉移方程 `dp[i][j] = Max(dp[i-1][j-1], dp[i][j-1], dp[i+1][j-1])`
並注意邊際條件

###### tags: `Leetcode` `Dynamic Programming`