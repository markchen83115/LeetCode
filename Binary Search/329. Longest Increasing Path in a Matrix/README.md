# Leetcode - 329. Longest Increasing Path in a Matrix (M)

[Leetcode](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/description/)

Given an `m x n` integers `matrix`, return _the length of the longest increasing path in _`matrix`.

From each cell, you can either move in four directions: left, right, up, or down. You **may not** move **diagonally** or move **outside the boundary** (i.e., wrap-around is not allowed).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/05/grid1.jpg)
```
Input: matrix = [[9,9,4],[6,6,8],[2,1,1]]
Output: 4
Explanation: The longest increasing path is `[1, 2, 6, 9]`.
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/27/tmp-grid.jpg)
```
Input: matrix = [[3,4,5],[3,2,6],[2,2,1]]
Output: 4
Explanation: The longest increasing path is `[3, 4, 5, 6]`. Moving diagonally is not allowed.
```
**Example 3:**
```
Input: matrix = [[1]]
Output: 1
```
**Constraints:**

-   `m == matrix.length`
-   `n == matrix[i].length`
-   `1 <= m, n <= 200`
-   `0 <= matrix[i][j] <= 231 - 1`

---

```java
// Java
class Solution {
    int[][] memo = new int[200][200];
    public int longestIncreasingPath(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        int ret = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                ret = Math.max(ret, dfs(matrix, i, j));
            }
        }
        return ret;
    }

    public int dfs(int[][] matrix, int i, int j) {
        if (memo[i][j] > 0)
            return memo[i][j];
        int m = matrix.length;
        int n = matrix[0].length;
        int ret = 1;
        int[] dir = new int[] {0, 1, 0, -1, 0};
        for (int k = 0; k < dir.length - 1; k++) {
            int x = i + dir[k];
            int y = j + dir[k+1];
            if (x < 0 || y < 0 || x >= m || y >= n || matrix[x][y] <= matrix[i][j]) continue;
            ret = Math.max(ret, 1 + dfs(matrix, x, y));
        }
        memo[i][j] = ret;
        return ret;
    }
}
```

---

從每個`matrix[i][j]` DFS出發, 並且記錄由`matrix[i][j]`出發的最大值,
若後續會經過`matrix[i][j]`時, 則不用再次計算


###### tags: `Leetcode` `DFS` `Memorization`
