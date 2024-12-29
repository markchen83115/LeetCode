# Leetcode - 3404. Count Special Subsequences (M+)

[Leetcode](https://leetcode.com/problems/count-special-subsequences/)

You are given an array `nums` consisting of positive integers.

A **special subsequence** is defined as a subsequence of length 4, represented by indices `(p, q, r, s)`, where `p < q < r < s`. This subsequence **must** satisfy the following conditions:

-   `nums[p] * nums[r] == nums[q] * nums[s]`
-   There must be _at least_ **one** element between each pair of indices. In other words, `q - p > 1`, `r - q > 1` and `s - r > 1`.

A subsequence is a sequence derived from the array by deleting zero or more elements without changing the order of the remaining elements.

Return the _number_ of different **special** **subsequences** in `nums`.

**Example 1:**

> **Input:** nums = [1,2,3,4,3,6,1]
> 
> **Output:** 1
> 
> **Explanation:**
> 
> There is one special subsequence in `nums`.
> 
> -   `(p, q, r, s) = (0, 2, 4, 6)`:
> -   This corresponds to elements `(1, 3, 3, 1)`.
> -   `nums[p] * nums[r] = nums[0] * nums[4] = 1 * 3 = 3`
> -   `nums[q] * nums[s] = nums[2] * nums[6] = 3 * 1 = 3`

**Example 2:**

> **Input:** nums = [3,4,3,4,3,4,3,4]
> 
> **Output:** 3
> 
> **Explanation:**
> 
> There are three special subsequences in `nums`.
> 
> -   `(p, q, r, s) = (0, 2, 4, 6)`:
>     -   This corresponds to elements `(3, 3, 3, 3)`.
>     -   `nums[p] * nums[r] = nums[0] * nums[4] = 3 * 3 = 9`
>     -   `nums[q] * nums[s] = nums[2] * nums[6] = 3 * 3 = 9`
> -   `(p, q, r, s) = (1, 3, 5, 7)`:
>     -   This corresponds to elements `(4, 4, 4, 4)`.
>     -   `nums[p] * nums[r] = nums[1] * nums[5] = 4 * 4 = 16`
>     -   `nums[q] * nums[s] = nums[3] * nums[7] = 4 * 4 = 16`
> -   `(p, q, r, s) = (0, 2, 5, 7)`:
>     -   This corresponds to elements `(3, 3, 4, 4)`.
>     -   `nums[p] * nums[r] = nums[0] * nums[5] = 3 * 4 = 12`
>     -   `nums[q] * nums[s] = nums[2] * nums[7] = 3 * 4 = 12`

**Constraints:**

-   `7 <= nums.length <= 1000`
-   `1 <= nums[i] <= 1000`

---
```java
class Solution {
    public long numberOfSubsequences(int[] nums) { 
        long ret = 0;
        HashMap<Double, Integer> map = new HashMap<>();
        int n = nums.length;
        for (int r = 4; r < n - 2; r++) {   // 固定r
            // 計算p/q的數量
            int q = r - 2;
            for (int p = 0; p <= q - 2; p++)
            {
                double div = (double) nums[p] / nums[q];
                map.merge(div, 1, Integer::sum);
            }

            // 計算p/q == s/r 的數量
            for (int s = r + 2; s < n; s++)
            {
                ret += map.getOrDefault((double) nums[s] / nums[r], 0);
            }
        }

        return ret;
    }
}
```
---

題目要求`nums[p] * nums[r] == nums[q] * nums[s]` 
=> 想為`nums[p] / nums[q] == nums[s] / nums[r]`

```
x x x x x x x 
p   q   r   s  
```

我們固定`r`, 計算以`r`為基礎點, 固定`q = r-2`, 遍歷`p = [0:q-2]`計算`p/q`的數值個數
接著遍歷`s = [r+2, n-1]`, 計算符合`p/q == s/r`的個數

接著下一輪, `r`向右移動一步, 因此`q`也會向右移動一步, 
保持`q = r-2`, 遍歷`p = [0:q-2]`計算`p/q`的數值個數
以此類推...


###### tags: `Leetcode` `Greedy`