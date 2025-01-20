# Leetcode - 1368. Minimum Cost to Make at Least One Valid Path in a Grid (H)

[Leetcode](https://leetcode.com/problems/minimum-cost-to-make-at-least-one-valid-path-in-a-grid/)

Given an `m x n` grid. Each cell of the grid has a sign pointing to the next cell you should visit if you are currently in this cell. The sign of `grid[i][j]` can be:

-   `1` which means go to the cell to the right. (i.e go from `grid[i][j]` to `grid[i][j + 1]`)
-   `2` which means go to the cell to the left. (i.e go from `grid[i][j]` to `grid[i][j - 1]`)
-   `3` which means go to the lower cell. (i.e go from `grid[i][j]` to `grid[i + 1][j]`)
-   `4` which means go to the upper cell. (i.e go from `grid[i][j]` to `grid[i - 1][j]`)

Notice that there could be some signs on the cells of the grid that point outside the grid.

You will initially start at the upper left cell `(0, 0)`. A valid path in the grid is a path that starts from the upper left cell `(0, 0)` and ends at the bottom-right cell `(m - 1, n - 1)` following the signs on the grid. The valid path does not have to be the shortest.

You can modify the sign on a cell with `cost = 1`. You can modify the sign on a cell **one time only**.

Return _the minimum cost to make the grid have at least one valid path_.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2020/02/13/grid1.png)
> 
> **Input:** grid = [[1,1,1,1],[2,2,2,2],[1,1,1,1],[2,2,2,2]]
> **Output:** 3
> **Explanation:** You will start at point (0, 0).
> The path to (3, 3) is as follows. (0, 0) --> (0, 1) --> (0, 2) --> (0, 3) change the arrow to down with cost = 1 --> (1, 3) --> (1, 2) --> (1, 1) --> (1, 0) change the arrow to down with cost = 1 --> (2, 0) --> (2, 1) --> (2, 2) --> (2, 3) change the arrow to down with cost = 1 --> (3, 3)
> The total cost = 3.

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2020/02/13/grid2.png)
> 
> **Input:** grid = [[1,1,3],[3,2,2],[1,1,4]]
> **Output:** 0
> **Explanation:** You can follow the path from (0, 0) to (2, 2).

**Example 3:**

> ![](https://assets.leetcode.com/uploads/2020/02/13/grid3.png)
> 
> **Input:** grid = [[1,2],[4,3]]
> **Output:** 1

**Constraints:**

-   `m == grid.length`
-   `n == grid[i].length`
-   `1 <= m, n <= 100`
-   `1 <= grid[i][j] <= 4`

---
```java
// Java 22ms(Beats 55.11%), Time O(M*N*log(M*N)), Space O(M*N)
class Solution {
    public int minCost(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][] cost = new int[m][n];
        int[][] dir = new int[][] {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        for (int i = 0; i < m; i++)
            Arrays.fill(cost[i], 10000);
        
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> (a[0] - b[0])); // cost, i, j
        pq.offer(new int[] {0, 0 ,0});
        cost[0][0] = 0;
        while (!pq.isEmpty())
        {
            int[] curr = pq.poll();
            int c = curr[0], i = curr[1], j = curr[2];
            if (i == m - 1 && j == n - 1)
                return c;
            for (int k = 0; k < 4; k++)
            {
                int x = i + dir[k][0];
                int y = j + dir[k][1];
                if (x < 0 || y < 0 || x >= m || y >= n)
                    continue;
                int nextCost = c + (grid[i][j] == k + 1 ? 0 : 1);
                if (cost[x][y] <= nextCost)
                    continue;
                cost[x][y] = nextCost;
                pq.offer(new int[] {nextCost, x, y});
            }
        }

        return -1;
    }
}
```
---

解法: Dijkstra
如果格子A的符號導向的是格子B，我們就認為AB之間的cost = 0, 否則cost = 1
本題變成求起點到終點的最短路徑問題
因為圖中任兩點之間的邊權重不等，所以用Dijkstra是顯而易見的方案。


###### tags: `Leetcode` `BFS` `Dijkstra (BFS+PQ)`