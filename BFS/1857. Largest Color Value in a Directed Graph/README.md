# Leetcode - 1857. Largest Color Value in a Directed Graph (H-)

[Leetcode](https://leetcode.com/problems/largest-color-value-in-a-directed-graph/)

There is a **directed graph** of `n` colored nodes and `m` edges. The nodes are numbered from `0` to `n - 1`.

You are given a string `colors` where `colors[i]` is a lowercase English letter representing the **color** of the `ith` node in this graph (**0-indexed**). You are also given a 2D array `edges` where `edges[j] = [aj, bj]` indicates that there is a **directed edge** from node `aj` to node `bj`.

A valid **path** in the graph is a sequence of nodes `x1 -> x2 -> x3 -> ... -> xk` such that there is a directed edge from `xi` to `xi+1` for every `1 <= i < k`. The **color value** of the path is the number of nodes that are colored the **most frequently** occurring color along that path.

Return _the **largest color value** of any valid path in the given graph, or _`-1`_ if the graph contains a cycle_.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2021/04/21/leet1.png)
> 
> **Input:** colors = "abaca", edges = [[0,1],[0,2],[2,3],[3,4]]
> **Output:** 3
> **Explanation:** The path 0 -> 2 -> 3 -> 4 contains 3 nodes that are colored `"a" (red in the above image)`.

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2021/04/21/leet2.png)
> 
> **Input:** colors = "a", edges = [[0,0]]
> **Output:** -1
> **Explanation:** There is a cycle from 0 to 0.

**Constraints:**

-   `n == colors.length`
-   `m == edges.length`
-   `1 <= n <= 105`
-   `0 <= m <= 105`
-   `colors` consists of lowercase English letters.
-   `0 <= aj, bj < n`

---
```java
// Java 52ms(Beats 97.73%), Time O(26N), Space O(N)
class Solution {
    List<Integer>[] next = new List[100005];
    int[] inDegree = new int[100005];

    public int largestPathValue(String colors, int[][] edges) {
        int n = colors.length();
        HashSet<Character> set = new HashSet<>();
        for (int i = 0; i < n; i++)
        {
            next[i] = new ArrayList<>();
            set.add(colors.charAt(i));
        }

        for (int[] e : edges)
        {
            int a = e[0], b = e[1];
            next[a].add(b);
            inDegree[b]++;
        }

        int ret = 1;
        for (char ch = 'a'; ch <= 'z'; ch++)
        {
            if (!set.contains(ch))
                continue;
            int ans = helper(ch - 'a', colors);
            if (ans == -1)
                return -1;
            ret = Math.max(ret, ans);
        }
        return ret;
    }

    int helper(int k, String colors)
    {
        int n = colors.length();
        int ret = 0;
        int visited = 0;
        int[] inD = Arrays.copyOf(inDegree, n);
        int[] count = new int[n];
        // count[i] = how many color k at most are there along the path from a start to the i-th node
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < n; i++)
            if (inD[i] == 0)
            {
                queue.offer(i);
                if (colors.charAt(i) - 'a' == k)
                    count[i] = 1;
            }

        while (!queue.isEmpty())
        {
            int cur = queue.poll();
            for (int p : next[cur])
            {
                count[p] = Math.max(count[p], count[cur] + (colors.charAt(p) - 'a' == k ? 1 : 0));
                ret = Math.max(ret, count[p]);
                if (--inD[p] == 0)
                    queue.offer(p);
            }
            visited++;
        }
        
        if (visited < n)
            return -1;

        return ret;
    }
}
```
---

參考[【每日一题】1857. Largest Color Value in a Directed Graph - YouTube](https://www.youtube.com/watch?v=VH1UevGQ4KQ)

首先一張圖裡是否有環，這個容易判斷。我們現在只考慮這張圖沒有環的情況，也就是這張圖裡的每個連通區域都是樹。

顯然，為了最大化題目所求的最大元素頻次，我們顯然會希望這個路徑盡量長，這樣才能經過更多的節點，增加每個元素的頻次。所以我們必然會找那些入度為零的節點作為路徑起點。

如果這條路徑沒有分叉，那麼我們在前進的過程中只要維護一個長度為26的計數器就行，用來記錄從路徑起點到該節點時所經過的字元頻次。接下來一個關鍵的問題是，如果有一條路徑到A時的頻次統計是count1，另一條路徑到B時的頻次統計是count2，而且A和B都會指向C，那麼我們要如何設定C的頻次統計？此時我們並不清楚，C的頻次統計是應該繼承自A還是B，這是因為我們不知道最終哪個字元的頻次會最大。可能在count1裡面字母a出現得最多，在count2裡面字母b出現得最多；如果最終全局來看a的頻次最多，那麼C應該繼承自count1，以最大化a的頻次；否則就應該繼承自count2.

所以我們這裡就會發現，如果不知道我們關注的究竟是哪個字符，那麼在這個交叉點上的選擇會很糾結。所以這就提示我們，不妨將「關注的字符」給固定下來。將原本的問題變成26個子問題，分別求一條路徑裡出現a的最多頻次、一條路徑裡出現b的最多頻次、一條路徑裡出現c的最多頻次... 對於每個子問題，我們用拓撲排序（考察當前入度是否為零）來遍歷所有節點，時間複雜度為o(N)。此時重新回顧上面的問題，C節點應該繼承自哪個路徑？取決於考察的字元ch，只需貪心地選count1和count2中ch的頻次更多的那個。

將問題拆解之後，整體的複雜度就是O(26N). 但仍有case會遇到TLE。改進的方法是指考察colors裡面出現過的字符，不用26個字符都跑一遍。


###### tags: `Leetcode` `BFS` `拓撲排序`