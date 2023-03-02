# Leetcode - 310. Minimum Height Trees (H-)

[Leetcode](https://leetcode.com/problems/minimum-height-trees/description/)

A tree is an undirected graph in which any two vertices are connected by _exactly_ one path. In other words, any connected graph without simple cycles is a tree.

Given a tree of `n` nodes labelled from `0` to `n - 1`, and an array of `n - 1` `edges` where `edges[i] = [ai, bi]` indicates that there is an undirected edge between the two nodes `ai` and `bi` in the tree, you can choose any node of the tree as the root. When you select a node `x` as the root, the result tree has height `h`. Among all possible rooted trees, those with minimum height (i.e. `min(h)`)  are called **minimum height trees** (MHTs).

Return _a list of all **MHTs'** root labels_. You can return the answer in **any order**.

The **height** of a rooted tree is the number of edges on the longest downward path between the root and a leaf.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/01/e1.jpg)
```
Input: n = 4, edges = [[1,0],[1,2],[1,3]]
Output: [1]
Explanation: As shown, the height of the tree is 1 when the root is the node with label 1 which is the only MHT.
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/01/e2.jpg)
```
Input: n = 6, edges = [[3,0],[3,1],[3,2],[3,4],[5,4]]
Output: [3,4]
```
**Constraints:**

-   `1 <= n <= 2 * 104`
-   `edges.length == n - 1`
-   `0 <= ai, bi < n`
-   `ai != bi`
-   All the pairs `(ai, bi)` are distinct.
-   The given input is **guaranteed** to be a tree and there will be **no repeated** edges.

---

```java
// Java
class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        if (n == 1) return Arrays.asList(0);
        if (n == 2) return Arrays.asList(0, 1);
        int[] inDegree = new int[n];
        ArrayList<Integer>[] next = new ArrayList[n];
        for (int i = 0; i < n; i++)
            next[i] = new ArrayList<Integer>();
        for (int[] edge : edges) {
            int a = edge[0], b = edge[1];
            inDegree[a]++;
            inDegree[b]++;
            next[a].add(b);
            next[b].add(a);
        }

        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            if (inDegree[i] == 1)
                queue.offer(i);
        }

        int count = n;
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int curr = queue.poll();
                count--;
                for (int node : next[curr]) {
                    if (--inDegree[node] == 1) {
                        queue.offer(node);
                    }
                }
            }
            if (count <= 2) break;
        }

        List<Integer> ret = new LinkedList(queue);
        return ret;
    }
}
```

---

[wisdompeak/YouTube講解](https://www.youtube.com/watch?v=2BaulnY_ssM)

像撥洋蔥一樣, 一層一層往內撥開,
從inDegree = 1的node開始, 透過BFS往內縮減


###### tags: `Leetcode` `BFS` `拓撲排序`
