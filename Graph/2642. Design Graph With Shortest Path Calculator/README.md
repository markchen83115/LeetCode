# Leetcode - 2642. Design Graph With Shortest Path Calculator (M+)

[Leetcode](https://leetcode.com/problems/design-graph-with-shortest-path-calculator/description/)

There is a **directed weighted** graph that consists of `n` nodes numbered from `0` to `n - 1`. The edges of the graph are initially represented by the given array `edges` where `edges[i] = [fromi, toi, edgeCosti]` meaning that there is an edge from `fromi` to `toi` with the cost `edgeCosti`.

Implement the `Graph` class:

-   `Graph(int n, int[][] edges)` initializes the object with `n` nodes and the given edges.
-   `addEdge(int[] edge)` adds an edge to the list of edges where `edge = [from, to, edgeCost]`. It is guaranteed that there is no edge between the two nodes before adding this one.
-   `int shortestPath(int node1, int node2)` returns the **minimum** cost of a path from `node1` to `node2`. If no path exists, return `-1`. The cost of a path is the sum of the costs of the edges in the path.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/01/11/graph3drawio-2.png)
```
Input
["Graph", "shortestPath", "shortestPath", "addEdge", "shortestPath"]
[[4, [[0, 2, 5], [0, 1, 2], [1, 2, 1], [3, 0, 3]]], [3, 2], [0, 3], [[1, 3, 4]], [0, 3]]
Output
[null, 6, -1, null, 6]

Explanation
Graph g = new Graph(4, [[0, 2, 5], [0, 1, 2], [1, 2, 1], [3, 0, 3]]);
g.shortestPath(3, 2); // return 6. The shortest path from 3 to 2 in the first diagram above is 3 -> 0 -> 1 -> 2 with a total cost of 3 + 2 + 1 = 6.
g.shortestPath(0, 3); // return -1. There is no path from 0 to 3.
g.addEdge([1, 3, 4]); // We add an edge from node 1 to node 3, and we get the second diagram above.
g.shortestPath(0, 3); // return 6. The shortest path from 0 to 3 now is 0 -> 1 -> 3 with a total cost of 2 + 4 = 6.
```
**Constraints:**

-   `1 <= n <= 100`
-   `0 <= edges.length <= n * (n - 1)`
-   `edges[i].length == edge.length == 3`
-   `0 <= fromi, toi, from, to, node1, node2 <= n - 1`
-   `1 <= edgeCosti, edgeCost <= 106`
-   There are no repeated edges and no self-loops in the graph at any point.
-   At most `100` calls will be made for `addEdge`.
-   At most `100` calls will be made for `shortestPath`.

---

```java
// Java Dijkstra 121ms(Beats 71.89%), Time O(Elog(V)) (Dijkstra), Space O(E + V)
class Graph {
    HashMap<Integer, List<int[]>> next;
    public Graph(int n, int[][] edges) {
        next = new HashMap<>();
        for (int[] edge : edges) {
            next.computeIfAbsent(edge[0], x -> new ArrayList<int[]>()).add(new int[] {edge[1], edge[2]});
        }
    }
    
    public void addEdge(int[] edge) {
        next.computeIfAbsent(edge[0], x -> new ArrayList<int[]>()).add(new int[] {edge[1], edge[2]});
    }
    
    public int shortestPath(int node1, int node2) {
        int[] dist = new int[100];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[node1] = 0;

        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        pq.offer(new int[] {0, node1});
        while (!pq.isEmpty()) {
            int[] cur = pq.poll();
            int cost = cur[0], node = cur[1];
            if (node == node2)
                return cost;
            if (!next.containsKey(node))
                continue;
            for (int[] nxt : next.get(node)) {
                int newDist = cost + nxt[1];
                if (newDist < dist[nxt[0]]) {
                    dist[nxt[0]] = newDist;
                    pq.offer(new int[] {newDist, nxt[0]});
                }
            }
        }
        return -1;
    }
}
```

```java
// Java Floyd 74ms(Beats 99.83%), Time O(N^3), space O(N^2)
class Graph {
    int[][] dp = new int[100][100];
    int m = Integer.MAX_VALUE / 3;
    int n;
    public Graph(int n, int[][] edges) {
        this.n = n;
        // initial distance
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                dp[i][j] = m;

        // self
        for (int i = 0; i < n; i++)
            dp[i][i] = 0;
        
        // cost
        for (int[] e : edges)
            dp[e[0]][e[1]] = e[2];

        // floyd
        for (int k = 0; k < n; k++) {
            for (int i = 0; i < n; i++)
                for (int j = 0; j < n; j++)
                    dp[i][j] = Math.min(dp[i][j], dp[i][k] + dp[k][j]);
        }
    }
    
    public void addEdge(int[] edge) {
        int a = edge[0], b = edge[1], cost = edge[2];
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                dp[i][j] = Math.min(dp[i][j], dp[i][a] + cost + dp[b][j]);
    }
    
    public int shortestPath(int node1, int node2) {
        return dp[node1][node2] == m ? -1 : dp[node1][node2];
    }
}

```

---

`Dijkstra`求固定起始點到每個點的最短距離
`Floyd`求圖內每個點到點的最短距離
Floyd適用於沒有`負環`的圖

[wisdompeak/YouTube](https://www.youtube.com/watch?v=r1OquW3gAJw&list=PLwdV8xC1EWHrtgsYCcDTXIMVaHSlsnLzL&index=2)

###### tags: `Leetcode` `Graph`