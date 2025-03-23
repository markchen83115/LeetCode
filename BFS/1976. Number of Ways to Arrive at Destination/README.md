# Leetcode - 1976. Number of Ways to Arrive at Destination (M+)

[Leetcode](https://leetcode.com/problems/number-of-ways-to-arrive-at-destination/)

You are in a city that consists of `n` intersections numbered from `0` to `n - 1` with **bi-directional** roads between some intersections. The inputs are generated such that you can reach any intersection from any other intersection and that there is at most one road between any two intersections.

You are given an integer `n` and a 2D integer array `roads` where `roads[i] = [ui, vi, timei]` means that there is a road between intersections `ui` and `vi` that takes `timei` minutes to travel. You want to know in how many ways you can travel from intersection `0` to intersection `n - 1` in the **shortest amount of time**.

Return _the **number of ways** you can arrive at your destination in the **shortest amount of time**_. Since the answer may be large, return it **modulo** `109 + 7`.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2025/02/14/1976_corrected.png)
> 
> **Input:** n = 7, roads = [[0,6,7],[0,1,2],[1,2,3],[1,3,3],[6,3,3],[3,5,1],[6,5,1],[2,5,1],[0,4,5],[4,6,2]]
> **Output:** 4
> **Explanation:** The shortest amount of time it takes to go from intersection 0 to intersection 6 is 7 minutes.
> The four ways to get there in 7 minutes are:
> - 0 ➝ 6
> - 0 ➝ 4 ➝ 6
> - 0 ➝ 1 ➝ 2 ➝ 5 ➝ 6
> - 0 ➝ 1 ➝ 3 ➝ 5 ➝ 6

**Example 2:**

> **Input:** n = 2, roads = [[1,0,10]]
> **Output:** 1
> **Explanation:** There is only one way to go from intersection 0 to intersection 1, and it takes 10 minutes.

**Constraints:**

-   `1 <= n <= 200`
-   `n - 1 <= roads.length <= n * (n - 1) / 2`
-   `roads[i].length == 3`
-   `0 <= ui, vi <= n - 1`
-   `1 <= timei <= 109`
-   `ui != vi`
-   There is at most one road connecting any two intersections.
-   You can reach any intersection from any other intersection.

---
```java
// Java 20ms(Beats 8.90%), Time O(ElogE), Space O(E)
class Solution {
    long[] dist = new long[201];
    List<int[]>[] next = new ArrayList[201];
    int mod = (int) 1e9 + 7;
    int[] count = new int[201];
    public int countPaths(int n, int[][] roads) {
        for (int i = 0; i < n; i++)
        {
            dist[i] = -1;
            next[i] = new ArrayList<>();
        }

        for (int[] road : roads)
        {
            int u = road[0], v = road[1], t = road[2];
            next[u].add(new int[] {v, t});
            next[v].add(new int[] {u, t});
        }

        PriorityQueue<long[]> pq = new PriorityQueue<>((a, b) -> Long.compare(a[0], b[0]));
        pq.offer(new long[]{0, 0});

        while (!pq.isEmpty())
        {
            long[] cur = pq.poll();
            long d = cur[0], node = cur[1];
            int c = (int) node;
            if (dist[c] != -1)
                continue;
            dist[c] = d;
            for (int[] nxt : next[c])
            {
                if (dist[nxt[0]] != -1)
                    continue;
                pq.offer(new long[] {d + nxt[1], nxt[0]});
            }
        }

        for (int i = 0; i < n; i++)
            count[i] = -1;
        return dfs(n - 1, dist[n - 1]);
    }

    int dfs(int cur, long d)
    {
        if (dist[cur] != d)
            return 0;
        if (cur == 0)
            return 1;
        if (count[cur] != -1)
            return count[cur];

        int ret = 0;
        for (int[] nxt : next[cur])
        {
            ret += dfs(nxt[0], d - nxt[1]);
            ret %= mod;
        }
        count[cur] = ret;
        return ret;
    }
}
```
```java
// Java 10ms(Beats 93.52%), Time O(ElogE), Space O(E)
class Solution {
    public int countPaths(int n, int[][] roads) {
        long[] dist = new long[201];
        List<int[]>[] next = new ArrayList[201];
        int mod = (int) 1e9 + 7;
        int[] ways = new int[201];

        for (int i = 0; i < n; i++)
        {
            dist[i] = Long.MAX_VALUE;
            next[i] = new ArrayList<>();
        }

        for (int[] road : roads)
        {
            int u = road[0], v = road[1], t = road[2];
            next[u].add(new int[] {v, t});
            next[v].add(new int[] {u, t});
        }

        PriorityQueue<long[]> pq = new PriorityQueue<>((a, b) -> Long.compare(a[0], b[0]));
        pq.offer(new long[]{0, 0});
        dist[0] = 0;
        ways[0] = 1;

        while (!pq.isEmpty())
        {
            long[] cur = pq.poll();
            long d = cur[0], node = cur[1];
            int c = (int) node;

            if (dist[c] < d) continue;

            for (int[] nxt : next[c])
            {
                int nxtNode = nxt[0], time = nxt[1];
                if (dist[c] + time < dist[nxtNode])
                {
                    dist[nxtNode] = dist[c] + time;
                    ways[nxtNode] = ways[c];
                    pq.offer(new long[] {dist[nxtNode], nxtNode});
                }
                else if (dist[c] + time == dist[nxtNode])
                {
                    ways[nxtNode] = (ways[nxtNode] + ways[c]) % mod;
                }
            }
        }

        return ways[n - 1];
    }
}
```
---

參考[【每日一题】LeetCode 1976. Number of Ways to Arrive at Destination - YouTube](https://www.youtube.com/watch?v=pbxO6LaQQjw)

如果是求起點到終點的最短時間，那麼可以採用標準的Dijsktra演算法。但本題更進一步，需要求最短距離的方案數，有什麼變化呢

比較直覺的想法就是倒推。假設從起點到終點的最短時間是T，而終點的鄰接城市是Ai（距離是ti），那麼意味著我們需要在T-ti的時間到達Ai
同時我們注意到Dijsktra演算法可以求得起點到任意位置的最短時間T[Ai]，那麼我們就可以發現：

如果`T > T[Ai] + ti`，這是不可能的，否則我們就找到了一條裡終點更短的路徑

如果`T < T[Ai] + t[i]`，那麼路徑Ai>也不會是最優路徑上的起點，因為會耗時的起點，那麼。所以我們知道最優路徑的倒數第二站，應該是那些滿足`T = T[Ai]+ti`的位置

所以我們就可以遞歸得到
`count(Destination) = sum { count(Ai) } , for T[Ai] + ti = T[Destination]`

由此我們可以遞歸下去。邊界條件是count(Start) = 0


###### tags: `Leetcode` `BFS` `Dijkstra (BFS+PQ)`