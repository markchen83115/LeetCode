# Leetcode - 787. Cheapest Flights Within K Stops (H)

[Leetcode](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

There are `n` cities connected by some number of flights. You are given an array `flights` where `flights[i] = [fromi, toi, pricei]` indicates that there is a flight from city `fromi` to city `toi` with cost `pricei`.

You are also given three integers `src`, `dst`, and `k`, return _**the cheapest price** from _`src`_ to _`dst`_ with at most _`k`_ stops. _If there is no such route, return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/03/18/cheapest-flights-within-k-stops-3drawio.png)
```
Input: n = 4, flights = [[0,1,100],[1,2,100],[2,0,100],[1,3,600],[2,3,200]], src = 0, dst = 3, k = 1
Output: 700
Explanation:
The graph is shown above.
The optimal path with at most 1 stop from city 0 to 3 is marked in red and has cost 100 + 600 = 700.
Note that the path through cities [0,1,2,3] is cheaper but is invalid because it uses 2 stops.
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2022/03/18/cheapest-flights-within-k-stops-1drawio.png)
```
Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 1
Output: 200
Explanation:
The graph is shown above.
The optimal path with at most 1 stop from city 0 to 2 is marked in red and has cost 100 + 100 = 200.
```
**Example 3:**

![](https://assets.leetcode.com/uploads/2022/03/18/cheapest-flights-within-k-stops-2drawio.png)
```
Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 0
Output: 500
Explanation:
The graph is shown above.
The optimal path with no stops from city 0 to 2 is marked in red and has cost 500.
```
**Constraints:**

-   `1 <= n <= 100`
-   `0 <= flights.length <= (n * (n - 1) / 2)`
-   `flights[i].length == 3`
-   `0 <= fromi, toi < n`
-   `fromi != toi`
-   `1 <= pricei <= 104`
-   There will not be any multiple flights between two cities.
-   `0 <= src, dst, k < n`
-   `src != dst`

---

```java
// Java Dijkastra 6ms
class Solution {
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {
        HashMap<Integer, List<int[]>> next = new HashMap<>();
        for (int[] flight : flights) {
            int a = flight[0], b = flight[1], c = flight[2];
            next.computeIfAbsent(a, value -> new ArrayList<int[]>()).add(new int[] {b, c});
        }

        int[] stops = new int[n];
        Arrays.fill(stops, Integer.MAX_VALUE);

        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]); // cost, city, stops
        pq.offer(new int[] {0, src, 0});

        while (!pq.isEmpty()) {
            int[] cur = pq.poll();
            int c = cur[0], city = cur[1], stop = cur[2];
            if (city == dst) return c;
            if (stop > stops[city] || stop > k) continue;
            stops[city] = stop;
            if (!next.containsKey(city)) continue;
            for (int[] nxt : next.get(city))
                pq.offer(new int[] {c + nxt[1], nxt[0], stop + 1});
        }

        return -1;
    }
}
```

```java
// Java DP 5ms
class Solution {
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {
        int[][] dp = new int[k+2][n];
        for (int i = 0; i < k+2; i++)
            Arrays.fill(dp[i], Integer.MAX_VALUE/2);
        dp[0][src] = 0;

        int ret = Integer.MAX_VALUE/2;
        for (int i = 1; i <= k+1; i++) {
            for (int[] flight : flights) {
                int a = flight[0], b = flight[1], p = flight[2];
                dp[i][b] = Math.min(dp[i][b], dp[i-1][a] + p);

                if (b == dst)
                    ret = Math.min(ret, dp[i][b]);
            }
        }

        return ret == Integer.MAX_VALUE/2 ? -1 : ret;
    }
}
```

```java
// Java Bellman Ford 4ms
class Solution {
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {
        int[] dp = new int[n];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[src] = 0;

        int ret = Integer.MAX_VALUE;
        for (int i = 0; i <= k; i++) {
            int[] temp = Arrays.copyOf(dp, n);
            for (int[] flight : flights) {
                int a = flight[0], b = flight[1], p = flight[2];
                if (dp[a] < Integer.MAX_VALUE)
                    temp[b] = Math.min(temp[b], dp[a] + p);
            }
            dp = temp;
        }

        return dp[dst] == Integer.MAX_VALUE ? -1 : dp[dst];
    }
}
```

---
[wisdompeak/YouTube解說](https://www.youtube.com/watch?v=Q8oMHlThySQ)
通常都可以用Dijkastra來處理, 模板為BSF+PQ


###### tags: `Leetcode` `BFS` `Dijkstra (BFS+PQ)`
