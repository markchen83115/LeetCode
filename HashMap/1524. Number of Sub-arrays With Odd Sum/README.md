# Leetcode - 1524. Number of Sub-arrays With Odd Sum (M)

[Leetcode](https://leetcode.com/problems/number-of-sub-arrays-with-odd-sum/)

Given an array of integers `arr`, return _the number of subarrays with an **odd** sum_.

Since the answer can be very large, return it modulo `109 + 7`.

**Example 1:**

> **Input:** arr = [1,3,5]
> **Output:** 4
> **Explanation:** All subarrays are [[1],[1,3],[1,3,5],[3],[3,5],[5]]
> All sub-arrays sum are [1,4,9,3,8,5].
> Odd sums are [1,9,3,5] so the answer is 4.

**Example 2:**

> **Input:** arr = [2,4,6]
> **Output:** 0
> **Explanation:** All subarrays are [[2],[2,4],[2,4,6],[4],[4,6],[6]]
> All sub-arrays sum are [2,6,12,4,10,6].
> All sub-arrays have even sum and the answer is 0.

**Example 3:**

> **Input:** arr = [1,2,3,4,5,6,7]
> **Output:** 16

**Constraints:**

-   `1 <= arr.length <= 105`
-   `1 <= arr[i] <= 100`

---
```java
// Java 7ms(Beats 83.08%), Time O(N), Space O(1)
class Solution {
    public int numOfSubarrays(int[] arr) {
        int ret = 0;
        int n = arr.length;
        int mod = (int) 1e9 + 7;
        int odd = 0, even = 1;
        int sum = 0;

        for (int i = 0; i < n; i++)
        {
            sum += arr[i];
            if (sum % 2 == 0)
            {
                ret += odd;
                even++;
            }
            else
            {
                ret += even;
                odd++;
            }
            ret %= mod;
        }
        return ret;
    }
}
```
---

對於區間和的問題，我們自然會轉化為前綴和之差。另外，有關區間個數的題目，一般是遍歷/固定一個端點，考察另一個端點的位置，這樣區間的計數就不會重複和遺漏。

右端點位於i的區間，我們考慮左端點放在哪些位置可以滿足條件呢？顯然我們需要找到一個位置j，使得presum[i]-presum[j]是奇數，這樣區間[j+1,i]的和一定就是奇數。所以我們只需要維護兩個計數器，分別統計在i之前的presum裡出現過多少次奇數count1和多少次偶數count2。如果presum[i]是奇數，那麼就有count2個適當的左端點j可以與之配對。如果presum[i]是偶數，那麼就有count1個適當的左端點j可以與之配對。

最後我們將每個i所對應的j的個數累積起來，就是答案。


###### tags: `Leetcode` `HashMap` `Hash+Prefix`