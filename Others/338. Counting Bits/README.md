# Leetcode - 338. Counting Bits (E)

[Leetcode](https://leetcode.com/problems/counting-bits/)

Given an integer `n`, return _an array _`ans`_ of length _`n + 1`_ such that for each _`i` (`0 <= i <= n`)_, _`ans[i]`_ is the **number of **_`1`_**'s** in the binary representation of _`i`.

**Example 1:**
```
Input: n = 2
Output: [0,1,1]
Explanation:
0 --> 0
1 --> 1
2 --> 10
```
**Example 2:**
```
Input: n = 5
Output: [0,1,1,2,1,2]
Explanation:
0 --> 0
1 --> 1
2 --> 10
3 --> 11
4 --> 100
5 --> 101
```
**Constraints:**

-   `0 <= n <= 105`

**Follow up:**

-   It is very easy to come up with a solution with a runtime of `O(n log n)`. Can you do it in linear time `O(n)` and possibly in a single pass?
-   Can you do it without using any built-in function (i.e., like `__builtin_popcount` in C++)?

---
```java
// Java 4ms(Beats 28.80%), Time O(NlogN), Space O(N)
class Solution {
    public int[] countBits(int n) {
        int[] ret = new int[n + 1];
        for (int i = 0; i <= n; i++) {
            int count = 0;
            int x = i;
            while (x > 0) {
                count += (x & 1);
                x /= 2;
            }
            ret[i] = count;
        }
        return ret;
    }
}
```

```java
// Java DP 2ms(Beats 78.84%), Time O(N), Space O(N)
class Solution {
    public int[] countBits(int n) {
        int[] ret = new int[n + 1];
        ret[0] = 0;
        for (int i = 1; i <= n; i++)
            ret[i] = ret[i / 2] + (i & 1);
        
        return ret;
    }
}
```
---


###### tags: `Leetcode` `Others` `位元計算`