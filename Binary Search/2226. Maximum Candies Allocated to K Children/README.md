# Leetcode - 2226. Maximum Candies Allocated to K Children (M)

[Leetcode](https://leetcode.com/problems/maximum-candies-allocated-to-k-children/)

You are given a **0-indexed** integer array `candies`. Each element in the array denotes a pile of candies of size `candies[i]`. You can divide each pile into any number of **sub piles**, but you **cannot** merge two piles together.

You are also given an integer `k`. You should allocate piles of candies to `k` children such that each child gets the **same** number of candies. Each child can be allocated candies from **only one** pile of candies and some piles of candies may go unused.

Return _the **maximum number of candies** each child can get._

**Example 1:**

> **Input:** candies = [5,8,6], k = 3
> **Output:** 5
> **Explanation:** We can divide candies[1] into 2 piles of size 5 and 3, and candies[2] into 2 piles of size 5 and 1. We now have five piles of candies of sizes 5, 5, 3, 5, and 1. We can allocate the 3 piles of size 5 to 3 children. It can be proven that each child cannot receive more than 5 candies.

**Example 2:**

> **Input:** candies = [2,5], k = 11
> **Output:** 0
> **Explanation:** There are 11 children but only 7 candies in total, so it is impossible to ensure each child receives at least one candy. Thus, each child gets no candy and the answer is 0.

**Constraints:**

-   `1 <= candies.length <= 105`
-   `1 <= candies[i] <= 107`
-   `1 <= k <= 1012`

---
```java
// Java 10ms(Beats 100.00%), Time O(NlogN), Space O(1)
class Solution {
    public int maximumCandies(int[] candies, long k) {
        long sum = 0;
        for (int c : candies)
            sum += c;
        
        int l = 0, r = (int) (sum / k);
        while (l < r)
        {
            int mid = r - (r - l) / 2;
            if (isOK(candies, k, mid))
                l = mid;
            else
                r = mid - 1;
        }
        return l;
    }

    boolean isOK(int[] candies, long k, int target)
    {
        for (int c : candies)
        {
            k -= c / target;
            if (k <= 0)
                return true;
        }
        return false;
    }
}
```
---

這是一道非常明顯的二分搜值的題目。

我們令每個孩子可以分得的糖果數目記做x
如果x很大，意味著我們可以構造的、符合條件（即每堆恰有x個糖果）的堆數會變少，極有可能最終不夠k堆
如果x很小，代表我們可以構造出更多的、符合條件的堆數，甚至超過k堆，導致這個答案不夠優秀
所以我們可以透過二分搜尋嘗試不同的x的值，來逼近最大的x，使得構造出的堆數剛好大於等於k

對於給定的x，將每堆的糖果個數除以x，就是該堆可以分割出的、符合條件的堆數。我們只需檢驗符合條件的總堆數是否大於等於k即可

此題和`1891. Cutting Ribbons`一模一樣


###### tags: `Leetcode` `Binary Search` `Binary Search by Value`