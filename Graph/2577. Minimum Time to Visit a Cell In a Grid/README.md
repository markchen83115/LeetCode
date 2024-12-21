# Leetcode - 2577. Minimum Time to Visit a Cell In a Grid (H-)

[Leetcode](https://leetcode.com/problems/minimum-time-to-visit-a-cell-in-a-grid/)

You are given a `m x n` matrix `grid` consisting of **non-negative** integers where `grid[row][col]` represents the **minimum** time required to be able to visit the cell `(row, col)`, which means you can visit the cell `(row, col)` only when the time you visit it is greater than or equal to `grid[row][col]`.

You are standing in the **top-left** cell of the matrix in the `0th` second, and you must move to **any** adjacent cell in the four directions: up, down, left, and right. Each move you make takes 1 second.

Return _the **minimum** time required in which you can visit the bottom-right cell of the matrix_. If you cannot visit the bottom-right cell, then return `-1`.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2023/02/14/yetgriddrawio-8.png)
> 
> **Input:** grid = [[0,1,3,2],[5,1,2,5],[4,3,8,6]]
> **Output:** 7
> **Explanation:** One of the paths that we can take is the following:
> - at t = 0, we are on the cell (0,0).
> - at t = 1, we move to the cell (0,1). It is possible because grid[0][1] <= 1.
> - at t = 2, we move to the cell (1,1). It is possible because grid[1][1] <= 2.
> - at t = 3, we move to the cell (1,2). It is possible because grid[1][2] <= 3.
> - at t = 4, we move to the cell (1,1). It is possible because grid[1][1] <= 4.
> - at t = 5, we move to the cell (1,2). It is possible because grid[1][2] <= 5.
> - at t = 6, we move to the cell (1,3). It is possible because grid[1][3] <= 6.
> - at t = 7, we move to the cell (2,3). It is possible because grid[2][3] <= 7.
> The final time is 7. It can be shown that it is the minimum time possible.

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2023/02/14/yetgriddrawio-9.png)
> 
> **Input:** grid = [[0,2,4],[3,2,1],[1,0,4]]
> **Output:** -1
> **Explanation:** There is no path from the top left to the bottom-right cell.

**Constraints:**

-   `m == grid.length`
-   `n == grid[i].length`
-   `2 <= m, n <= 1000`
-   `4 <= m * n <= 105`
-   `0 <= grid[i][j] <= 105`
-   `grid[0][0] == 0`

---
```java
// Java 121ms (Beats 72.53%), Time O(M*N), Space O(M*N)
class Solution {
    public int minimumTime(int[][] grid) {
        if (grid[0][1] > 1 && grid[1][0] > 1)
            return -1;
        int m = grid.length, n = grid[0].length;
        boolean[][] visited = new boolean[m][n];
        int[] dir = new int[] {1, 0, -1, 0, 1};
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        pq.offer(new int[] {0, 0, 0}); // step, i, j
        while (!pq.isEmpty())
        {
            int[] curr = pq.poll();
            int step = curr[0], i = curr[1], j = curr[2];
            if (i == m - 1 && j == n - 1)
                return step;
            for (int k = 0; k < 4; k++)
            {
                int x = i + dir[k];
                int y = j + dir[k+1];
                if (x < 0 || y < 0 || x >= m || y >= n || visited[x][y])
                    continue;
                int nextStep = step + 1;
                if (grid[x][y] > nextStep)
                    nextStep = grid[x][y] + (grid[x][y] - nextStep) % 2;
                visited[x][y] = true;
                pq.offer(new int[] {nextStep, x, y});
            }
        }

        return -1;
    }
}
```
---

如果題目沒有限制`you must move to any adjacent cell`, 
則可直接套用dijkstra模板, 直到我們到達右下角

因此如果進入`grid[i][j]`時間還未到, 我們可以前後跳, 使每次時間都+2
故若需等待才能進入`grid[i][j]`的時間需要計算`grid[x][y] + (grid[x][y] - nextStep) % 2`
考量以下兩種情況:
1. 目前為1(到grid[i][j]為2) 下一個grid[i][j]為3
2. 目前為1(到grid[i][j]為2) 下一個grid[i][j]為4



###### tags: `Leetcode` `Graph` `Dijkstra`