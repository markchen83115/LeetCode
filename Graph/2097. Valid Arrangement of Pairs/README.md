# Leetcode - 2097. Valid Arrangement of Pairs (H)

[Leetcode](https://leetcode.com/problems/valid-arrangement-of-pairs/)

You are given a **0-indexed** 2D integer array `pairs` where `pairs[i] = [starti, endi]`. An arrangement of `pairs` is **valid** if for every index `i` where `1 <= i < pairs.length`, we have `endi-1 == starti`.

Return _**any** valid arrangement of _`pairs`.

**Note:** The inputs will be generated such that there exists a valid arrangement of `pairs`.

**Example 1:**

> **Input:** pairs = [[5,1],[4,5],[11,9],[9,4]]
> **Output:** [[11,9],[9,4],[4,5],[5,1]]
> **Explanation:** This is a valid arrangement since endi-1 always equals starti.
> end0 = 9 == 9 = start1 
> end1 = 4 == 4 = start2
> end2 = 5 == 5 = start3

**Example 2:**

> **Input:** pairs = [[1,3],[3,2],[2,1]]
> **Output:** [[1,3],[3,2],[2,1]]
> **Explanation:**
> This is a valid arrangement since endi-1 always equals starti.
> end0 = 3 == 3 = start1
> end1 = 2 == 2 = start2
> The arrangements [[2,1],[1,3],[3,2]] and [[3,2],[2,1],[1,3]] are also valid.

**Example 3:**

> **Input:** pairs = [[1,2],[1,3],[2,1]]
> **Output:** [[1,2],[2,1],[1,3]]
> **Explanation:**
> This is a valid arrangement since endi-1 always equals starti.
> end0 = 2 == 2 = start1
> end1 = 1 == 1 = start2

**Constraints:**

-   `1 <= pairs.length <= 105`
-   `pairs[i].length == 2`
-   `0 <= starti, endi <= 109`
-   `starti != endi`
-   No two pairs are exactly the same.
-   There **exists** a valid arrangement of `pairs`.

---
```java
// Java 125ms(Beats 86.72%), Time O(N), Space O(N)
class Solution {
    public int[][] validArrangement(int[][] pairs) {
        HashMap<Integer, Deque<Integer>> nextMap = new HashMap<>();
        HashMap<Integer, Integer> inDegreeMap = new HashMap<>();
        HashMap<Integer, Integer> outDegreeMap = new HashMap<>();
        int n = pairs.length;
        for (int i = 0; i < n; i++)
        {
            nextMap.computeIfAbsent(pairs[i][0], v -> new ArrayDeque<>()).add(pairs[i][1]);
            inDegreeMap.merge(pairs[i][0], 1, Integer::sum);
            outDegreeMap.merge(pairs[i][1], 1, Integer::sum);
        }

        //  找到起點 (inDegree - outDegree = 1)
        int startVal = -1;
        for (int s : inDegreeMap.keySet())
            if (inDegreeMap.get(s) - outDegreeMap.getOrDefault(s, 0) == 1)
            {
                startVal = s;
                break;
            }
        
        if (startVal == -1)
            startVal = pairs[0][0];
        
        List<Integer> list = new ArrayList<>();
        //  倒序DFS
        dfs(startVal, nextMap, list);

        Collections.reverse(list);

        int[][] ret = new int[n][2];
        for (int i = 1; i <= n; i++)
            ret[i - 1] = new int[] {list.get(i - 1), list.get(i)};
        
        return ret;
    }

    void dfs(int node, HashMap<Integer, Deque<Integer>> nextMap, List<Integer> list)
    {
        Deque<Integer> nextQueue = nextMap.get(node);
        while (nextQueue != null && !nextQueue.isEmpty())
        {
            int nxt = nextQueue.pollFirst();
            dfs(nxt, nextMap, list);
        }
        list.add(node);
    }
    // [11,5],[5,4],[4,5],[5,1]
}
```
---

[【每日一题】LeetCode 2097. Valid Arrangement of Pairs - YouTube](https://youtu.be/vRZcrOytvgs)

此題學習`倒序DFS`
以及歐拉路徑如何找出起點 利用`inDegree - outDegree = 1`

以下節錄自[LeetCode/Graph/2097.Valid-Arrangement-of-Pairs at master · wisdompeak/LeetCode · GitHub](https://github.com/wisdompeak/LeetCode/tree/master/Graph/2097.Valid-Arrangement-of-Pairs)

因為本題保證一定有解，所以這就是一個歐拉路徑問題：我們將每個數字當作節點，每個pair看做是兩個節點之間的路徑，那麼本題就是規劃一條路徑，要求不重複地走過所有邊。類似的題目有LC332。

對於歐拉路徑，我們回顧一下相關的知識點：

歐拉路徑：從一個點出發，到達另一個點，所有的邊都經過且只經過1次。

歐拉迴路：歐拉路徑中，終點能回到起點。

如果判斷是否存在歐拉路徑？

1.無向圖：(a) 如果只有兩個點的度是奇數，其他的點的度都是偶數，則存在從一個奇數度點到另一個奇數度點的歐拉路徑（不是迴路）。 (b) 如果所有的點的度數都是偶數，那就是歐拉迴路。

2.有向圖：(a) 如果最多有一個點出度大於入度by1，且最多有一個點入度大於出度by1，那麼就有一條從前者（如果沒有則可以任意）到後者（如果沒有則可以任意）的歐拉路徑。 (b) 如果所有的點的入度等於出度，那麼就存在歐拉迴路。

對本題而言，我們只要依照有向圖的規則，找出`出度-入度==1`的那一點作為起點。如果不存在，就可以任意為起點。

那麼如何規劃這條路徑呢？我們需要有這樣一個概念：對於一個注定存在歐拉路徑的圖而言，拋開起點，只可能最多有一個dead end （這是注定的終點）。假設存在這樣一個死胡同，那麼你從任意一個點出發，在不重複走邊的前提下任意走，最終狀態一定會走進這個死胡同；並且此時未走過的邊，必然都是一個一個封閉的環。這是因為歐拉路徑裡除了起點和終點，其他所有點都是入度等於出度，因此除了dead end外，你只要能從某邊進入某點，必然可以通過另一條邊出去。

於是我們可以這樣規劃DFS演算法：當你打算從start出發，隨便選一個出口，就無腦遞歸下去，這條路徑最終會走進一個dead end，我們記做path1. 然後我們考察start還有其他未被遍歷的那些邊。根據先前的分析，走這些邊之後必然是經歷一個封閉的環，也就是你無論選哪個邊走下去，最終都肯定會走回來，我們將這些記做path2a, path2b, path2c... 於是我們只要這樣規劃路徑：`start + ... + path2c + path2b + path2a + path1`，也就是將唯一會走入死胡同的路徑放在最後一個即可，這樣就構造一條從start開始的歐拉路徑。

那如果全圖沒有dead end呢？那麼相當於沒有path1，你從start的任何一個邊出去，一定會再轉回來。上述的演算法依然有效。


###### tags: `Leetcode` `Graph`