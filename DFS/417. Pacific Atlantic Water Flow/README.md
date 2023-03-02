# Leetcode - 417. Pacific Atlantic Water Flow (M)

[Leetcode](https://leetcode.com/problems/pacific-atlantic-water-flow/description/)

There is an `m x n` rectangular island that borders both the **Pacific Ocean** and **Atlantic Ocean**. The **Pacific Ocean** touches the island's left and top edges, and the **Atlantic Ocean** touches the island's right and bottom edges.

The island is partitioned into a grid of square cells. You are given an `m x n` integer matrix `heights` where `heights[r][c]` represents the **height above sea level** of the cell at coordinate `(r, c)`.

The island receives a lot of rain, and the rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is **less than or equal to** the current cell's height. Water can flow from any cell adjacent to an ocean into the ocean.

Return _a **2D list** of grid coordinates _`result`_ where _`result[i] = [ri, ci]`_ denotes that rain water can flow from cell _`(ri, ci)`_ to **both** the Pacific and Atlantic oceans_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/08/waterflow-grid.jpg)
```
Input: heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]
Output: [[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]
Explanation: The following cells can flow to the Pacific and Atlantic oceans, as shown below:
[0,4]: [0,4] -> Pacific Ocean 
       [0,4] -> Atlantic Ocean
[1,3]: [1,3] -> [0,3] -> Pacific Ocean 
       [1,3] -> [1,4] -> Atlantic Ocean
[1,4]: [1,4] -> [1,3] -> [0,3] -> Pacific Ocean 
       [1,4] -> Atlantic Ocean
[2,2]: [2,2] -> [1,2] -> [0,2] -> Pacific Ocean 
       [2,2] -> [2,3] -> [2,4] -> Atlantic Ocean
[3,0]: [3,0] -> Pacific Ocean 
       [3,0] -> [4,0] -> Atlantic Ocean
[3,1]: [3,1] -> [3,0] -> Pacific Ocean 
       [3,1] -> [4,1] -> Atlantic Ocean
[4,0]: [4,0] -> Pacific Ocean 
       [4,0] -> Atlantic Ocean
Note that there are other possible paths for these cells to flow to the Pacific and Atlantic oceans.
```
**Example 2:**
```
Input: heights = [[1]]
Output: [[0,0]]
Explanation: The water can flow from the only cell to the Pacific and Atlantic oceans.
```
**Constraints:**

-   `m == heights.length`
-   `n == heights[r].length`
-   `1 <= m, n <= 200`
-   `0 <= heights[r][c] <= 105`

---

```java
// Java DFS 5ms
class Solution {
    public List<List<Integer>> pacificAtlantic(int[][] heights) {
        int m = heights.length;
        int n = heights[0].length;
        boolean[][] pacific = new boolean[m][n];
        boolean[][] atlantic = new boolean[m][n];
        List<List<Integer>> ret = new ArrayList<>();
        for (int i = 0; i < m; i++) {
            dfs(heights, pacific,  i, 0,   heights[i][0]);
            dfs(heights, atlantic, i, n-1, heights[i][n-1]);
        }
        for (int j = 0; j < n; j++) {
            dfs(heights, pacific,  0,   j, heights[0][j]);
            dfs(heights, atlantic, m-1, j, heights[m-1][j]);
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (pacific[i][j] && atlantic[i][j])
                    ret.add(Arrays.asList(i, j));
            }
        }
        return ret;
    }

    public void dfs(int[][] heights, boolean[][] visited, int i, int j, int height) {
        int m = heights.length;
        int n = heights[0].length;
        int[] dir = {0, 1, 0, -1, 0};
        if (i < 0 || j < 0 || i >= m || j >= n) return;
        if (visited[i][j] || heights[i][j] < height) return;
        visited[i][j] = true;
        for (int k = 0; k < dir.length - 1; k++) {
            dfs(heights, visited, i + dir[k], j + dir[k+1], heights[i][j]);
        }
    
    }
}
```

```java
// Java BFS 10ms
class Solution {
    public List<List<Integer>> pacificAtlantic(int[][] heights) {
        int m = heights.length;
        int n = heights[0].length;
        boolean[][] pacific = new boolean[m][n];
        boolean[][] atlantic = new boolean[m][n];
        List<List<Integer>> ret = new ArrayList<>();
        Queue<Integer> pacQueue = new LinkedList<>();
        Queue<Integer> atlQueue = new LinkedList<>();
        for (int i = 0; i < m; i++) {
            pacQueue.offer(i * n);
            atlQueue.offer(i * n + n-1);
        }
        for (int j = 0; j < n; j++) {
            pacQueue.offer(j);
            atlQueue.offer((m-1) * n + j);
        }
        bfs(heights, pacific,  pacQueue);
        bfs(heights, atlantic, atlQueue);
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (pacific[i][j] && atlantic[i][j])
                    ret.add(Arrays.asList(i, j));
            }
        }
        return ret;
    }

    public void bfs(int[][] heights, boolean[][] visited, Queue<Integer> queue) {
        int m = heights.length;
        int n = heights[0].length;
        int[] dir = {0, 1, 0, -1, 0};

        while (!queue.isEmpty()) {
            int index = queue.poll();
            int x = index / n;
            int y = index % n;
            visited[x][y] = true;
            for (int k = 0; k < dir.length - 1; k++) {
                int i = x + dir[k];
                int j = y + dir[k+1];
                if (i >= 0 && j >= 0 && i < m && j < n
                    && !visited[i][j] && heights[i][j] >= heights[x][y]) {
                    queue.offer(i * n + j);
                }
            }
        }
    }
}
```

---

用DFS/BFS從海洋往陸地走, 若高度相同或越高代表雨水可流至海洋

###### tags: `Leetcode` `DFS` `BFS`
