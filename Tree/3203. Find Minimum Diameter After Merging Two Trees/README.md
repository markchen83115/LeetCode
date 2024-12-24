# Leetcode - 3203. Find Minimum Diameter After Merging Two Trees (H-)

[Leetcode](https://leetcode.com/problems/find-minimum-diameter-after-merging-two-trees/)

There exist two **undirected **trees with `n` and `m` nodes, numbered from `0` to `n - 1` and from `0` to `m - 1`, respectively. You are given two 2D integer arrays `edges1` and `edges2` of lengths `n - 1` and `m - 1`, respectively, where `edges1[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the first tree and `edges2[i] = [ui, vi]` indicates that there is an edge between nodes `ui` and `vi` in the second tree.

You must connect one node from the first tree with another node from the second tree with an edge.

Return the **minimum possible diameter** of the resulting tree.

The **diameter** of a tree is the length of the _longest_ path between any two nodes in the tree.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2024/04/22/example11-transformed.png)
> 
> **Input:** edges1 = [[0,1],[0,2],[0,3]], edges2 = [[0,1]]
> 
> **Output:** 3
> 
> **Explanation:**
> 
> We can obtain a tree of diameter 3 by connecting node 0 from the first tree with any node from the second tree.

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2024/04/22/example211.png)
> 
> **Input:** edges1 = [[0,1],[0,2],[0,3],[2,4],[2,5],[3,6],[2,7]], edges2 = [[0,1],[0,2],[0,3],[2,4],[2,5],[3,6],[2,7]]
> 
> **Output:** 5
> 
> **Explanation:**
> 
> We can obtain a tree of diameter 5 by connecting node 0 from the first tree with node 0 from the second tree.

**Constraints:**

-   `1 <= n, m <= 105`
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
// Java 125ms(Beats 55.12%), Time O(N), Space O(N)
class Solution {
    public int minimumDiameterAfterMerge(int[][] edges1, int[][] edges2) {
        int dis1 = BFSFindDiameter(edges1, edges1.length + 1);
        int dis2 = BFSFindDiameter(edges2, edges2.length + 1);
        int disCombine = (dis1+1) / 2 + (dis2+1) / 2 + 1;
        int ret = Math.max(dis1, Math.max(dis2, disCombine));

        return ret;
    }

    int BFSFindDiameter(int[][] edges, int n)
    {
        ArrayList<Integer>[] next = new ArrayList[n];
        for (int i = 0; i < n; i++)
            next[i] = new ArrayList<Integer>();
        for (int[] e : edges)
        {
            next[e[0]].add(e[1]);
            next[e[1]].add(e[0]);
        }

        Queue<Integer> queue = new LinkedList<>();
        // find farest node
        int node = -1;
        queue.offer(0);
        boolean[] visited = new boolean[n];
        visited[0] = true;
        
        while (!queue.isEmpty())
        {
            int size = queue.size();
            for (int i = 0; i < size; i++)
            {
                node = queue.poll();
                for (int nxt : next[node])
                    if (!visited[nxt])
                    {
                        queue.offer(nxt);
                        visited[nxt] = true;
                    }
            }
        }

        // calculate diameter
        int dist = -1;
        queue.offer(node);
        visited = new boolean[n];
        visited[node] = true;

        while (!queue.isEmpty())
        {
            int size = queue.size();
            dist++;
            for (int i = 0; i < size; i++)
            {
                node = queue.poll();
                for (int nxt : next[node])
                    if (!visited[nxt])
                    {
                        queue.offer(nxt);
                        visited[nxt] = true;
                    }
            }
        }

        return dist;
    }
}
```
---

關於樹的最大路徑(直徑), 已經有`1245.Tree-Diameter`的做法
`直徑`: 從樹中隨機一點找到離他最遠端點, 再從此最遠端點出發, 再走到最遠點的距離即為直徑

這題要求連接後的樹的直徑要最小, 那必定使用各自樹的直徑中點來連接
因此這題的一個解為`ceil(d1/2) + ceil(d2/2) + 1`

必須注意, 聯通樹的直徑不一定是直徑
若樹1遠大於樹2, 則樹1本身直徑才是直徑
因此答案為`Max(ceil(d1/2) + ceil(d2/2) + 1, d1, d2)`


###### tags: `Leetcode` `Tree` `Path in a tree`