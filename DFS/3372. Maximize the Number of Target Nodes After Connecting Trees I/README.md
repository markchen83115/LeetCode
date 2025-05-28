# Leetcode - 3372. Maximize the Number of Target Nodes After Connecting Trees I (M+)

[Leetcode](https://leetcode.com/problems/maximize-the-number-of-target-nodes-after-connecting-trees-i/)

There exist two **undirected **trees with `n` and `m` nodes, with **distinct** labels in ranges `[0, n - 1]` and `[0, m - 1]`, respectively.

You are given two 2D integer arrays `edges1` and `edges2` of lengths `n - 1` and `m - 1`, respectively, where `edges1[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the first tree and `edges2[i] = [ui, vi]` indicates that there is an edge between nodes `ui` and `vi` in the second tree. You are also given an integer `k`.

Node `u` is **target** to node `v` if the number of edges on the path from `u` to `v` is less than or equal to `k`. **Note** that a node is _always_ **target** to itself.

Return an array of `n` integers `answer`, where `answer[i]` is the **maximum** possible number of nodes **target** to node `i` of the first tree if you have to connect one node from the first tree to another node in the second tree.

**Note** that queries are independent from each other. That is, for every query you will remove the added edge before proceeding to the next query.

**Example 1:**

> **Input:** edges1 = [[0,1],[0,2],[2,3],[2,4]], edges2 = [[0,1],[0,2],[0,3],[2,7],[1,4],[4,5],[4,6]], k = 2
> 
> **Output:** [9,7,9,8,8]
> 
> **Explanation:**
> 
> -   For `i = 0`, connect node 0 from the first tree to node 0 from the second tree.
> -   For `i = 1`, connect node 1 from the first tree to node 0 from the second tree.
> -   For `i = 2`, connect node 2 from the first tree to node 4 from the second tree.
> -   For `i = 3`, connect node 3 from the first tree to node 4 from the second tree.
> -   For `i = 4`, connect node 4 from the first tree to node 4 from the second tree.
> 
> ![](https://assets.leetcode.com/uploads/2024/09/24/3982-1.png)

**Example 2:**

> **Input:** edges1 = [[0,1],[0,2],[0,3],[0,4]], edges2 = [[0,1],[1,2],[2,3]], k = 1
> 
> **Output:** [6,3,3,3,3]
> 
> **Explanation:**
> 
> For every `i`, connect node `i` of the first tree with any node of the second tree.
> 
> ![](https://assets.leetcode.com/uploads/2024/09/24/3928-2.png)

 **Constraints:**
 
 -   `2 <= n, m <= 1000`
 -   `edges1.length == n - 1`
 -   `edges2.length == m - 1`
 -   `edges1[i].length == edges2[i].length == 2`
 -   `edges1[i] = [ai, bi]`
 -   `0 <= ai, bi < n`
 -   `edges2[i] = [ui, vi]`
 -   `0 <= ui, vi < m`
 -   The input is generated such that `edges1` and `edges2` represent valid trees.
 -   `0 <= k <= 1000`

---
```java
// Java 258ms(Beats 75.95%), Time O(E*E), Space O(E*E)
class Solution {
    public int[] maxTargetNodes(int[][] edges1, int[][] edges2, int k) {
        int n = edges1.length, m = edges2.length;
        int[] target1 = build(edges1, k);
        int[] target2 = build(edges2, k - 1);
        System.out.println(Arrays.toString(target1));
        System.out.println(Arrays.toString(target2));
        int maxTarget2 = 0;
        for (int t2 : target2)
            maxTarget2 = Math.max(maxTarget2, t2);
        
        int[] ret = new int[n + 1];
        for (int i = 0; i <= n; i++)
            ret[i] = target1[i] + maxTarget2;
        
        return ret;
    }

    int[] build(int[][] edges, int k)
    {
        int n = edges.length;
        int[] ret = new int[n + 1];
        List<Integer>[] next = new List[n + 1];
        for (int i = 0; i <= n; i++)
            next[i] = new ArrayList<>();

        for (int[] e : edges)
        {
            int a = e[0], b = e[1];
            next[a].add(b);
            next[b].add(a);
        }

        for (int i = 0; i <= n; i++)
            ret[i] = dfs(i, -1, next, k);
        
        return ret;
    }

    int dfs(int i, int parent, List<Integer>[] next, int dis)
    {
        int ret = 1;
        if (dis < 0)
            return 0;
        for (int nxt : next[i])
        {
            if (nxt == parent)
                continue;
            ret += dfs(nxt, i, next, dis - 1);
        }
        return ret;
    }
}
```
---

題目要計算以第一棵樹`i`爲中心, 並且用一條線將第二棵樹任意一點與第一棵樹i點相連接,
求`距離i <= k`的點最多有幾個

因此可先求出以第二棵樹每個點為中心, `相距 <= k - 1`的點有幾個
則最多`相距 <= k - 1`的點的中心點, 必定會用來與第一棵樹相連接

最後再求得以第一棵樹每個點為中心, `相距 <= k的點有幾個 + 第二棵樹的最大值`, 即為答案


###### tags: `Leetcode` `DFS`