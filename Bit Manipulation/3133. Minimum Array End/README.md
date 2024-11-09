# Leetcode - 3133. Minimum Array End (M+)

[Leetcode](https://leetcode.com/problems/minimum-array-end/)

You are given two integers `n` and `x`. You have to construct an array of **positive** integers `nums` of size `n` where for every `0 <= i < n - 1`, `nums[i + 1]` is **greater than** `nums[i]`, and the result of the bitwise `AND` operation between all elements of `nums` is `x`.

Return the **minimum** possible value of `nums[n - 1]`.

**Example 1:**

**Input:** n = 3, x = 4

**Output:** 6

**Explanation:**

`nums` can be `[4,5,6]` and its last element is 6.

**Example 2:**

**Input:** n = 2, x = 7

**Output:** 15

**Explanation:**

`nums` can be `[7,15]` and its last element is 15.

**Constraints:**

-   `1 <= n, x <= 10^8`

---
```java
// Java 0ms(Beats 100.00%), Time O(logN), Space O(N)
class Solution {
    public long minEnd(int n, int x) {
        long ret = x;
        for (long bitX = 1, bitN = 1; bitN < n; bitX <<= 1)
        {
            /*
            100110 x
            100100 n
            */
            if ((x & bitX) == 0)             // x的bit = 0
            {            
                if ((bitN & (n - 1)) > 0)    // n - 1的bit = 1
                    ret += bitX;

                bitN <<= 1;
            }
        }
        return ret;
    }
}
```
---

參考[【每日一题】LeetCode 3133. Minimum Array End - YouTube](https://youtu.be/qjo7da3eNDQ)

![未命名](https://hackmd.io/_uploads/HkEF6L3Zyg.jpg)
將x進行二進位分解, 保持屬於1的bit位不變
因此可以組成長度為n的最小序列：`0 1 2 ... n-1`
其中將最大元素`n-1`進行二進位拆解，但是填充在x的那些屬於0的bit位上
這樣就得到答案



###### tags: `Leetcode` `Bit Manipulation`