# Leetcode - 077. Combinations (H-)

[Leetcode](https://leetcode.com/problems/combinations/)

Given two integers `n` and `k`, return _all possible combinations of_ `k` _numbers chosen from the range_ `[1, n]`.

You may return the answer in **any order**.

**Example 1:**

> **Input:** n = 4, k = 2
> **Output:** [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
> **Explanation:** There are 4 choose 2 = 6 total combinations.
> Note that combinations are unordered, i.e., [1,2] and [2,1] are considered to be the same combination.

**Example 2:**

> **Input:** n = 1, k = 1
> **Output:** [[1]]
> **Explanation:** There is 1 choose 1 = 1 total combination.

**Constraints:**

-   `1 <= n <= 20`
-   `1 <= k <= n`

---
```java
// Java 18ms(Beats 80.56%), Time O(N*(N-1)*(N-2)* ... *(N-K)), Space O(N*(N-1)*(N-2)* ... *(N-K))
class Solution {
    List<List<Integer>> ret = new ArrayList<>();
    public List<List<Integer>> combine(int n, int k) {
        backtracking(n, k, 1, new ArrayList<>());
        return ret;
    }
    
    void backtracking(int n, int k, int p, List<Integer> list)
    {
        if (list.size() == k)
        {
            ret.add(new ArrayList<>(list));
            return;
        }

        for (int i = p; i <= n; i++)
        {
            list.add(i);
            backtracking(n, k, i + 1, list);
            list.remove(list.size() - 1);
        }
    }
}
```
---

比較容易實現的是遞歸的演算法，就是最基本的DFS。

還有一種更優秀的數學建構法，值得學習一下。我們的目的是用n個數字構造一個逐位遞增的k位數，現在嘗試從小到大構造next combination.

以n=5，k=7為例子。最小的combination應該是12345.那我們會如何構造next comb呢？很顯然我們會在最低位嘗試加1，寫出12346。再下一個，我們會寫出12347.那麼再下一個呢？顯然最低位我們無法再增加1了，只好考慮十位數，發現可以在數字4上可以增加1也就是5；之後在最低位上，我們肯定會取最小的選擇6，於是我們就寫出了12356。

依序類推，假設目前的comb是12367，它的next comb是什麼呢？個位數不能增加了；十位數也不能增加了（否則十位數是7的話，個位數就沒法填寫了）；我們只有考慮在百位數上增加1.於是我們最終得到的是12456，

至此我們可以大致總結出做法。我們會從最低位開始看，嘗試能否加1，如果那一位的數字已經到頂，我們就往前面找，知道能找到一位可以加1的地方comb[i]；然後對於comb[i]後面的數字，我們會設置最小的組合，也就是依次令comb[j]=comb[j-1]+1。

一個最關鍵的分析，comb[i]的數位封頂是多少呢？簡單的分析可以知道是i+1+n-k（注意我們這裡的i是0-index）。理由是如果comb[i]取得再大的話，就沒有足夠的數字來填滿i之後的位置了。


###### tags: `Leetcode` `Math` `Combinatorics`