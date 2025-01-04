# Leetcode - 332. Reconstruct Itinerary (H)

[Leetcode](https://leetcode.com/problems/reconstruct-itinerary/)

You are given a list of airline `tickets` where `tickets[i] = [fromi, toi]` represent the departure and the arrival airports of one flight. Reconstruct the itinerary in order and return it.

All of the tickets belong to a man who departs from `"JFK"`, thus, the itinerary must begin with `"JFK"`. If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string.

-   For example, the itinerary `["JFK", "LGA"]` has a smaller lexical order than `["JFK", "LGB"]`.

You may assume all tickets form at least one valid itinerary. You must use all the tickets once and only once.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2021/03/14/itinerary1-graph.jpg)
> 
> **Input:** tickets = [["MUC","LHR"],["JFK","MUC"],["SFO","SJC"],["LHR","SFO"]]
> **Output:** ["JFK","MUC","LHR","SFO","SJC"]

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2021/03/14/itinerary2-graph.jpg)
> 
> **Input:** tickets = [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
> **Output:** ["JFK","ATL","JFK","SFO","ATL","SFO"]
> **Explanation:** Another possible reconstruction is ["JFK","SFO","ATL","JFK","ATL","SFO"] but it is larger in lexical order.

**Constraints:**

-   `1 <= tickets.length <= 300`
-   `tickets[i].length == 2`
-   `fromi.length == 3`
-   `toi.length == 3`
-   `fromi` and `toi` consist of uppercase English letters.
-   `fromi != toi`

---
```java
// Java 5ms(Beats 99.33%), Time O(NlogN), Space O(N)
class Solution {
    List<String> ret = new ArrayList<>();
    HashMap<String, PriorityQueue<String>> next = new HashMap<>();
    public List<String> findItinerary(List<List<String>> tickets) {
        for (List<String> t : tickets)
        {
            String from = t.get(0), to = t.get(1);
            next.computeIfAbsent(from, v -> new PriorityQueue<>()).offer(to);
        }

        // 逆序DFS
        dfs("JFK");
        return ret;
    }

    void dfs(String x)
    {
        PriorityQueue<String> pq = next.get(x);
        while (pq != null && !pq.isEmpty())
            dfs(pq.poll());

        ret.add(0, x);
    }
}
```
---

參考[【每日一题】332. Reconstruct Itinerary, 6/28/2020 - YouTube](https://youtu.be/5yM3H0UgXTo)

### 現在來回顧幾個概念：
歐拉路徑：從一個點出發，到達另一個點，所有的邊都經過且只經過1次。

歐拉迴路：歐拉路徑中，終點能回到起點。

如果判斷是否存在歐拉路徑？

1.無向圖：(a) 如果只有兩個點的度是奇數，其他的點的度都是偶數，則存在從一個奇數度點到另一個奇數度點的歐拉路徑（不是迴路）。 (b) 如果所有的點的度數都是偶數，那就是歐拉迴路。

2.有向圖：(a) 如果最多有一個點出度大於入度by1，且最多有一個點入度大於出度by1，那麼就有一條從前者（如果沒有則可以任意）到後者（如果沒有則可以任意）的歐拉路徑。 (b) 如果所有的點的入度等於出度，那麼就存在歐拉迴路。

### 利用歐拉路徑的性質

本題保證了肯定有一條路徑：所有的航程都會用到，每個航程只用一次。從有向圖的角度來說，就是所給的圖就是一個歐拉路徑。讓你將這個路徑印出來。

這題本質就是一個歐拉一筆畫的問題。

有一個非常好的性質：每條邊都是必須遍歷的，而且只需要遍歷一次，因為它肯定是最終歐拉路徑的一部分。所以對邊的遍歷，我們都不該浪費（某種意義上可以存著再利用）。在探索歐拉路徑的過程中，不像無腦DFS那樣會有被「廢棄」的支路。因此，建構歐拉路徑的時間複雜度只需要o(E)。

接下來我們討論具體構造歐拉路徑的演算法。首先，我們先擺出這麼一個結論。假設我們第一次到達B點，開始往後遍歷，保證每條邊只走一次。接下來只有兩種可能：選擇某條支路走遍了所有後續節點並走到了終點，完美地建構了B之後的所有歐拉路徑。或者選擇某條支路走到了終點，但沒有遍歷完所有後續節點；我們只好回溯走另外一條支路，一番探索之後最終返回B點（此時B點沒有任何未走的出度）停止。

```
 -> D -> E
A -> B <-> F

```

如上述的例子（注意B->F和F->B是兩條不同的邊）。最理想的情況是一次遍歷走完所有想走的點B->F->B->D->E. 但是我們在B的支路選擇上不可能總是這麼幸運，可能會走這樣一條路->D->E，這樣走到了盡頭。但B還有另一條路->F沒有走，此時我們再嘗試走那一條支路的話，就是->F->B，然後停止（因為此時B沒有任何未走過的出度了）。

那我們建構歐拉路徑的想法是：B + path2 + path1，其中path1是從B點出發，選擇任意支路並且能夠順利走到終點的歐拉路徑。 path2是在path1走完之後，再從B點出發，最後走回B點的路徑。注意，如果夠幸運，path1走遍了B後面的所有邊，那麼path2就不存在了。


### 逆序DFS
用[倒序DFS](https://hackmd.io/_bV5k-tNRhmegLxeWiGE-w?view)來印出路徑
```java
void dfs(String x)
    {
        PriorityQueue<String> pq = next.get(x);
        while (pq != null && !pq.isEmpty())
            dfs(pq.poll());

        ret.add(0, x); // 逆序DFS的結果是相反的, 所以需要在reverse才是結果
    }
```


###### tags: `Leetcode` `Graph`