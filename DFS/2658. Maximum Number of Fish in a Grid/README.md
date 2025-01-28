# Leetcode - 2658. Maximum Number of Fish in a Grid (M-)

[Leetcode](https://leetcode.com/problems/maximum-number-of-fish-in-a-grid/)

You are given a **0-indexed** 2D matrix `grid` of size `m x n`, where `(r, c)` represents:

-   A **land** cell if `grid[r][c] = 0`, or
-   A **water** cell containing `grid[r][c]` fish, if `grid[r][c] > 0`.

A fisher can start at any **water** cell `(r, c)` and can do the following operations any number of times:

-   Catch all the fish at cell `(r, c)`, or
-   Move to any adjacent **water** cell.

Return _the **maximum** number of fish the fisher can catch if he chooses his starting cell optimally, or _`0` if no water cell exists.

An **adjacent** cell of the cell `(r, c)`, is one of the cells `(r, c + 1)`, `(r, c - 1)`, `(r + 1, c)` or `(r - 1, c)` if it exists.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2023/03/29/example.png)
> 
> **Input:** grid = [[0,2,1,0],[4,0,0,3],[1,0,0,4],[0,3,2,0]]
> **Output:** 7
> **Explanation:** The fisher can start at cell `(1,3)` and collect 3 fish, then move to cell `(2,3)` and collect 4 fish.

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2023/03/29/example2.png)
> 
> **Input:** grid = [[1,0,0,0],[0,0,0,0],[0,0,0,0],[0,0,0,1]]
> **Output:** 1
> **Explanation:** The fisher can start at cells (0,0) or (3,3) and collect a single fish. 

**Constraints:**

-   `m == grid.length`
-   `n == grid[i].length`
-   `1 <= m, n <= 10`
-   `0 <= grid[i][j] <= 10`

---
```java
// Java 3ms(Beats 100.00%), Time O(M*N), Space O(1)
class Solution {
    public int findMaxFish(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int ret = 0;
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
                if (grid[i][j] > 0)
                    ret = Math.max(ret, dfs(grid, i, j));

        return ret; 
    }

    int dfs(int[][] grid, int i, int j)
    {
        int m = grid.length, n = grid[0].length;
        int ret = 0;
        int[] dir = new int[] {1, 0, -1, 0, 1};
        ret += grid[i][j];
        grid[i][j] = 0;
        for (int k = 0; k < 4; k++)
        {
            int x = i + dir[k];
            int y = j + dir[k + 1];
            if (x < 0 || y < 0 || x >= m || y >= n || grid[x][y] <= 0)
                continue;
            ret += dfs(grid, x, y);
        }

        return ret;
    }
}
```
---

將每個點走過, 若遇到`grid[i][j] > 0`時, 則從`[i, j]`開始`DFS`, 將相鄰且`>0`的點加總起來
因此`DFS`需將走過的點更新為`grid[i][j] = 0`
若遇到`grid[i][j] == 0`則不繼續走, 代表為已走過或是陸地


###### tags: `Leetcode` `DFS`