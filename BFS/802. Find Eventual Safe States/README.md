# Leetcode - 802. Find Eventual Safe States (H-)

[Leetcode](https://leetcode.com/problems/find-eventual-safe-states/)

There is a directed graph of `n` nodes with each node labeled from `0` to `n - 1`. The graph is represented by a **0-indexed** 2D integer array `graph` where `graph[i]` is an integer array of nodes adjacent to node `i`, meaning there is an edge from node `i` to each node in `graph[i]`.

A node is a **terminal node** if there are no outgoing edges. A node is a **safe node** if every possible path starting from that node leads to a **terminal node** (or another safe node).

Return _an array containing all the **safe nodes** of the graph_. The answer should be sorted in **ascending** order.

**Example 1:**

> ![Illustration of graph](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/03/17/picture1.png)
> 
> **Input:** graph = [[1,2],[2,3],[5],[0],[5],[],[]]
> **Output:** [2,4,5,6]
> **Explanation:** The given graph is shown above.
> Nodes 5 and 6 are terminal nodes as there are no outgoing edges from either of them.
> Every path starting at nodes 2, 4, 5, and 6 all lead to either node 5 or 6.

**Example 2:**

> **Input:** graph = [[1,2,3,4],[1,2],[3,4],[0,4],[]]
> **Output:** [4]
> **Explanation:**
> Only node 4 is a terminal node, and every path starting at node 4 leads to node 4.

**Constraints:**

-   `n == graph.length`
-   `1 <= n <= 104`
-   `0 <= graph[i].length <= n`
-   `0 <= graph[i][j] <= n - 1`
-   `graph[i]` is sorted in a strictly increasing order.
-   The graph may contain self-loops.
-   The number of edges in the graph will be in the range `[1, 4 * 104]`.

---
```java
// Java BFS 35ms(Beats 10.27%), Time O(ElogE), Space O(E)
class Solution {
    public List<Integer> eventualSafeNodes(int[][] graph) {
        List<Integer> ret = new ArrayList<>();
        int n = graph.length;
        HashMap<Integer, List<Integer>> next = new HashMap<>();
        int[] outDegree = new int[n];
        for (int i = 0; i < n; i++)
            for (int j : graph[i])
            {
                next.computeIfAbsent(j, v -> new ArrayList<>()).add(i);
                outDegree[i]++;
            }
        
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < n; i++)
            if (outDegree[i] == 0)
            {
                queue.offer(i);
                ret.add(i);
            }
        
        while (!queue.isEmpty())
        {
            int curr = queue.poll();
            if (!next.containsKey(curr))
                continue;
            for (int nxt : next.get(curr))
                if (--outDegree[nxt] == 0)
                {
                    queue.offer(nxt);
                    ret.add(nxt);
                }
        }

        Collections.sort(ret);
        return ret;
    }
}
```
```java
// Java DFS 4ms(Beats 99.02%), Time O(E), Space O(E)
class Solution {
    public List<Integer> eventualSafeNodes(int[][] graph) {
        List<Integer> ret = new ArrayList<>();
        int n = graph.length;
        int[] visited = new int[n];
        for (int i = 0; i < n; i++)
            if (dfs(graph, i, visited))
                ret.add(i);
        
        return ret;
    }

    boolean dfs(int[][] graph, int node, int[] visited)
    {
        if (visited[node] == 1) return true;
        if (visited[node] == 2) return false;

        visited[node] = 2;
        for (int next : graph[node])
            if (dfs(graph, next, visited) == false)
                return false;
        
        visited[node] = 1;
        return true;
    }
}
```
---

這是一道經典的涉及有向圖是否存在環的問題。 DFS和BFS都有經典的解法。

#### DFS

[ ](https://github.com/wisdompeak/LeetCode/tree/master/BFS/802.Find-Eventual-Safe-States#dfs)

用DFS來判定是否有環，可以參考 207.Course-Schedule

基本概念是，將每個節點的visited標記為三種狀態
第一次遍歷到節點i標記2, 如果從節點i後續的DFS都沒有偵測到環, 成功回溯到節點i時, 改標記為1
因此在遍歷的過程中, 遇到了已經標記為1的點, 則說明之後肯定`safe`, 不用再走下去
如果遇到了已經標記為2的點, 則表示該DFS的路線遇到了環

在本題中, 對任意節點`i`, 如果`DFS(i)`判定無環, 則可以放入答案中

#### BFS

[ ](https://github.com/wisdompeak/LeetCode/tree/master/BFS/802.Find-Eventual-Safe-States#bfs)

拓樸排序的應用
最容易判定safe的節點, 是那些`outDegree = 0`的節點
將這些點剪除之後, 接下來`outDegree = 0`的節點, 肯定還是`safe`的節點
以此`BFS`不斷推進, 如果還有剩下的節點, 那麼他們肯定`outDegree != 0`, 也就是互相成環的, 可以終止程式


###### tags: `Leetcode` `BFS` `拓撲排序`