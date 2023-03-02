# Leetcode - 994. Rotting Oranges (M)

[Leetcode](https://leetcode.com/problems/rotting-oranges/description/)

You are given an `m x n` `grid` where each cell can have one of three values:

-   `0` representing an empty cell,
-   `1` representing a fresh orange, or
-   `2` representing a rotten orange.

Every minute, any fresh orange that is **4-directionally adjacent** to a rotten orange becomes rotten.

Return _the minimum number of minutes that must elapse until no cell has a fresh orange_. If _this is impossible, return_ `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/02/16/oranges.png)

Input: grid = [[2,1,1],[1,1,0],[0,1,1]]
Output: 4

**Example 2:**

Input: grid = [[2,1,1],[0,1,1],[1,0,1]]
Output: -1
Explanation: The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.

**Example 3:**

Input: grid = [[0,2]]
Output: 0
Explanation: Since there are already no fresh oranges at minute 0, the answer is just 0.

**Constraints:**

-   `m == grid.length`
-   `n == grid[i].length`
-   `1 <= m, n <= 10`
-   `grid[i][j]` is `0`, `1`, or `2`.

---

```java
// Java DFS
class Solution {
    public int orangesRotting(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 2) {
                    // time=2 represent time 0, in order to avoid [0 1 2]
                    dfs(grid, i, j, 2);	
                }
            }
        }
        int maxTime = 2;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1)
                    return -1;
                maxTime = Math.max(maxTime, grid[i][j]);
            }
        }
        return maxTime - 2;
    }

    public void dfs(int[][] grid, int i, int j, int time) {
        int m = grid.length, n = grid[0].length;
        if (i < 0 || j < 0 || i >= m || j >= n || grid[i][j] == 0 
            || (1 < grid[i][j] && grid[i][j] < time))
            return;
        grid[i][j] = time;
        dfs(grid, i + 1, j, time + 1);
        dfs(grid, i - 1, j, time + 1);
        dfs(grid, i, j + 1, time + 1);
        dfs(grid, i, j - 1, time + 1);
    }
}
```

```java
// Java BFS
class Solution {
    public int orangesRotting(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        Queue<Integer> queue = new LinkedList<>();
        int totalFreshOrange = 0, maxTime = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 2)
                    queue.offer(i * n + j);
                if (grid[i][j] == 1)
                    totalFreshOrange++;
            }
        }
        if (totalFreshOrange == 0)
            return 0;
        int[] dir = {0, 1, 0, -1, 0};
        while (!queue.isEmpty()) {
            int size = queue.size();
            maxTime++;
            for (int k = 0; k < size; k++) {
                int index = queue.poll();
                for (int d = 0; d < dir.length - 1; d++) {
                    int row = index / n + dir[d];
                    int col = index % n + dir[d + 1];
                    if (row >= 0 && row < m && col >= 0 && col < n && grid[row][col] == 1) {
                        grid[row][col] = 2;
                        queue.offer(row * n + col);
                        totalFreshOrange--;
                    }
                }
            }
        }
        
        return totalFreshOrange == 0 ? maxTime - 1 : -1;
    }
}
```

---

> DFS

當找到`orange = 2(rotten)`時, 由此`grid[i][j]`進行DFS
每往外一格`time + 1`, 將時間更新至`grid[i][j]`
若`grid[i][j] > 1`代表已被其他orange rotten, 需要與現在時間比較做更新
最後尋找grid內最大的時間, 若找到1則回傳-1

> BFS

當找到`orange = 2(rotten)`時放進queue, 並計算fresh orange數量
每一次跑完當前的queue.size時, 代表時間需要+1
最後檢查是否已沒有fresh orange


###### tags: `Leetcode` `BFS` `DFS`
