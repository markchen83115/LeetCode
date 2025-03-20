# Leetcode - 3108. Minimum Cost Walk in Weighted Graph (H-)

[Leetcode](https://leetcode.com/problems/minimum-cost-walk-in-weighted-graph/)

There is an undirected weighted graph with `n` vertices labeled from `0` to `n - 1`.

You are given the integer `n` and an array `edges`, where `edges[i] = [ui, vi, wi]` indicates that there is an edge between vertices `ui` and `vi` with a weight of `wi`.

A walk on a graph is a sequence of vertices and edges. The walk starts and ends with a vertex, and each edge connects the vertex that comes before it and the vertex that comes after it. It's important to note that a walk may visit the same edge or vertex more than once.

The **cost** of a walk starting at node `u` and ending at node `v` is defined as the bitwise `AND` of the weights of the edges traversed during the walk. In other words, if the sequence of edge weights encountered during the walk is `w0, w1, w2, ..., wk`, then the cost is calculated as `w0 & w1 & w2 & ... & wk`, where `&` denotes the bitwise `AND` operator.

You are also given a 2D array `query`, where `query[i] = [si, ti]`. For each query, you need to find the minimum cost of the walk starting at vertex `si` and ending at vertex `ti`. If there exists no such walk, the answer is `-1`.

Return _the array _`answer`_, where _`answer[i]`_ denotes the **minimum** cost of a walk for query `i`.

**Example 1:**

> **Input:** n = 5, edges = [[0,1,7],[1,3,7],[1,2,1]], query = [[0,3],[3,4]]
> 
> **Output:** [1,-1]
> 
> **Explanation:**
> 
> ![](https://assets.leetcode.com/uploads/2024/01/31/q4_example1-1.png)
> 
> To achieve the cost of 1 in the first query, we need to move on the following edges: `0->1` (weight 7), `1->2` (weight 1), `2->1` (weight 1), `1->3` (weight 7).
> 
> In the second query, there is no walk between nodes 3 and 4, so the answer is -1.

**Example 2:**

> **Input:** n = 3, edges = [[0,2,7],[0,1,15],[1,2,6],[1,2,1]], query = [[1,2]]
> 
> **Output:** [0]
> 
> **Explanation:**
> 
> ![](https://assets.leetcode.com/uploads/2024/01/31/q4_example2e.png)
> 
> To achieve the cost of 0 in the first query, we need to move on the following edges: `1->2` (weight 1), `2->1` (weight 6), `1->2` (weight 1).

**Constraints:**

-   `2 <= n <= 105`
-   `0 <= edges.length <= 105`
-   `edges[i].length == 3`
-   `0 <= ui, vi <= n - 1`
-   `ui != vi`
-   `0 <= wi <= 105`
-   `1 <= query.length <= 105`
-   `query[i].length == 2`
-   `0 <= si, ti <= n - 1`
-   `si != ti`

---
```java
// Java 7ms(Beats 80.59%), Time O(N), Space O(N)
class Solution {
    int[] father;
    int[] minCost;
    public int[] minimumCost(int n, int[][] edges, int[][] query) {
        father = new int[n];
        minCost = new int[n];
        for (int i = 0; i < n; i++)
        {
            father[i] = i;
            minCost[i] = -1;    // binary of -1 = 11111111111111111111111111111111 (32位)
        }

        for (int[] edge : edges)
        {
            int x = edge[0], y = edge[1], cost = edge[2];
            if (findFather(x) == findFather(y))
                minCost[findFather(x)] &= cost;
            else
                union(x, y, cost);
        }

        int[] ret = new int[query.length];
        for (int i = 0; i < query.length; i++)
        {
            int x = query[i][0], y = query[i][1];
            if (x == y)
                ret[i] = 0;
            else if (findFather(x) != findFather(y))
                ret[i] = -1;
            else
                ret[i] = minCost[findFather(x)];
        }

        return ret;
    }

    int findFather(int x)
    {
        if (father[x] != x)
            father[x] = findFather(father[x]);
        return father[x];
    }

    void union(int x, int y, int cost)
    {
        x = father[x];
        y = father[y];
        if (x < y)
            father[y] = x;
        else
            father[x] = y;
        
        minCost[x] &= minCost[y] & cost;
        minCost[y] = minCost[x];
    }
}
```
---

核心概念：AND的操作，得到的結果只會相等或是變小，所以只要AND的值愈多，就愈有機會得到最小值

如果要讓從`a`走到`b`的cost最小，把所有`a`到`b`的可能路徑全都走一遍就好
因為得到的AND值一定是最小

而從`graph`的角度來看，其實就是把所有`node`根據是否聯通來分組：
1. 有聯通的`node`，任兩點的`min cost`都相等，都是把同組的所有path weight AND 在一起的值
2. 沒有聯通的`node`，`cost`是`-1`

分組這件事自然會想到用`union find`，而`weight`的部分可以在`union`的時候一起處理

另外如果面試問到要怎麼做到均攤複雜度`O(1)`，要能夠講的出來下面這兩個技巧：
1. Path compression
2. Union by rank


###### tags: `Leetcode` `Union Find`