# Leetcode - 1922. Count Good Numbers (M)

[Leetcode](https://leetcode.com/problems/count-good-numbers/)

A digit string is **good** if the digits **(0-indexed)** at **even** indices are **even** and the digits at **odd** indices are **prime** (`2`, `3`, `5`, or `7`).

-   For example, `"2582"` is good because the digits (`2` and `8`) at even positions are even and the digits (`5` and `2`) at odd positions are prime. However, `"3245"` is **not** good because `3` is at an even index but is not even.

Given an integer `n`, return _the **total** number of good digit strings of length _`n`. Since the answer may be large, **return it modulo **`109 + 7`.

A **digit string** is a string consisting of digits `0` through `9` that may contain leading zeros.

> **Example 1:**
> 
> **Input:** n = 1
> **Output:** 5
> **Explanation:** The good numbers of length 1 are "0", "2", "4", "6", "8".

> **Example 2:**
> 
> **Input:** n = 4
> **Output:** 400

> **Example 3:**
> 
> **Input:** n = 50
> **Output:** 564908303

**Constraints:**

-   `1 <= n <= 1015`

---
```java
class Solution {
    long mod = (long) 1e9 + 7;
    public int countGoodNumbers(long n) {
        return (int) (quickPow(5L, (n + 1) / 2) * quickPow(4L, n / 2) % mod);
    }

    long quickPow(long x, long k)
    {
        if (k == 0)
            return 1;
        long y = quickPow(x, k / 2) % mod;

        return k % 2 == 0 ? y * y % mod : y * y % mod * x % mod;
    }

    // 0 2 4 6 8
    // 2 3 5 7
    // 5 * 4 * 5 * 4 ... 
}
```
---

本題就是直接用`50. Pow(x, n)`實作快速冪的程式碼
基本想法是：`pow(x, k) = pow(x, k/2) ^ 2 * (k % 2 == 1 ? x : 1);`


###### tags: `Leetcode` `Math`