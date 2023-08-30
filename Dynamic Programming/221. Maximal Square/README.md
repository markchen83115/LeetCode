# Leetcode - 221. Maximal Square (H-)

[Leetcode](https://leetcode.com/problems/maximal-square/)

Given an `m x n` binary `matrix` filled with `0`'s and `1`'s, _find the largest square containing only_ `1`'s _and return its area_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/26/max1grid.jpg)
```
Input: matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
Output: 4
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/26/max2grid.jpg)
```
Input: matrix = [["0","1"],["1","0"]]
Output: 1
```
**Example 3:**
```
Input: matrix = [["0"]]
Output: 0
```
**Constraints:**

-   `m == matrix.length`
-   `n == matrix[i].length`
-   `1 <= m, n <= 300`
-   `matrix[i][j]` is `'0'` or `'1'`.

---
```java
// Java DP, 7ms(Beats 66.95%), Time O(N^2), Space O(N^2)
class Solution {
    public int maximalSquare(char[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        int[][] dp = new int[m+1][n+1];
        int ret = 0;
        //dp[i+1][j+1] = matrix[i][j]
        //dp[i][j] = matrix[i-1][j-1]

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (matrix[i-1][j-1] == '1') {
                    dp[i][j] = Math.min(dp[i-1][j-1], Math.min(dp[i-1][j], dp[i][j-1])) + 1;
                    ret = Math.max(ret, dp[i][j]);
                }
            }
        }
        return ret * ret;
    }
}
```
---


###### tags: `Leetcode` `Dynamic Programming` `Stack` `Monotonic stack: next greater / smaller`