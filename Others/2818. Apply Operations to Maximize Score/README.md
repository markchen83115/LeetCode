# Leetcode - 2818. Apply Operations to Maximize Score (H-)

[Leetcode](https://leetcode.com/problems/apply-operations-to-maximize-score/)

You are given an array `nums` of `n` positive integers and an integer `k`.

Initially, you start with a score of `1`. You have to maximize your score by applying the following operation at most `k` times:

-   Choose any **non-empty** subarray `nums[l, ..., r]` that you haven't chosen previously.
-   Choose an element `x` of `nums[l, ..., r]` with the highest **prime score**. If multiple such elements exist, choose the one with the smallest index.
-   Multiply your score by `x`.

Here, `nums[l, ..., r]` denotes the subarray of `nums` starting at index `l` and ending at the index `r`, both ends being inclusive.

The **prime score** of an integer `x` is equal to the number of distinct prime factors of `x`. For example, the prime score of `300` is `3` since `300 = 2 * 2 * 3 * 5 * 5`.

Return _the **maximum possible score** after applying at most _`k`_ operations_.

Since the answer may be large, return it modulo `109 + 7`.

**Example 1:**

> **Input:** nums = [8,3,9,3,8], k = 2
> **Output:** 81
> **Explanation:** To get a score of 81, we can apply the following operations:
> - Choose subarray nums[2, ..., 2]. nums[2] is the only element in this subarray. Hence, we multiply the score by nums[2]. The score becomes 1 * 9 = 9.
> - Choose subarray nums[2, ..., 3]. Both nums[2] and nums[3] have a prime score of 1, but nums[2] has the smaller index. Hence, we multiply the score by nums[2]. The score becomes 9 * 9 = 81.
> It can be proven that 81 is the highest score one can obtain.

**Example 2:**

> **Input:** nums = [19,12,14,6,10,18], k = 3
> **Output:** 4788
> **Explanation:** To get a score of 4788, we can apply the following operations: 
> - Choose subarray nums[0, ..., 0]. nums[0] is the only element in this subarray. Hence, we multiply the score by nums[0]. The score becomes 1 * 19 = 19.
> - Choose subarray nums[5, ..., 5]. nums[5] is the only element in this subarray. Hence, we multiply the score by nums[5]. The score becomes 19 * 18 = 342.
> - Choose subarray nums[2, ..., 3]. Both nums[2] and nums[3] have a prime score of 2, but nums[2] has the smaller index. Hence, we multipy the score by nums[2]. The score becomes 342 * 14 = 4788.
> It can be proven that 4788 is the highest score one can obtain.

**Constraints:**

-   `1 <= nums.length == n <= 105`
-   `1 <= nums[i] <= 105`
-   `1 <= k <= min(n * (n + 1) / 2, 109)`

---
```java
// Java 238ms(Beats 93.65%), Time O(NloglogN), Space O(N)
class Solution {
    int mod = (int) 1e9 + 7;
    public int maximumScore(List<Integer> nums, int k) {
        int n = nums.size();
        int MAX = 0;
        for (int num : nums)
            MAX = Math.max(MAX, num);

        int[] era = Eratosthenes(MAX);
        int[] score = new int[n];
        for (int i = 0; i < n; i++)
            score[i] = era[nums.get(i)];

        int[] prevLarger = new int[n];
        Arrays.fill(prevLarger, -1);
        Deque<Integer> stack = new ArrayDeque<>();
        for (int i = 0; i < n; i++)
        {
            while (!stack.isEmpty() && score[stack.peek()] < score[i])
                stack.pop();
            if (!stack.isEmpty())
                prevLarger[i] = stack.peek();
            stack.push(i);
        }

        int[] nextLarger = new int[n];
        Arrays.fill(nextLarger, n);
        stack = new ArrayDeque<>();
        for (int i = n - 1; i >= 0; i--)
        {
            while (!stack.isEmpty() && score[stack.peek()] <= score[i])
                stack.pop();
            if (!stack.isEmpty())
                nextLarger[i] = stack.peek();
            stack.push(i);
        }

        long ret = 1;
        long[][] tmp = new long[n][];
        for (int i = 0; i < n; i++)
        {
            long t = (long)(i - prevLarger[i]) * (nextLarger[i] - i);
            tmp[i] = new long[] {nums.get(i), t};
        }

        Arrays.sort(tmp, (a, b) -> Long.compare(b[0], a[0]));
        for (long[] p : tmp)
        {
            long num = p[0], t = p[1];
            if ((long) k >= t)
            {
                ret = ret * quickPow(num, t) % mod;
                k -= t;
            }
            else
            {
                ret = ret * quickPow(num, k) % mod;
                k = 0;
            }
            if (k == 0) break;
        }
        return (int) ret;
    }

    int[] Eratosthenes(int n)   // 埃氏篩 O(NloglogN)
    {
        int[] nums = new int[n + 1];
        for (int i = 2; i <= n; i++)
        {
            if (nums[i] >= 1) continue;
            nums[i] += 1;
            int j = i * 2;
            while (j <= n)
            {
                nums[j] += 1;
                j += i;
            }
        }
        return nums;
    }

    long quickPow(long x, long k) {
        if (k == 0) {
            return 1;
        }
        long y = quickPow(x, k / 2) % mod;
        return k % 2 == 0 ? (y * y % mod) : (y * y % mod * x % mod);
    }
}
```
---

簡單來說這是個大雜燴題，融了四種概念在裡面
1. Omega 函數：找出某個數字有幾個不同的質因數，可以用埃式篩法找出來
2. 區間領域展開：或者說「貢獻法」，找出某個數字他可以往左右延伸多少範圍內是最猛的，也就是要用 mono stack 找 prev & next greater element
3. greedy & sort：要讓 ans 最大需要從最大的 element 開始挑
4. Fast power：快速算出 a^b % mod 的值，python 可以直接用 pow()，不用傻傻自己實作

---

參考[【每日一题】LeetCode 2818. Apply Operations to Maximize Score - YouTube](https://www.youtube.com/watch?v=-h12SmBQznA)

首先，根據題意，我們要在n^2個區間裡挑選k個區間。這n^2個區間裡，有的x可以很大，有的x會很小。我們不會去遍歷所有這n^2個區間、再根據他們的x排序。相較之下，x的取值範圍只有n種（即nums裡的n個元素），透過遍歷x來列舉區間的效率更高。

顯然，我們必然會貪心地使用「x最大」的那些區間，我們將nums陣列裡最大元素記做nums[i]。那麼有多少區間的`highest prime score`是nums[i]呢？假設每個元素的`prime right-i)`. 也就是說，在最終選取的k個區間裡，我們優先選取這`(i-left)*(right-i)`個區間，因為每次都可以讓結果乘以nums[i]（全域最大的x）。

以此類推，我們再貪心地使用「x第二大」的那些區間，記做nums[j]。同理計算出有多少區間滿足scores[j]是最大元素。當k還沒選夠時，我們就會優先使用這些區間。

再找nums第三大元素、第四大元素... 直到把k個區間都用完。

以上就是本題的大致思路。其中還有不少小問題。比如

1. 怎麼預處理得到scores數組？可以用埃氏篩的思路，在根據某個質因數向上篩除合數時，可以順便給該合數增1，就可以記錄下每個數的distinct質因數的個數了。
2. 如何求`previous larger or equal number`和`next larger number`，這是單調堆疊的經典應用了。
3. 假設某個數x對應的區間有P個，那麼我們就要`ret *= x^P`，其中P可能很大，所以需要呼叫快速冪的libary。


###### tags: `Leetcode` `Others` `Count Subarray by Element`