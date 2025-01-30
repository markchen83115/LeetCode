# Leetcode - 2493. Divide Nodes Into the Maximum Number of Groups (H-)

[Leetcode](https://leetcode.com/problems/divide-nodes-into-the-maximum-number-of-groups/)

You are given a positive integer `n` representing the number of nodes in an **undirected** graph. The nodes are labeled from `1` to `n`.

You are also given a 2D integer array `edges`, where `edges[i] = [ai, bi]` indicates that there is a **bidirectional** edge between nodes `ai` and `bi`. **Notice** that the given graph may be disconnected.

Divide the nodes of the graph into `m` groups (**1-indexed**) such that:

-   Each node in the graph belongs to exactly one group.
-   For every pair of nodes in the graph that are connected by an edge `[ai, bi]`, if `ai` belongs to the group with index `x`, and `bi` belongs to the group with index `y`, then `|y - x| = 1`.

Return _the maximum number of groups (i.e., maximum _`m`_) into which you can divide the nodes_. Return `-1` _if it is impossible to group the nodes with the given conditions_.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2022/10/13/example1.png)
> 
> **Input:** n = 6, edges = [[1,2],[1,4],[1,5],[2,6],[2,3],[4,6]]
> **Output:** 4
> **Explanation:** As shown in the image we:
> - Add node 5 to the first group.
> - Add node 1 to the second group.
> - Add nodes 2 and 4 to the third group.
> - Add nodes 3 and 6 to the fourth group.
> We can see that every edge is satisfied.
> It can be shown that that if we create a fifth group and move any node from the third or fourth group to it, at least on of the edges will not be satisfied.

**Example 2:**

> **Input:** n = 3, edges = [[1,2],[2,3],[3,1]]
> **Output:** -1
> **Explanation:** If we add node 1 to the first group, node 2 to the second group, and node 3 to the third group to satisfy the first two edges, we can see that the third edge will not be satisfied.
> It can be shown that no grouping is possible.

**Constraints:**

-   `1 <= n <= 500`
-   `1 <= edges.length <= 104`
-   `edges[i].length == 2`
-   `1 <= ai, bi <= n`
-   `ai != bi`
-   There is at most one edge between any pair of vertices.

---
```java
// Java 262ms(Beats 76.98%), Time O(N*E), Space O(N)
class Solution {
    List<Integer>[] next = new List[505];
    int[] result = new int[505];
    public int magnificentSets(int n, int[][] edges) {
        for (int i = 0; i < 505; i++)
            next[i] = new ArrayList<>();

        for (int[] e : edges)
        {
            next[e[0]].add(e[1]);
            next[e[1]].add(e[0]);
        }

        for (int i = 1; i <= n; i++)
        {
            Queue<int[]> queue = new LinkedList<>();
            queue.offer(new int[] {i, 1}); // node, depth
            int maxDepth = 0;
            int minNode = Integer.MAX_VALUE;
            int[] level = new int[505];
            level[i] = 1;

            while (!queue.isEmpty())
            {
                int[] curr = queue.poll();
                int node = curr[0];
                int depth = curr[1];
                maxDepth = Math.max(maxDepth, depth);
                minNode = Math.min(minNode, node);
                for (int nxt : next[node])
                {
                    if (level[nxt] == depth)
                        return -1;

                    if (level[nxt] == 0)
                    {
                        level[nxt] = depth + 1;
                        queue.offer(new int[] {nxt, depth + 1});
                    }
                }
            }
            
            result[minNode] = Math.max(result[minNode], maxDepth);
        }

        int ret = 0;
        for (int i = 1; i <= n; i++)
            ret += result[i];
        
        return ret;
    }
}
```
---

參考[【每日一题】LeetCode 2493. Divide Nodes Into the Maximum Number of Groups - YouTube](https://youtu.be/oa26sFeHRNM)

本題的突破點是，只要我們能確定一個節點作為第一個group，那麼剩下的節點該如何安排其實是可以貪心地確定的：很顯然只要用BFS進行層級遍歷即可，就像生成一棵樹一樣，把同屬於一個層級的放入一個group，不停往下擴展，這樣就可以得到最多的層級（也就是group）。

因為本題要求同一個group不能有邊，所以我們需要檢查一下這種方法得到的拓撲結構：是否有任何點指向了同一層級的其他點。有的話就標記BFS。特別注意的是，如果發現了這種情況，不僅意味著從當前根節點出發的BFS無解，也意味著整個連通圖無解，即你從此連通圖的任何一個位置作為根，都無法得到合法的層級結構。

因此，遍歷起點的循環是V次，每次BFS需要至多訪問E條邊。總的時間複雜度是o(VE)剛好符合要求。

有人會問，以上的方法約定了第一個group只能有一個節點（看做是根）。可不可能有兩個節點A與B都是第一個group的最優解呢？答案是不會比較優。當A與B（第一層級）都和C（第二層級）聯通時，我們其實按照之前的方法，會把B看做是第三個層級，顯然這個方案能夠得到更多的group。

此外，本題可能會有多個不同的聯通區域。最終答案是每個聯通區域所能建構出的最大group數量總和。一種處理方法是先用Union Find把不同聯通區域的節點都標記出來，接著再遍歷每個聯通區域，變換根的位置去做BFS的嘗試。另一種處理方法可以直接遍歷每個節點作為根，BFS完之後記得將遇到的最小編號的節點作為聯通區域的代號，最後我們將不同聯通區域的答案再相加。



###### tags: `Leetcode` `BFS`