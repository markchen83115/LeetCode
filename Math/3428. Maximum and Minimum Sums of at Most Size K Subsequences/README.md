# Leetcode - 3428. Maximum and Minimum Sums of at Most Size K Subsequences (M+)

[Leetcode](https://leetcode.com/problems/maximum-and-minimum-sums-of-at-most-size-k-subsequences/)

You are given an integer array `nums` and a positive integer `k`. Return the sum of the **maximum** and **minimum** elements of all **subsequences** of `nums` with **at most** `k` elements.

A **non-empty subsequence **is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

Since the answer may be very large, return it **modulo** `109 + 7`.

**Example 1:**

> **Input:** nums = [1,2,3], k = 2
> 
> **Output:** 24
> 
> **Explanation:**
> 
> The subsequences of `nums` with at most 2 elements are:
> 
> | **Subsequence** | Minimum | Maximum | Sum |
> | --- | --- | --- | --- |
> | `[1]` | 1 | 1 | 2 |
> | `[2]` | 2 | 2 | 4 |
> | `[3]` | 3 | 3 | 6 |
> | `[1, 2]` | 1 | 2 | 3 |
> | `[1, 3]` | 1 | 3 | 4 |
> | `[2, 3]` | 2 | 3 | 5 |
> | **Final Total** |   |   | 24 |
> 
> The output would be 24.

**Example 2:**

> **Input:** nums = [5,0,6], k = 1
> 
> **Output:** 22
> 
> **Explanation:**
> 
> For subsequences with exactly 1 element, the minimum and maximum values are the element itself. Therefore, the total is `5 + 5 + 0 + 0 + 6 + 6 = 22`.

**Example 3:**

> **Input:** nums = [1,1,1], k = 2
> 
> **Output:** 12
> 
> **Explanation:**
> 
> The subsequences `[1, 1]` and `[1]` each appear 3 times. For all of them, the minimum and maximum are both 1. Thus, the total is 12.

**Constraints:**

-   `1 <= nums.length <= 105`
-   `0 <= nums[i] <= 109`
-   `1 <= k <= min(100, nums.length)`

---
```java
// Java 484ms(Beats 100.00%), Time O(M*N), Space O(N)
class Solution {
    long mod = (int) 1e9 + 7;
    long[] factorial;
    long[] invFactorial;

    void getFactorial(int n)
    {
        factorial[0] = invFactorial[0] = 1;
        for (int i = 1; i<=n; i++)
            factorial[i] = factorial[i-1] * i % mod;

        invFactorial[n] = quickPow(factorial[n], mod - 2);
        for (int i = n - 1; i >= 1; i--) {
            invFactorial[i] = invFactorial[i + 1] * (i + 1) % mod;
        }
    }
    
    long quickPow(long base, long exp) {
        long result = 1;
        while (exp > 0) {
            if ((exp & 1) == 1) {
                result = (result * base) % mod;
            }
            base = (base * base) % mod;
            exp >>= 1;
        }
        return result;
    }
    
    long comb(int n, int k) {
        if (k > n || k < 0) 
            return 0;
        return factorial[n] * invFactorial[k] % mod * invFactorial[n - k] % mod;
    }
    
    public int minMaxSums(int[] nums, int k) {
        int n = nums.length;
        long ret = 0;
        factorial = new long[100005];
        invFactorial = new long[100005];
        getFactorial(100000);

        Arrays.sort(nums);

        for (int i = 0; i < n; i++)
            for (int size = 1; size <= k; size++)
            {
                long minContribution = nums[i] * comb(n - 1 - i, size - 1) % mod;
                long maxContribution = nums[i] * comb(i, size - 1) % mod;
                ret = (ret + minContribution + maxContribution) % mod;
            }

        return (int)ret;   
    }   
}
```
---

參考template - Combination 排列組合

將nums排序, 用nums[i]當作最小值或最大值
查看當nums[i]為最小值時, 能做出多少貢獻
貢獻為`comb(n - 1 - i, size - 1)`
查看當nums[i]為最大值時, 能做出多少貢獻
貢獻為`comb(i, size - 1)`


###### tags: `Leetcode` `Math` `Combinatorics`