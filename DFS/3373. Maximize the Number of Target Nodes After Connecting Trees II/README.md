# Leetcode - 3373. Maximize the Number of Target Nodes After Connecting Trees II (H-)

[Leetcode](https://leetcode.com/problems/maximize-the-number-of-target-nodes-after-connecting-trees-ii/)

There exist two **undirected **trees with `n` and `m` nodes, labeled from `[0, n - 1]` and `[0, m - 1]`, respectively.

You are given two 2D integer arrays `edges1` and `edges2` of lengths `n - 1` and `m - 1`, respectively, where `edges1[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the first tree and `edges2[i] = [ui, vi]` indicates that there is an edge between nodes `ui` and `vi` in the second tree.

Node `u` is **target** to node `v` if the number of edges on the path from `u` to `v` is even. **Note** that a node is _always_ **target** to itself.

Return an array of `n` integers `answer`, where `answer[i]` is the **maximum** possible number of nodes that are **target** to node `i` of the first tree if you had to connect one node from the first tree to another node in the second tree.

**Note** that queries are independent from each other. That is, for every query you will remove the added edge before proceeding to the next query.

**Example 1:**

> **Input:** edges1 = [[0,1],[0,2],[2,3],[2,4]], edges2 = [[0,1],[0,2],[0,3],[2,7],[1,4],[4,5],[4,6]]
> 
> **Output:** [8,7,7,8,8]
> 
> **Explanation:**
> 
> -   For `i = 0`, connect node 0 from the first tree to node 0 from the second tree.
> -   For `i = 1`, connect node 1 from the first tree to node 4 from the second tree.
> -   For `i = 2`, connect node 2 from the first tree to node 7 from the second tree.
> -   For `i = 3`, connect node 3 from the first tree to node 0 from the second tree.
> -   For `i = 4`, connect node 4 from the first tree to node 4 from the second tree.
> 
> ![](https://assets.leetcode.com/uploads/2024/09/24/3982-1.png)

**Example 2:**

> **Input:** edges1 = [[0,1],[0,2],[0,3],[0,4]], edges2 = [[0,1],[1,2],[2,3]]
> 
> **Output:** [3,6,6,6,6]
> 
> **Explanation:**
> 
> For every `i`, connect node `i` of the first tree with any node of the second tree.
> 
> ![](https://assets.leetcode.com/uploads/2024/09/24/3928-2.png)

**Constraints:**

-   `2 <= n, m <= 105`
-   `edges1.length == n - 1`
-   `edges2.length == m - 1`
-   `edges1[i].length == edges2[i].length == 2`
-   `edges1[i] = [ai, bi]`
-   `0 <= ai, bi < n`
-   `edges2[i] = [ui, vi]`
-   `0 <= ui, vi < m`
-   The input is generated such that `edges1` and `edges2` represent valid trees.

---
```java
// Java 83ms(Beats 88.14%), Time O(E), Space O(E)
class Solution {
    public int[] maxTargetNodes(int[][] edges1, int[][] edges2) {
        int n = edges1.length, m = edges2.length;
        int[] ret = new int[n + 1];
        int[] color1 = new int[n + 1];
        int[] color2 = new int[m + 1];
        int[] count1 = build(edges1, color1);
        int[] count2 = build(edges2, color2);
        for (int i = 0; i <= n; i++)
            ret[i] = count1[color1[i]] + Math.max(count2[0], count2[1]);
        
        return ret;
    }

    int[] build(int[][] edges, int[] color)  // {num of white node, num of black node}
    {
        int n = edges.length;
        List<Integer>[] next = new ArrayList[n + 1];
        for (int i = 0; i <= n; i++)
            next[i] = new ArrayList<>();

        for (int[] e : edges)
        {
            int a = e[0], b = e[1];
            next[a].add(b);
            next[b].add(a);
        }

        int cntWhite = dfs(0, -1, 0, next, color);
        return new int[] {cntWhite, n + 1 - cntWhite};

    }

    int dfs(int node, int parent, int dist, List<Integer>[] next, int[] color)
    {
        color[node] = dist & 1;
        int ret = 1 - color[node];
        for (int nxt : next[node])
        {
            if (nxt == parent)
                continue;
            ret += dfs(nxt, node, dist + 1, next, color);
        }
        return ret;
    }
}
```
---

將樹的node以白與黑相間漆上顏色, 因此可得到第二棵樹有多少白點與黑點
第一棵樹同理, 但是需要在DFS過程中, 記錄每個node的顏色

最後答案為 :
若node為`白點 = 第一棵樹的白點 + 第二棵樹Max(白點, 黑點)`
若node為`黑點 = 第一棵樹的黑點 + 第二棵樹Max(白點, 黑點)`


###### tags: `Leetcode` `DFS`