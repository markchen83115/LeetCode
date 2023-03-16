# Leetcode - 2571. Minimum Operations to Reduce an Integer to 0 (H-)

[Leetcode](https://leetcode.com/problems/minimum-operations-to-reduce-an-integer-to-0/description/)

You are given a positive integer `n`, you can do the following operation **any** number of times:

-   Add or subtract a **power** of `2` from `n`.

Return _the **minimum** number of operations to make _`n`_ equal to _`0`.

A number `x` is power of `2` if `x == 2i` where `i >= 0`_._

**Example 1:**
```
Input: n = 39
Output: 3
Explanation: We can do the following operations:
- Add 20 = 1 to n, so now n = 40.
- Subtract 23 = 8 from n, so now n = 32.
- Subtract 25 = 32 from n, so now n = 0.
It can be shown that 3 is the minimum number of operations we need to make n equal to 0.
```
**Example 2:**
```
Input: n = 54
Output: 3
Explanation: We can do the following operations:
- Add 21 = 2 to n, so now n = 56.
- Add 23 = 8 to n, so now n = 64.
- Subtract 26 = 64 from n, so now n = 0.
So the minimum number of operations is 3.
```
**Constraints:**

-   `1 <= n <= 105`

---
```java
// Java
class Solution {
    public int minOperations(int n) {
        int ret = 0;
        while (n > 0) {
            if ((n & 3) == 3) { // 3 => 11, checking multiple 1
                ret++;
                n++;
            } else {
                ret += (n & 1); // if 1 => n-1, if 0 => n-0
                n -= (n & 1);
            }
            n >>= 1;
        }
        return ret;
        
        /*
        15: 1111
        27: 11011
        38: 100110
        39: 100111
        54: 110110
        */
    }
}

```
---

當尾巴為連續多個1時, 直接加1, 因此檢查`(n & 3) == 3`
當尾巴只有一個１時, 直接減1, 因此檢查`(n & 1) == 1`


###### tags: `Leetcode` `Math`
