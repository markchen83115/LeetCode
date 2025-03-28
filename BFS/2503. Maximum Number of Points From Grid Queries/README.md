# Leetcode - 2503. Maximum Number of Points From Grid Queries (H-)

[Leetcode](https://leetcode.com/problems/maximum-number-of-points-from-grid-queries/)

You are given an `m x n` integer matrix `grid` and an array `queries` of size `k`.

Find an array `answer` of size `k` such that for each integer `queries[i]` you start in the **top left** cell of the matrix and repeat the following process:

-   If `queries[i]` is **strictly** greater than the value of the current cell that you are in, then you get one point if it is your first time visiting this cell, and you can move to any **adjacent** cell in all `4` directions: up, down, left, and right.
-   Otherwise, you do not get any points, and you end this process.

After the process, `answer[i]` is the **maximum** number of points you can get. **Note** that for each query you are allowed to visit the same cell **multiple** times.

Return _the resulting array_ `answer`.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2025/03/15/image1.png)
> 
> **Input:** grid = [[1,2,3],[2,5,7],[3,5,1]], queries = [5,6,2]
> **Output:** [5,8,1]
> **Explanation:** The diagrams above show which cells we visit to get points for each query.

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2022/10/20/yetgriddrawio-2.png)
> 
> **Input:** grid = [[5,2,1],[1,1,2]], queries = [3]
> **Output:** [0]
> **Explanation:** We can not get any points because the value of the top left cell is already greater than or equal to 3.

**Constraints:**

-   `m == grid.length`
-   `n == grid[i].length`
-   `2 <= m, n <= 1000`
-   `4 <= m * n <= 105`
-   `k == queries.length`
-   `1 <= k <= 104`
-   `1 <= grid[i][j], queries[i] <= 106`

---
```java
// Java 97ms(Beats 64.12%), Time O(MNlogMN), Space O(MN)
class Solution {
    public int[] maxPoints(int[][] grid, int[] queries) {
        int k = queries.length;
        int[][] qs = new int[k][];
        for (int i = 0; i < k; i++)
            qs[i] = new int[] {queries[i], i};
        Arrays.sort(qs, (a, b) -> a[0] - b[0]);

        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);   // val, i, j
        int m = grid.length, n = grid[0].length;
        boolean[][] visited = new boolean[m][n];
        pq.offer(new int[] {grid[0][0], 0, 0});
        visited[0][0] = true;

        int[] ret = new int[k];
        int count = 0;
        int[] dir = new int[] {1, 0, -1, 0, 1};
        for (int p = 0; p < k; p++)
        {
            int query = qs[p][0], index = qs[p][1];
            while (!pq.isEmpty() && pq.peek()[0] < query)
            {
                int[] cur = pq.poll();
                int val = cur[0], i = cur[1], j = cur[2];
                count++;
                for (int d = 0; d < 4; d++)
                {
                    int x = i + dir[d];
                    int y = j + dir[d + 1];
                    if (x < 0 || y < 0 || x >= m || y >= n || visited[x][y])
                        continue;
                    visited[x][y] = true;
                    pq.offer(new int[] {grid[x][y], x, y});
                }
            }

            ret[index] = count;
        }

        return ret;
    }
}
```
---

參考[【每日一题】LeetCode 2503. Maximum Number of Points From Grid Queries - YouTube](https://www.youtube.com/watch?v=G7Gg9w5_KWk)

根據題意，如果query越小，那麼我們能夠擴展的範圍越小
query越大，擴展的範圍是單調地增大的

所以我們必然會將queries排序，優先處理更小的query，例如常規的BFS來遍歷所有小於query的格子；然後再處理更大的query，嘗試重複利用已經探索過的網格區域

此時，我們發現此題很像`778.Swim-in-Rising-Water` ，一開始只能在較矮的水平面游泳。後來水平面提升了，必然可以淹過一些在邊界處相對地勢較低的格子，不斷往外溢出
於是，我們只要在BFS的時候，將隊列裡將格子按照地勢從低到高排列

如果隊列首元素的海拔小於query，那麼它就確認被淹沒了，它的鄰接格子就會成為新的堤岸被加入隊列做下一步的考察
直到隊列的首元素大於等於query，就表示我們BFS的進程中被當前的邊界所圍住，已經佔據了從左上角開始可以聯絡到的所有小於query的位置


###### tags: `Leetcode` `BFS` `Dijkstra (BFS+PQ)`