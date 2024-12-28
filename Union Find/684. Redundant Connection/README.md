# Leetcode - 684. Redundant Connection (M)

[Leetcode](https://leetcode.com/problems/redundant-connection/)

In this problem, a tree is an **undirected graph** that is connected and has no cycles.

You are given a graph that started as a tree with `n` nodes labeled from `1` to `n`, with one additional edge added. The added edge has two **different** vertices chosen from `1` to `n`, and was not an edge that already existed. The graph is represented as an array `edges` of length `n` where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the graph.

Return _an edge that can be removed so that the resulting graph is a tree of _`n`_ nodes_. If there are multiple answers, return the answer that occurs last in the input.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2021/05/02/reduntant1-1-graph.jpg)
> 
> **Input:** edges = [[1,2],[1,3],[2,3]]
> **Output:** [2,3]

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2021/05/02/reduntant1-2-graph.jpg)
> 
> **Input:** edges = [[1,2],[2,3],[3,4],[1,4],[1,5]]
> **Output:** [1,4]

**Constraints:**

-   `n == edges.length`
-   `3 <= n <= 1000`
-   `edges[i].length == 2`
-   `1 <= ai < bi <= edges.length`
-   `ai != bi`
-   There are no repeated edges.
-   The given graph is connected.

---
```java
// Java 1ms(Beats 74.92%), Time O(N), Space O(N)
class Solution {
    int[] father = new int[1005];
    public int[] findRedundantConnection(int[][] edges) {
        for (int i = 0; i < 1005; i++)
            father[i] = i;
            
        for (int[] e : edges)
        {
            int a = e[0], b = e[1];
            if (findFather(a) == findFather(b))
                return e;
            else
                union(a, b);
        }

        return edges[0];
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

參考[【每日一题】684. Redundant Connection, 10/01/2019 - YouTube](https://youtu.be/8u-sjzyHjDg)

考慮這些edges所組成的網路是個無向圖。
那麼這個圖是tree的充要條件是：所有節點到另外一個節點的通路有且僅有一條。

所以從前往後遍歷edges，一旦發現某個edge的兩個點，已經在之前的遍歷中是聯通的了，那麼這個edge的加入就會導致「樹」定義的不成立，故必須除去。
根據題意，保證了只有唯一的答案，故就不用繼續往後查下去了。

於是本題就是一個考察union find的基本題。兩個基本操作要熟練：

```java
int FindSet(int x)
{
   if (x != Father[x])
   {
      Father[x] = FindSet(Father[x]);
   }
   return Father[x];
}
```

```java
void Union (int x, int y)
{
   x = Father[x];
   y = Father[y];
   if (x < y)
      Father[y] = x;
   else
      Father[x] = y;
}
```
在遍歷edges的過程中
```java
int x = edges[i][0];
int y = edges[i][1];
if (FindSet[x] == FindSet[y])  //注意，不是 if (Father[x]==Father[y])，因為Father[x]可能還沒更新到這個集合的根
    return edges[i];
else
    Union(x,y);
```

###### tags: `Leetcode` `Union Find`