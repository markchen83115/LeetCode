# Leetcode - 3334. Find the Maximum Factor Score of Array (M+)

[Leetcode](https://leetcode.com/problems/find-the-maximum-factor-score-of-array/)

You are given an integer array `nums`.

The **factor score** of an array is defined as the _product_ of the LCM and GCD of all elements of that array.

Return the **maximum factor score** of `nums` after removing **at most** one element from it.

**Note** that _both_ the LCM and GCD of a single number are the number itself, and the _factor score_ of an **empty** array is 0.

**Example 1:**

> **Input:** nums = [2,4,8,16]
> 
> **Output:** 64
> 
> **Explanation:**
> 
> On removing 2, the GCD of the rest of the elements is 4 while the LCM is 16, which gives a maximum factor score of `4 * 16 = 64`.

**Example 2:**

> **Input:** nums = [1,2,3,4,5]
> 
> **Output:** 60
> 
> **Explanation:**
> 
> The maximum factor score of 60 can be obtained without removing any elements.

**Example 3:**

> **Input:** nums = [3]
> 
> **Output:** 9

**Constraints:**

-   `1 <= nums.length <= 100`
-   `1 <= nums[i] <= 30`

---
```java
// Java 2ms(Beats 100.00%), Time O(N), Space O(N)
class Solution {
    public long maxScore(int[] nums) {
        int n = nums.length;
        long[] prefixGCD = new long[n];
        long[] prefixLCM = new long[n];
        prefixGCD[0] = nums[0];
        prefixLCM[0] = nums[0];

        // 1. calculate all prefix[i] for GCD and LCM
        for (int i = 1; i < n; i++)
        {
            prefixGCD[i] = gcd(prefixGCD[i - 1], nums[i]);
            prefixLCM[i] = lcm(prefixLCM[i - 1], nums[i]);
        }
        long ret = prefixGCD[n - 1] * prefixLCM[n - 1]; // select all elements

        // 2. go backward to calculate suffix[i] for GCD and LCM
        // 3. update the max factor score at the same time
        long suffixGCD = 0;
        long suffixLCM = 1;
        for (int i = n - 1; i >= 1; i--)
        {
            // remove one element
            long score = lcm(prefixLCM[i - 1], suffixLCM) * gcd(prefixGCD[i - 1], suffixGCD);
            ret = Math.max(ret, score);

            suffixGCD = gcd(suffixGCD, nums[i]);
            suffixLCM = lcm(suffixLCM, nums[i]);
        }
        ret = Math.max(ret, suffixGCD * suffixLCM);

        return ret;
    }

    long gcd(long a, long b)
    {
        return b == 0 ? a : gcd(b, a % b);
    }

    long lcm(long a, long b)
    {
        return a * b / gcd(a, b);
    }
}
```
---

若要計算不含`nums[i]`的`factor score`, 則需要`prefix[i-1]`與`suffix[i+1]`
```
prefix[i-1]    suffix[i+1]
[x x x x]   x  [x x x x]
            i  
```

1.  先計算GCD與LCM的`prefix[i]`
2.  由後向前計算GCD與LCM的`suffix[i]`
3.  同時, 計算`factor score`並更新最大值

**小技巧 :**
GCD初始值 = 0, LCM的初始值 = 1
`GCD(0, x) = x`
`LCM(1, x) = x`

###### tags: `Leetcode` `Dynamic Programming`