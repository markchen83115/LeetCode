# Leetcode - 778. Swim in Rising Water (H)

[Leetcode](https://leetcode.com/problems/swim-in-rising-water/)

You are given an `n x n` integer matrix `grid` where each value `grid[i][j]` represents the elevation at that point `(i, j)`.

The rain starts to fall. At time `t`, the depth of the water everywhere is `t`. You can swim from a square to another 4-directionally adjacent square if and only if the elevation of both squares individually are at most `t`. You can swim infinite distances in zero time. Of course, you must stay within the boundaries of the grid during your swim.

Return _the least time until you can reach the bottom right square _`(n - 1, n - 1)`_ if you start at the top left square _`(0, 0)`.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2021/06/29/swim1-grid.jpg)
> 
> **Input:** grid = mod[mod[0,2mod],mod[1,3mod]mod]
> **Output:** 3
> Explanation:
> At time 0, you are in grid location (0, 0).
> You cannot go anywhere else because 4-directionally adjacent neighbors have a higher elevation than t = 0.
> You cannot reach point (1, 1) until time 3.
> When the depth of water is 3, we can swim anywhere inside the grid.

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2021/06/29/swim2-grid-1.jpg)
> 
> **Input:** grid = mod[mod[0,1,2,3,4mod],mod[24,23,22,21,5mod],mod[12,13,14,15,16mod],mod[11,17,18,19,20mod],mod[10,9,8,7,6mod]mod]
> **Output:** 16
> **Explanation:** The final route is shown.
> We need to wait until time 16 so that (0, 0) and (4, 4) are connected.

**Constraints:**

-   `n == grid.length`
-   `n == grid[i].length`
-   `1 <= n <= 50`
-   `0 <= grid[i][j] < n2`
-   Each value `grid[i][j]` is **unique**.

---
```java
// Java 10ms(Beats 71.86%), Time O(M*N), Space O(M*N)
class Solution {
    public int swimInWater(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[] dir = new int[] {1, 0, -1, 0 ,1};
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        pq.offer(new int[] {grid[0][0], 0, 0});

        int[][] time = new int[m][n];
        for (int i = 0; i < m; i++)
            Arrays.fill(time[i], Integer.MAX_VALUE / 2);
        time[0][0] = grid[0][0];

        while (!pq.isEmpty())
        {
            int[] curr = pq.poll();
            int t = curr[0], i = curr[1], j = curr[2];
            if (i == m-1 && j == n-1)
                return t;
            for (int k = 0; k < 4; k++)
            {
                int x = i + dir[k];
                int y = j + dir[k+1];
                if (x < 0 || y < 0 || x >= m || y >= n)
                    continue;
                int nextTime = Math.max(t, grid[x][y]);
                if (time[x][y] <= nextTime)
                    continue;
                time[x][y] = nextTime;
                pq.offer(new int[] {nextTime, x, y});
            }
        }

        return -1;
    }
}
```
---

用`Dijkstra`一步一步往終點前進

因為題目說`You can swim infinite distances in zero time`, 因此到達四周點的時間為`nextTime = Math.max(t, grid[x][y])`

另外紀錄`grid[x][y]`的最早到達時間, 只有`nextTime < grid[x][y]`時
我們才會將`grid[x][y]`放入優先隊列中, 並記得更新最早到達時間


###### tags: `Leetcode` `BFS` `Dijkstra (BFS+PQ)`