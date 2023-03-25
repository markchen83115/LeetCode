# Leetcode - 2581. Count Number of Possible Root Nodes (H)

[Leetcode](https://leetcode.com/problems/count-number-of-possible-root-nodes/description/)

Alice has an undirected tree with `n` nodes labeled from `0` to `n - 1`. The tree is represented as a 2D integer array `edges` of length `n - 1` where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the tree.

Alice wants Bob to find the root of the tree. She allows Bob to make several **guesses** about her tree. In one guess, he does the following:

-   Chooses two **distinct** integers `u` and `v` such that there exists an edge `[u, v]` in the tree.
-   He tells Alice that `u` is the **parent** of `v` in the tree.

Bob's guesses are represented by a 2D integer array `guesses` where `guesses[j] = [uj, vj]` indicates Bob guessed `uj` to be the parent of `vj`.

Alice being lazy, does not reply to each of Bob's guesses, but just says that **at least** `k` of his guesses are `true`.

Given the 2D integer arrays `edges`, `guesses` and the integer `k`, return _the **number of possible nodes** that can be the root of Alice's tree_. If there is no such tree, return `0`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/12/19/ex-1.png)
```
Input: edges = [[0,1],[1,2],[1,3],[4,2]], guesses = [[1,3],[0,1],[1,0],[2,4]], k = 3
Output: 3
Explanation: 
Root = 0, correct guesses = [1,3], [0,1], [2,4]
Root = 1, correct guesses = [1,3], [1,0], [2,4]
Root = 2, correct guesses = [1,3], [1,0], [2,4]
Root = 3, correct guesses = [1,0], [2,4]
Root = 4, correct guesses = [1,3], [1,0]
Considering 0, 1, or 2 as root node leads to 3 correct guesses.
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2022/12/19/ex-2.png)
```
Input: edges = [[0,1],[1,2],[2,3],[3,4]], guesses = [[1,0],[3,4],[2,1],[3,2]], k = 1
Output: 5
Explanation: 
Root = 0, correct guesses = [3,4]
Root = 1, correct guesses = [1,0], [3,4]
Root = 2, correct guesses = [1,0], [2,1], [3,4]
Root = 3, correct guesses = [1,0], [2,1], [3,2], [3,4]
Root = 4, correct guesses = [1,0], [2,1], [3,2]
Considering any node as root will give at least 1 correct guess. 
```
**Constraints:**

-   `edges.length == n - 1`
-   `2 <= n <= 105`
-   `1 <= guesses.length <= 105`
-   `0 <= ai, bi, uj, vj <= n - 1`
-   `ai != bi`
-   `uj != vj`
-   `edges` represents a valid tree.
-   `guesses[j]` is an edge of the tree.
-   `guesses` is unique.
-   `0 <= k <= guesses.length`

---

```java
// Java 81ms(84.78%), Time O(N), Space O(N)
class Solution {
    ArrayList<Integer>[] next = new ArrayList[100005];
    HashSet<Integer>[] guess = new HashSet[100005];
    int ret = 0;
    int K;
    public int rootCount(int[][] edges, int[][] guesses, int k) {
        this.K = k;
        int n = edges.length + 1;
        // initial
        for (int i = 0; i < n; i++) {
            next[i] = new ArrayList<Integer>();
            guess[i] = new HashSet<Integer>();
        }
        // store edges
        for (int[] edge : edges) {
            next[edge[0]].add(edge[1]);
            next[edge[1]].add(edge[0]);
        }
        // store guesses
        for (int[] g : guesses) {
            guess[g[0]].add(g[1]);
        }

        // use 0 as root, calculate total correct guess
        int count = dfs(0, -1); 

        // re-root
        dfs2(0, -1, count);
        return ret;
    }

    public int dfs(int node, int parent) {
        int count = 0;
        for (int nxt : next[node]) {
            if (nxt == parent) continue;
            if (guess[node] != null && guess[node].contains(nxt))
                count++;
            count += dfs(nxt, node);
        }
        return count;
    }

    public void dfs2(int node, int parent, int count) {
        if (count >= K) ret++;

        for (int nxt : next[node]) {
            if (nxt == parent) continue;
            int tmp = count;
            if (guess[node].contains(nxt)) 
                tmp--;
            if (guess[nxt].contains(node)) 
                tmp++;
            dfs2(nxt, node, tmp);
        }
    }
}
```

---

[wisdompeak/YouTube解說](https://www.youtube.com/watch?v=Dq_c2XZV1jc&list=PLwdV8xC1EWHrtgsYCcDTXIMVaHSlsnLzL&index=3)

一般而言, 會想以所有的點當作root去計算出答案,
但測資為10^5, 因此只能以線性來計算

關於這類題目，有一種套路是`移根(re-root)`,
假設以A為root的答案為k, 那是否以A的相鄰節點B的答案是否能快速從A轉換而來?

假設以A為root的答案為`k`
當root由A轉成B時, 有改變的只有`A->B`這個線段, 
若猜想內有`A -> B`的線段時, 轉換為B後, B的答案為`k - 1`
若猜想內有`B -> A`的線段時, 轉換為B後, B的答案為`k + 1`

因此本題解法為, 以0作為root先計算出有多少個正確的猜想count,
再依相鄰的點一一計算出各點的count, 即可找出總共猜對幾個


###### tags: `Leetcode` `Tree` `Re-Root`