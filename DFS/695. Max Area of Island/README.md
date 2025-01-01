# Leetcode - 695. Max Area of Island (M-)

[Leetcode](https://leetcode.com/problems/max-area-of-island/)

You are given an `m x n` binary matrix `grid`. An island is a group of `1`'s (representing land) connected **4-directionally** (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The **area** of an island is the number of cells with a value `1` in the island.

Return _the maximum **area** of an island in _`grid`. If there is no island, return `0`.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2021/05/01/maxarea1-grid.jpg)
> 
> **Input:** grid = [[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]
> **Output:** 6
> **Explanation:** The answer is not 11, because the island must be connected 4-directionally.

**Example 2:**

> **Input:** grid = [[0,0,0,0,0,0,0,0]]
> **Output:** 0

**Constraints:**

-   `m == grid.length`
-   `n == grid[i].length`
-   `1 <= m, n <= 50`
-   `grid[i][j]` is either `0` or `1`.

---
```java
// Java 2ms(Beats 69.77%), Time O(M*N), Space O(M*N)
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int ret = 0;
        for (int i = 0; i < m ; i++)
            for (int j = 0; j < n; j++)
                if (grid[i][j] == 1)
                    ret = Math.max(ret, dfs(grid, i, j));

        return ret;
    }

    int dfs(int[][] grid, int i, int j)
    {
        int ret = 0;
        int m = grid.length, n = grid[0].length;
        int[] dir = new int[] {1, 0, -1, 0, 1};
        grid[i][j] = 0;
        ret++;
        for (int k = 0; k < 4; k++)
        {
            int x = i + dir[k];
            int y = j + dir[k+1];
            if (x < 0 || y < 0 || x >= m || y >= n)
                continue;
            if (grid[x][y] == 0)
                continue;
            ret += dfs(grid, x, y);
        }

        return ret;
    }
}
```
---

經典的`DFS`題目
當遇到`grid[i][j] = 1`時, 執行`DFS`
將走過的點標為0, 避免後續重複走
並且同時計算走過的步數


###### tags: `Leetcode` `DFS`