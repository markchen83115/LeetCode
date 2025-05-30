# Leetcode - 2359. Find Closest Node to Given Two Nodes (M)

[Leetcode](https://leetcode.com/problems/find-closest-node-to-given-two-nodes/)

You are given a **directed** graph of `n` nodes numbered from `0` to `n - 1`, where each node has **at most one** outgoing edge.

The graph is represented with a given **0-indexed** array `edges` of size `n`, indicating that there is a directed edge from node `i` to node `edges[i]`. If there is no outgoing edge from `i`, then `edges[i] == -1`.

You are also given two integers `node1` and `node2`.

Return _the **index** of the node that can be reached from both _`node1`_ and _`node2`_, such that the **maximum** between the distance from _`node1`_ to that node, and from _`node2`_ to that node is **minimized**_. If there are multiple answers, return the node with the **smallest** index, and if no possible answer exists, return `-1`.

Note that `edges` may contain cycles.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2022/06/07/graph4drawio-2.png)
> 
> **Input:** edges = [2,2,3,-1], node1 = 0, node2 = 1
> **Output:** 2
> **Explanation:** The distance from node 0 to node 2 is 1, and the distance from node 1 to node 2 is 1.
> The maximum of those two distances is 1. It can be proven that we cannot get a node with a smaller maximum distance than 1, so we return node 2.

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2022/06/07/graph4drawio-4.png)
> 
> **Input:** edges = [1,2,-1], node1 = 0, node2 = 2
> **Output:** 2
> **Explanation:** The distance from node 0 to node 2 is 2, and the distance from node 2 to itself is 0.
> The maximum of those two distances is 2. It can be proven that we cannot get a node with a smaller maximum distance than 2, so we return node 2.

**Constraints:**

-   `n == edges.length`
-   `2 <= n <= 105`
-   `-1 <= edges[i] < n`
-   `edges[i] != i`
-   `0 <= node1, node2 < n`

---
```java
// Java 10ms(Beats 98.94%), Time O(N), Space O(N)
class Solution {
    public int closestMeetingNode(int[] edges, int node1, int node2) {
        int n = edges.length;
        int[] dist1 = getDist(edges, node1);
        int[] dist2 = getDist(edges, node2);

        int ret = -1;
        int minDist = n + 1;
        for (int i = 0; i < n; i++)
        {
            if (dist1[i] < 0 || dist2[i] < 0)
                continue;
            if (minDist > Math.max(dist1[i], dist2[i]))
            {
                minDist = Math.max(dist1[i], dist2[i]);
                ret = i;
            }
        }

        return ret;
    }

    int[] getDist(int[] edges, int node)
    {
        int n = edges.length;
        int[] dist = new int[n];
        Arrays.fill(dist, -1);
        dist[node] = 0;

        while (node >= 0)
        {
            int next = edges[node];
            if (next == -1 || dist[next] != -1)
                break;
            dist[next] = dist[node] + 1;
            node = next;
        }

        return dist;
    }
}
```
---

本題就是直觀的模擬。
我們只要遍歷所有的節點，看看它到node1的距離與它到node2的距離即可。

那這兩個距離怎麼求呢？
我們事實上只要從node1開始無腦走一遍，就可以得到任意節點i到node1的距離dist1[i]，同理也可以得到任意節點到node2的距離dist2[i]。

我們把這兩個距離數組都保存下來，再遍歷所有的i，求出最小的`max{dist1[i], dist2[i]}`即可。


###### tags: `Leetcode` `Others`