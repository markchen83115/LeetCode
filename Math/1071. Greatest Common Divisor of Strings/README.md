## Leetcode - 1071. Greatest Common Divisor of Strings (M)

[Leetcode](https://leetcode.com/problems/greatest-common-divisor-of-strings/description/)

For two strings `s` and `t`, we say "`t` divides `s`" if and only if `s = t + ... + t` (i.e., `t` is concatenated with itself one or more times).

Given two strings `str1` and `str2`, return _the largest string _`x`_ such that _`x`_ divides both _`str1`_ and _`str2`.

**Example 1:**
```
Input: str1 = "ABCABC", str2 = "ABC"
Output: "ABC"
```
**Example 2:**
```
Input: str1 = "ABABAB", str2 = "ABAB"
Output: "AB"
```
**Example 3:**
```
Input: str1 = "LEET", str2 = "CODE"
Output: ""
```
**Constraints:**

-   `1 <= str1.length, str2.length <= 1000`
-   `str1` and `str2` consist of English uppercase letters.

---

```java
class Solution {
    public String gcdOfStrings(String str1, String str2) {
        if (!(str1 + str2).equals(str2 + str1))
            return "";
        int gcdLen = gcd(str1.length(), str2.length());
        return str1.substring(0, gcdLen);
    }

    public int gcd(int x, int y) {
        if (y == 0)
            return x;
        return gcd(y, x % y);
    }
}
```

---

1. 檢查是否有GCD string
![](https://i.imgur.com/19tK2bY.png)

2. 求出GCD string長度, 答案為`substring(0, gcdLength)`


###### tags: `Leetcode` `Math`
