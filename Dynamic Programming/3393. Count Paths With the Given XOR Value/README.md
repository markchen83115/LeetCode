# Leetcode - 3393. Count Paths With the Given XOR Value (M-)

[Leetcode](https://leetcode.com/problems/count-paths-with-the-given-xor-value/)

You are given a 2D integer array `grid` with size `m x n`. You are also given an integer `k`.

Your task is to calculate the number of paths you can take from the top-left cell `(0, 0)` to the bottom-right cell `(m - 1, n - 1)` satisfying the following **constraints**:

-   You can either move to the right or down. Formally, from the cell `(i, j)` you may move to the cell `(i, j + 1)` or to the cell `(i + 1, j)` if the target cell _exists_.
-   The `XOR` of all the numbers on the path must be **equal** to `k`.

Return the total number of such paths.

Since the answer can be very large, return the result **modulo** `109 + 7`.

**Example 1:**

> **Input:** grid = [[2, 1, 5], [7, 10, 0], [12, 6, 4]], k = 11
> 
> **Output:** 3
> 
> **Explanation:** 
> 
> The 3 paths are:
> 
> -   `(0, 0) → (1, 0) → (2, 0) → (2, 1) → (2, 2)`
> -   `(0, 0) → (1, 0) → (1, 1) → (1, 2) → (2, 2)`
> -   `(0, 0) → (0, 1) → (1, 1) → (2, 1) → (2, 2)`

**Example 2:**

> **Input:** grid = [[1, 3, 3, 3], [0, 3, 3, 2], [3, 0, 1, 1]], k = 2
> 
> **Output:** 5
> 
> **Explanation:**
> 
> The 5 paths are:
> 
> -   `(0, 0) → (1, 0) → (2, 0) → (2, 1) → (2, 2) → (2, 3)`
> -   `(0, 0) → (1, 0) → (1, 1) → (2, 1) → (2, 2) → (2, 3)`
> -   `(0, 0) → (1, 0) → (1, 1) → (1, 2) → (1, 3) → (2, 3)`
> -   `(0, 0) → (0, 1) → (1, 1) → (1, 2) → (2, 2) → (2, 3)`
> -   `(0, 0) → (0, 1) → (0, 2) → (1, 2) → (2, 2) → (2, 3)`

**Example 3:**

> **Input:** grid = [[1, 1, 1, 2], [3, 0, 3, 2], [3, 0, 2, 2]], k = 10
> 
> **Output:** 0
> 
> **Constraints:**
> 
> -   `1 <= m == grid.length <= 300`
> -   `1 <= n == grid[r].length <= 300`
> -   `0 <= grid[r][c] < 16`
> -   `0 <= k < 16`

---
```java
// Java 46ms(Beats 100.00%), Time O(M*N), Space O(M*N)
class Solution {
    public int countPathsWithXorValue(int[][] grid, int t) {
        int ret = 0;
        int mod = 1000000007;
        int m = grid.length, n = grid[0].length;
        int[][][] dp = new int[m][n][16];
        dp[0][0][grid[0][0]] = 1;
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
                for (int k = 0; k < 16; k++) {
                    int p = grid[i][j];
                    if (i >= 1 && dp[i-1][j][k] > 0) {
                        int q = k ^ p;
                        dp[i][j][q] += dp[i-1][j][k];
                        dp[i][j][q] %= mod;
                    }
                    if (j >= 1 && dp[i][j-1][k] > 0) {
                        int q = k ^ p;
                        dp[i][j][q] += dp[i][j-1][k];
                        dp[i][j][q] %= mod;
                    }
                }

        return dp[m-1][n-1][t];
    }
}
```
---

因為`0 <= k < 16`, 所以可用`dp[i][j][k]`來計算
`dp[i][j][k]` = 走到`grid[i][j]`時, 當前XOR結果為`k`的個數


###### tags: `Leetcode` `Dynamic Programming`