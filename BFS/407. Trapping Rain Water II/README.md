# Leetcode - 407. Trapping Rain Water II (H)

[Leetcode](https://leetcode.com/problems/trapping-rain-water-ii/)

Given an `m x n` integer matrix `heightMap` representing the height of each unit cell in a 2D elevation map, return _the volume of water it can trap after raining_.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2021/04/08/trap1-3d.jpg)
> 
> **Input:** heightMap = [[1,4,3,1,3,2],[3,2,1,3,2,4],[2,3,3,2,3,1]]
> **Output:** 4
> **Explanation:** After the rain, water is trapped between the blocks.
> We have two small ponds 1 and 3 units trapped.
> The total volume of water trapped is 4.

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2021/04/08/trap2-3d.jpg)
> 
> **Input:** heightMap = [[3,3,3,3,3],[3,2,2,2,3],[3,2,1,2,3],[3,2,2,2,3],[3,3,3,3,3]]
> **Output:** 10

**Constraints:**

-   `m == heightMap.length`
-   `n == heightMap[i].length`
-   `1 <= m, n <= 200`
-   `0 <= heightMap[i][j] <= 2 * 104`

---
```java
// Java 27ms(Beats 23.28%), Time O(M*N*log(M*N)), Space O(M*N)
class Solution {
    public int trapRainWater(int[][] heightMap) {
        int m = heightMap.length, n = heightMap[0].length;
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        boolean[][] visited = new boolean[m][n];

        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
                if (i == 0 || j == 0 || i == m - 1 || j == n - 1)
                {
                    pq.offer(new int[] {heightMap[i][j], i, j});
                    visited[i][j] = true;
                }
        
        int ret = 0;
        int seaHeight = 0;
        int[] dir = new int[] {1, 0, -1, 0 ,1};
        while (!pq.isEmpty())
        {
            int[] curr = pq.poll();
            int h = curr[0];
            int i = curr[1];
            int j = curr[2];
            if (h > seaHeight)
                seaHeight = h;
            ret += seaHeight - heightMap[i][j];

            for (int k = 0; k < 4; k++)
            {
                int x = i + dir[k];
                int y = j + dir[k+1];
                if (x < 0 || y < 0 || x >= m || y >= n)
                    continue;
                if (visited[x][y])
                    continue;
                visited[x][y] = true;
                pq.offer(new int[] {heightMap[x][y], x, y});
            }
        }

        return ret;
    }
}
```
---

參考[【每日一题】407. Trapping Rain Water II , 2/17/2021 - YouTube](https://youtu.be/uupOnJJxPbI)

注意此題和一維的版本不一樣，無法使用DP的想法。

我們將矩陣所有的邊界格子加入優先隊列，裡面的格子依照海拔從低到高排列。這些邊界格子相當於閉合的堤岸，圍了中間的區域。

我們想像一下海平面逐漸上漲的過程，當海平面cur上漲到優先隊列裡最矮的那個元素時，就相當於從那裡衝破了堤岸，拓展進了內陸、同時會有新的堤岸格子加入隊列。如果加入隊列的這一系列新格子高度都小於cur，那麼它們其實也被淹沒了，洪水可以基於它們的位置繼續向四周擴展。這個「氾濫」的過程直到隊列裡面的所有元素高度（也就是首元素）都大於cur。在上面的過程中，所有被「淹沒」的格子(i,j)，計算`cur-h(i,j)`就是該點的儲水量。

然後重複上述的過程，再次想像海平面cur上漲到隊列最矮元素的高度，這樣就再一次“決堤”，這個氾濫的過程依然是在各地“儲水”的機會，直至隊列裡面的堤岸都高於海平面cur位置，「氾濫」結束。

以上過程中，所有被「決堤」或被「淹沒」的格子，都要做標記，不用重複入隊列。直至隊列為空停止。


###### tags: `Leetcode` `BFS` `Dijkstra (BFS+PQ)`