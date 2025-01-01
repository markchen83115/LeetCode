# Leetcode - 743. Network Delay Time (H-)

[Leetcode](https://leetcode.com/problems/network-delay-time/)

You are given a network of `n` nodes, labeled from `1` to `n`. You are also given `times`, a list of travel times as directed edges `times[i] = (ui, vi, wi)`, where `ui` is the source node, `vi` is the target node, and `wi` is the time it takes for a signal to travel from source to target.

We will send a signal from a given node `k`. Return _the **minimum** time it takes for all the_ `n` _nodes to receive the signal_. If it is impossible for all the `n` nodes to receive the signal, return `-1`.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2019/05/23/931_example_1.png)
> 
> **Input:** times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2
> **Output:** 2

**Example 2:**

> **Input:** times = [[1,2,1]], n = 2, k = 1
> **Output:** 1

**Example 3:**

> **Input:** times = [[1,2,1]], n = 2, k = 2
> **Output:** -1

**Constraints:**

-   `1 <= k <= n <= 100`
-   `1 <= times.length <= 6000`
-   `times[i].length == 3`
-   `1 <= ui, vi <= n`
-   `ui != vi`
-   `0 <= wi <= 100`
-   All the pairs `(ui, vi)` are **unique**. (i.e., no multiple edges.)

---
```java
// Java dijkstra 11ms(Beats 73.37%), Time O(ElogE), Space O(E)
class Solution {
    public int networkDelayTime(int[][] times, int n, int k) {
        int[] dist = new int[n+1];
        HashMap<Integer, List<int[]>> nextMap = new HashMap<>();
        for (int i = 1; i <= n; i++)
        {
            nextMap.put(i, new ArrayList<int[]>());
            dist[i] = Integer.MAX_VALUE / 2;
        }
        for (int[] t : times)
        {
            int i = t[0], j = t[1], d = t[2];
            nextMap.get(i).add(new int[] {j, d});  // j, time
        }

        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]); // time, i
        pq.offer(new int[] {0, k});
        dist[k] = 0;

        while (!pq.isEmpty())
        {
            int[] curr = pq.poll();
            int t = curr[0], node = curr[1];
            for (int[] nxt : nextMap.get(node))
            {
                int j = nxt[0];
                int nextTime = t + nxt[1];
                if (dist[j] <= nextTime)
                    continue;
                dist[j] = nextTime;
                pq.offer(new int[] {nextTime, nxt[0]});
            }
        }

        int ret = 0;
        for (int i = 1; i <= n; i++)
            ret = Math.max(ret, dist[i]);
        
        if (ret == Integer.MAX_VALUE / 2)
            return -1;
        
        return ret;

    }
}
```
```java
// Java Floyd 11ms(Beats 73.37%), Time O(N^3), Space O(N^2)
class Solution {
    public int networkDelayTime(int[][] times, int n, int k) {
        int[][] dp = new int[n+1][n+1];
        //dp[i][j] = the shortest dist between i and j
        for (int i = 1; i <= n; i++)
            Arrays.fill(dp[i], Integer.MAX_VALUE / 2);
        for (int[] t : times)
            dp[t[0]][t[1]] = t[2];
        for (int i = 1; i <= n; i++)
            dp[i][i] = 0;

        for (int p = 1; p <= n; p++)
        {
            // update all dp[i][j]
            for (int i = 1; i <= n; i++)
                for (int j = 1; j <= n; j++)
                    dp[i][j] = Math.min(dp[i][j], dp[i][p] + dp[p][j]);
        }

        int ret = 0;
        for (int i = 1; i <= n; i++)
            ret = Math.max(ret, dp[k][i]);
        
        return ret == Integer.MAX_VALUE / 2 ? -1 : ret;
            
    }
}
```
---

#### Dijkstra (BFS+PQ)
`Dijkstra`的本質就是將`BFS`的傳統隊列替換為優先隊列，採用貪心的策略
每次從優先隊列中彈出當前離起點最近的點`curr`，然後將它所有鄰接的點以`{dist, next}`的形式加入隊列

注意, 用`dist[]`來記錄每個點目前最小到達時間
每次取出一個點更新`dist[]`, 並看他是否能對相鄰的點有更近的`dist`, 也就是`ArrivalTime + time < dist[nextNode]`
是的話, 將新狀態`(ArrivalTime + time, nextNode)`加入隊列
直到隊列為空, 最後查看`dist[]`的最大值即為本題的解

#### Floyd
求兩點之間的最短路徑，典型的圖論中的基本演算法
Floyd是首選，因為程式碼短，容易理解，而且對於邊權的值沒有正數的限制
本質就是遍歷所有的節點k看是否能對`dp[i][j]`的更新做出貢獻
即`dp[i][j] = min(dp[i][j], dp[i][k]+dp[k][j])`


###### tags: `Leetcode` `BFS` `Dijkstra (BFS+PQ)`