# Leetcode - 1016. Binary String With Substrings Representing 1 To N (M)

[Leetcode](https://leetcode.com/problems/binary-string-with-substrings-representing-1-to-n/)

Given a binary string `s` and a positive integer `n`, return `true`_ if the binary representation of all the integers in the range _`[1, n]`_ are **substrings** of _`s`_, or _`false`_ otherwise_.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

> **Input:** s = "0110", n = 3
> **Output:** true

**Example 2:**

> **Input:** s = "0110", n = 4
> **Output:** false

**Constraints:**

-   `1 <= s.length <= 1000`
-   `s[i]` is either `'0'` or `'1'`.
-   `1 <= n <= 109`

---
```java
// Java 0ms(Beats 100.00%), Time O(N^2), Space O(1)
class Solution {
    public boolean queryString(String s, int n) {
        for (int i = n; i > n / 2; i--)
            if (!s.contains(Integer.toBinaryString(i)))
                return false;
        return true;
    }
}
```
---

以`n = 16`為例
```
16 = 10000,  8 = 1000
15 = 1111,   7 = 111
14 = 1110,   7 = 111
13 = 1101,   6 = 110
12 = 1100,   6 = 110
11 = 1011,   5 = 101
10 = 1010,   5 = 101
9  = 1001,   4 = 100
```
若s有含16, 則必定含有8
若s有含15, 則必定含有7
若s有含14, 則必定含有7...以此類推

因此我們只需要檢查`n`~`n/2 + 1`即可



###### tags: `Leetcode` `Greedy`