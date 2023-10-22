# Leetcode - 7. Reverse Integer (M)

[Leetcode](https://leetcode.com/problems/reverse-integer/)

Given a signed 32-bit integer `x`, return `x`_ with its digits reversed_. If reversing `x` causes the value to go outside the signed 32-bit integer range `[-231, 231 - 1]`, then return `0`.

**Assume the environment does not allow you to store 64-bit integers (signed or unsigned).**

**Example 1:**
```
Input: x = 123
Output: 321
```
**Example 2:**
```
Input: x = -123
Output: -321
```
**Example 3:**
```
Input: x = 120
Output: 21
```
**Constraints:**

-   `-231 <= x <= 231 - 1`

---
```java
// Java 0ms(Beats 100.00%), Time O(N), Space O(1)
class Solution {
    public int reverse(int x) {
        int ret = 0;
        while (x != 0)
        {
            int num = x % 10;
            int newRet = ret * 10 + num;
            if ((newRet - num) / 10 != ret)
                return 0;

            ret = ret * 10 + num;
            x /= 10;
        }

        return ret;
    }
}
```

```java
// Java 1ms(Beats 97.75%), Time O(N), Space O(N)
class Solution {
    public int reverse(int x) {
        int ret = 0;
        while (x != 0)
        {
            if (ret > 214748364 || ret < -214748364)
                return 0;

            ret = ret * 10 + x % 10;
            x /= 10;
        }

        return ret;
    }
}
```

---

###### tags: `Leetcode` `Others`