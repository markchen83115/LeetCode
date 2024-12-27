# Leetcode - 1584. Min Cost to Connect All Points (H-)

[Leetcode](https://leetcode.com/problems/min-cost-to-connect-all-points/)

You are given an array `points` representing integer coordinates of some points on a 2D-plane, where `points[i] = [xi, yi]`.

The cost of connecting two points `[xi, yi]` and `[xj, yj]` is the **manhattan distance** between them: `|xi - xj| + |yi - yj|`, where `|val|` denotes the absolute value of `val`.

Return _the minimum cost to make all points connected._ All points are connected if there is **exactly one** simple path between any two points.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2020/08/26/d.png)
> 
> **Input:** points = [[0,0],[2,2],[3,10],[5,2],[7,0]]
> **Output:** 20
> **Explanation:** 
> ![](https://assets.leetcode.com/uploads/2020/08/26/c.png)
> We can connect the points as shown above to get the minimum cost of 20.
> Notice that there is a unique path between every pair of points.

**Example 2:**

> **Input:** points = [[3,12],[-2,5],[-4,1]]
> **Output:** 18

**Constraints:**

-   `1 <= points.length <= 1000`
-   `-106 <= xi, yi <= 106`
-   All pairs `(xi, yi)` are distinct.

---
```java
// Java Kruskal 329ms(Beats 44.68%), Time O(N^2logN), Space O(N^2)
class Solution {
    int[] father = new int[1000];
    public int minCostConnectPoints(int[][] points) {
        int ret = 0;
        int n = points.length;
        for (int i = 0; i < n; i++)
            father[i] = i;

        List<int[]> edges = new ArrayList<>(); // dist, i, j
        for (int i = 0; i < n; i++)
            for (int j = i + 1; j < n; j++)
            {
                int dist = Math.abs(points[i][0] - points[j][0]) + Math.abs(points[i][1] - points[j][1]);
                edges.add( new int[] {dist, i, j});
            }
        
        Collections.sort(edges, (a, b) -> a[0] - b[0]);

        int count = 0;
        for (int[] edge : edges)
        {
            int dist = edge[0];
            int a = edge[1];
            int b = edge[2];
            if (findFather(a) != findFather(b))
            {
                union(a, b);
                count++;
                ret += dist;
            }

            if (count == n - 1) // 已找到 n-1 條邊
                break;
        }

        return ret;
    }

    int findFather(int x)
    {
        if (father[x] != x)
            father[x] = findFather(father[x]);
        return father[x];
    }

    void union(int x, int y)
    {
        x = father[x];
        y = father[y];
        if (x < y)
            father[y] = x;
        else
            father[x] = y;
    }
}
```
```java
// Java Prim 235ms(Beats 49.88%), Time O(N^2logN), Space O(N^2)
class Solution {
    public int minCostConnectPoints(int[][] points) {
        int ret = 0;
        int n = points.length;
        if (n == 1) // points = [[0,0]]
            return 0;
        boolean[] visited = new boolean[1000];
        HashMap<Integer, List<int[]>> edgeMap = new HashMap<>(); // i : dist, j
        for (int i = 0; i < n; i++)
            for (int j = i + 1; j < n; j++)
            {
                int dist = Math.abs(points[i][0] - points[j][0]) + Math.abs(points[i][1] - points[j][1]);
                edgeMap.computeIfAbsent(i, v -> new ArrayList<>()).add(new int[] {dist, j});
                edgeMap.computeIfAbsent(j, v -> new ArrayList<>()).add(new int[] {dist, i});
            }
        
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);

        int count = 0;
        for (int[] edge : edgeMap.get(0))
            pq.offer(edge);
        visited[0] = true;

        while (!pq.isEmpty())
        {
            int[] edge = pq.poll();
            int dist = edge[0];
            int next = edge[1];
            if (visited[next])
                continue;

            visited[next] = true;
            count++;
            ret += dist;
            if (count == n - 1) // 已找到 n-1 條邊
                break;

            for (int[] e : edgeMap.get(next))
                pq.offer(e);
        }

        return ret;
    }
}
```
---

參考[【每日一题】1584. Min Cost to Connect All Points, 9/13/2020 - YouTube](https://youtu.be/ZVh2WTcE8EY)

#### 解法1：Kruskal

[ ](https://github.com/wisdompeak/LeetCode/tree/master/Union_Find/1584.Min-Cost-to-Connect-All-Points#%E8%A7%A3%E6%B3%951kruskal)

本題依然是MST的基本題，但是時間要求非常嚴格。
對於Krusal演算法而言，需要將所有的邊進行排序。時間複雜度ElogE。
本題中，任兩點之間都可能有路徑，所以複雜度是N^2logN，對於不開o2優化的C++而言極有可能TLE。

降低TLE的方法有兩個：

1. 使用priority_queue而不是直接排序整個邊長數組。原因是pq不會將所有裝入其中的元素馬上排序（建堆過程是o(E)），而是只有訪問頂堆元素的時候才將最小值推出來。因為我們只要找到N條邊，數量級比N^2的總邊數要少的多，所以可以省去不需要的排序過程。
2. 避免使用vector（時空非常坑爹），改用定長數組array<int,N>這個資料結構。

#### 解法2：Prim

[ ](https://github.com/wisdompeak/LeetCode/tree/master/Union_Find/1584.Min-Cost-to-Connect-All-Points#%E8%A7%A3%E6%B3%952prim)

Prim演算法通常的複雜度也是ElogV，本題就是o(N^2logN)。

Prim的基本想法是，MST從節點0開始生長。 
MST裡面只有節點0時，從所有節點0出發的邊裡面挑一個最短的，這時MST裡就有兩個點。
第二步是從這兩點所出發的所有邊裡面挑一個最短的，如果對應的第三點是已經訪問過的，那就換下一個最短邊，直到能確定第三個點。
依序繼續，直至確定N個點，則MST建置完成。



###### tags: `Leetcode` `Union Find` `MST`