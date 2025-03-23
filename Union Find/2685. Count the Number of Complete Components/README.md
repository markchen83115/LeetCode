# Leetcode - 2685. Count the Number of Complete Components (M+)

[Leetcode](https://leetcode.com/problems/count-the-number-of-complete-components/)

You are given an integer `n`. There is an **undirected** graph with `n` vertices, numbered from `0` to `n - 1`. You are given a 2D integer array `edges` where `edges[i] = [ai, bi]` denotes that there exists an **undirected** edge connecting vertices `ai` and `bi`.

Return _the number of **complete connected components** of the graph_.

A **connected component** is a subgraph of a graph in which there exists a path between any two vertices, and no vertex of the subgraph shares an edge with a vertex outside of the subgraph.

A connected component is said to be **complete** if there exists an edge between every pair of its vertices.

**Example 1:**

> **![](https://assets.leetcode.com/uploads/2023/04/11/screenshot-from-2023-04-11-23-31-23.png)**
> 
> **Input:** n = 6, edges = [[0,1],[0,2],[1,2],[3,4]]
> **Output:** 3
> **Explanation:** From the picture above, one can see that all of the components of this graph are complete.

**Example 2:**

> **![](https://assets.leetcode.com/uploads/2023/04/11/screenshot-from-2023-04-11-23-32-00.png)**
> 
> **Input:** n = 6, edges = [[0,1],[0,2],[1,2],[3,4],[3,5]]
> **Output:** 1
> **Explanation:** The component containing vertices 0, 1, and 2 is complete since there is an edge between every pair of two vertices. On the other hand, the component containing vertices 3, 4, and 5 is not complete since there is no edge between vertices 4 and 5. Thus, the number of complete components in this graph is 1.

**Constraints:**

-   `1 <= n <= 50`
-   `0 <= edges.length <= n * (n - 1) / 2`
-   `edges[i].length == 2`
-   `0 <= ai, bi <= n - 1`
-   `ai != bi`
-   There are no repeated edges.

---
```java
// Java 5ms(Beats 95.47%), Time O(N), Space O(N)
class Solution {
    int[] father;
    public int countCompleteComponents(int n, int[][] edges) {
        father = new int[n];
        int[] groupEdgeCount = new int[n];
        int[] groupNodeCount = new int[n];

        int ret = 0;
        for (int i = 0; i < n; i++)
            father[i] = i;
        for (int[] edge : edges)
        {
            int x = edge[0], y = edge[1];
            if (findFather(x) != findFather(y))
                union(x, y);
        }

        for (int i = 0; i < n; i++)
            groupNodeCount[findFather(i)]++;

        for (int[] edge : edges)
            groupEdgeCount[father[edge[0]]]++;

        for (int i = 0; i < n; i++)
        {
            if (father[i] == i && groupNodeCount[father[i]] != 0)
            {
                if ((groupNodeCount[i] * (groupNodeCount[i] - 1) / 2) == groupEdgeCount[i])
                    ret++;
            }
            
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
---

利用Union Find進行分組
分好組之後, 利用`father[i] == i` 檢查是否組裡面符合`Node數 * (Node數 - 1) / 2 == Edges數`


###### tags: `Leetcode` `Union Find`