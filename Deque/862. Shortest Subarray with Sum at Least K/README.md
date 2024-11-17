# Leetcode - 862. Shortest Subarray with Sum at Least K (H)

[Leetcode](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/)

Given an integer array `nums` and an integer `k`, return _the length of the shortest non-empty **subarray** of _`nums`_ with a sum of at least _`k`. If there is no such **subarray**, return `-1`.

A **subarray** is a **contiguous** part of an array.

**Example 1:**

> **Input:** nums = [1], k = 1
> **Output:** 1

**Example 2:**

> **Input:** nums = [1,2], k = 4
> **Output:** -1

**Example 3:**

> **Input:** nums = [2,-1,2], k = 3
> **Output:** 3

**Constraints:**

-   `1 <= nums.length <= 105`
-   `-105 <= nums[i] <= 105`
-   `1 <= k <= 109`

---
```java
// Java 39ms(Beats 44.73%), Time O(N), Space O(N)
class Solution {
    public int shortestSubarray(int[] nums, int k) {
        int n = nums.length;
        int ret = n + 1;
        long[] preSum = new long[n + 1];
        for (int i = 1; i <= n; i++)
            preSum[i] = preSum[i - 1] + nums[i - 1];    // preSum[0] = 0, preSum[i] -> nums[i + 1]

        Deque<Integer> queue = new ArrayDeque<>();
        for (int i = 0; i <= n; i++)
        {
            while (!queue.isEmpty() && preSum[queue.peekLast()] >= preSum[i])
                queue.pollLast();

            while (!queue.isEmpty() && preSum[i] - preSum[queue.peekFirst()] >= k)
            {
                ret = Math.min(ret, i - queue.peekFirst());
                queue.pollFirst();
            }

            queue.offerLast(i);
        }

        return ret <= n ? ret : -1;
    }
}
```
---
參考wisdompeak解說
[【每日一题】LeetCode 862. Shortest Subarray with Sum at Least K - YouTube](https://youtu.be/HeFW6EPBGBg)


[LeetCode/Deque/862.Shortest-Subarray-with-Sum-at-Least-K at master · wisdompeak/LeetCode · GitHub](https://github.com/wisdompeak/LeetCode/tree/master/Deque/862.Shortest-Subarray-with-Sum-at-Least-K#%E8%A7%A3%E6%B3%952%E5%8D%95%E8%B0%83%E9%98%9F%E5%88%97)

#### 解法１

遇到continous subarray的題目,最常用的策略就是建立累加和preSum數組.這樣,此題就轉化為在preSum中找到兩個間距最短的位置j和i,使得`preSum[j]-preSum[i] >=K`.對於這種題,通常我們會遍歷i,然後在每個i之前的index查找滿足條件的最小j.

對於此類問題,我們會把所有經歷過的preSum都存在一個有序集合裡.這裡我們使用map,記錄曾經出現過的preSum以及它對應的在數組中的index.注意到如果遇到相同的preSum ,後加入的index會覆蓋先前的值,這是合理的:因為對於任何preSum我們恰需要更新的,較大的index來保持[j,i]之間的距離最短.

假設我們考慮某個i,那麼在i之前出現的所有preSum都已經在map裡.此時如果map裡所有鍵小於`preSum[i]-K`的preSum都是符合要求的.我們只要遍歷這些鍵對應的值(也就是index),找到最大的那個就是距離i最近的j.

這樣的演算法仍然會超時,主要是因為上面遍歷鍵值的過程花時間.怎麼優化呢?

我們每次在將`{preSum[i],i}`插入map時,插入的可能是map中間的某個位置.我們發現,此時所有大於preSum[i]的鍵都是沒有意義的,因為你preSum[i]帶來了當前最新(也是最大)的值i.舉個例子,如果此時目標鍵為k且k>preSum[i],那麼在map中搜尋所有小於等於k的"鍵"並且找它們之中最大的"值",得到的結果必然是i.所以每個回合裡,我們插入`{preSum[i],i}`之後,可以將map從後往前不斷刪除元素,直至遇到preSum[i]為止,這樣可以使得map始終保持精簡.

上面這個操作會帶來一個意想不到的好處!那就是我們不需要找在map裡遍歷所有小於等於`preSum[i]-K`的所有鍵並求最大的值．因為這些"鍵"對應的最大"值"就存在於離`preSum[i]-K`最近的那個鍵裡面．更本質的原因是，map裡不只鍵是遞增的，對應的值也是遞增的．

所以每個回合的過程可以總結為:

```
1.在map裡找到小於並且離preSum[i]-K最近的那個鍵,其值就是所需的滿足條件的最大j
2.在map插入{preSum[i],i}
3.在map刪除所有大於preSum[i]的鍵

```

#### 解法2：單調佇列

上述的解法複雜度是o(NlogN),但實際上還有更好的o(N)的解法．我們基於nums的前綴和陣列presum，維護一個雙端隊列q，保持隊列裡面的元素是遞增的。我們每處理一個新的presum[i]，希望在隊列裡查看最近的j，使得`presum[i]-presum[j]>=k`. 顯然我們希望j的位置盡量靠後，同時presum [j]的數值盡量小。

我們假想，presum的前若干個元素本身就是遞增的，那麼我們就可以照單全收都放入deque裡面。此時如果新元素presum[i]比隊尾元素（記做j）小，那麼我們就可以把隊尾元素j去掉。這是因為從此以後，presum[j]都不會是最優解所對應的區間左端點。考察上面式子的被減數，這個presum[j]相比於presum[i]而言既“老”又“大”，選j永遠不如選i。

同時針對新加入的presum[i]，我們考察隊首元素（也記做j），觀察是否滿足`presum[i]-presum[j]>=k`。如果是的話，顯然`[j+1,i]`就是一個合法的解。注意，此時我們就可以將j彈出了。因為我們不需要j再與其他位置（指i之後的）匹配合法的區間了，因為即使存在，那樣的區間長度也會更長。

所以每次處理一個presum[i]時，遵循上述兩個步驟，保證隊列儲存的是一個presum的遞增序列，就能夠方便的求出相對於i的區間左端點（如果存在的話，就是隊首元素）。


###### tags: `Leetcode` `Deque`