# Leetcode - 2257. Count Unguarded Cells in the Grid (M)

[Leetcode](https://leetcode.com/problems/count-unguarded-cells-in-the-grid/)

You are given two integers `m` and `n` representing a **0-indexed** `m x n` grid. You are also given two 2D integer arrays `guards` and `walls` where `guards[i] = [rowi, coli]` and `walls[j] = [rowj, colj]` represent the positions of the `ith` guard and `jth` wall respectively.

A guard can see **every** cell in the four cardinal directions (north, east, south, or west) starting from their position unless **obstructed** by a wall or another guard. A cell is **guarded** if there is **at least** one guard that can see it.

Return_ the number of unoccupied cells that are **not** **guarded**._

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2022/03/10/example1drawio2.png)
> 
> **Input:** m = 4, n = 6, guards = [[0,0],[1,1],[2,3]], walls = [[0,1],[2,2],[1,4]]
> **Output:** 7
> **Explanation:** The guarded and unguarded cells are shown in red and green respectively in the above diagram.
> There are a total of 7 unguarded cells, so we return 7.

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2022/03/10/example2drawio.png)
> 
> **Input:** m = 3, n = 3, guards = [[1,1]], walls = [[0,1],[1,0],[2,1],[1,2]]
> **Output:** 4
> **Explanation:** The unguarded cells are shown in green in the above diagram.
> There are a total of 4 unguarded cells, so we return 4.

**Constraints:**

-   `1 <= m, n <= 105`
-   `2 <= m * n <= 105`
-   `1 <= guards.length, walls.length <= 5 * 104`
-   `2 <= guards.length + walls.length <= m * n`
-   `guards[i].length == walls[j].length == 2`
-   `0 <= rowi, rowj < m`
-   `0 <= coli, colj < n`
-   All the positions in `guards` and `walls` are **unique**.

---
```java
// Java 22ms(Beats 66.67%), Time O(M*N), Space O(M*N)
class Solution {
    public int countUnguarded(int m, int n, int[][] guards, int[][] walls) {
        int[][] dp = new int[m][n];
        int ret = 0;
        for (int[] g : guards)
            dp[g[0]][g[1]] = 1;
        for (int[] w : walls)
            dp[w[0]][w[1]] = 1;
        int[] dir = new int[] {1, 0, -1, 0, 1};
        for (int[] g : guards)
            for (int k = 0; k < 4; k++)
            {
                int x = g[0] + dir[k];
                int y = g[1] + dir[k + 1];
                while (!(x < 0 || y < 0 || x >= m || y >= n || dp[x][y] == 1))
                {
                    dp[x][y] = 2;
                    x += dir[k];
                    y += dir[k + 1];
                }
            }
        
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
                if (dp[i][j] == 0)
                    ret++;
        
        return ret;
    }
}
```
---

從guard開始, 往上下左右四個方向掃, 碰到guards或walls則停止

###### tags: `Leetcode` `Greedy`