# Leetcode - 647. Palindromic Substrings (M+)

[Leetcode](https://leetcode.com/problems/palindromic-substrings/)

Given a string `s`, return _the number of **palindromic substrings** in it_.

A string is a **palindrome** when it reads the same backward as forward.

A **substring** is a contiguous sequence of characters within the string.

**Example 1:**

> **Input:** s = "abc"
> **Output:** 3
> **Explanation:** Three palindromic strings: "a", "b", "c".

**Example 2:**

> **Input:** s = "aaa"
> **Output:** 6
> **Explanation:** Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".

**Constraints:**

-   `1 <= s.length <= 1000`
-   `s` consists of lowercase English letters.

---
```java
// Java Manacher 9ms(Beats 47.36%), Time O(N), Space O(N)
class Solution {
    public int countSubstrings(String s) {
        String t = "#";
        for (int i = 0; i < s.length(); i++)
            t += s.charAt(i) + "#";
        int n = t.length();
        int[] P = new int[n];

        int mc = -1;
        int mr = -1;
        for (int i = 0; i < n; i++)
        {
            int r = 0;
            if (i < mr)
            {
                int j = 2 * mc - i;
                r = Math.min(P[j], mr - i);
            }

            while (i - r >= 0 && i + r < n && t.charAt(i - r) == t.charAt(i + r))
                r++;

            P[i] = r - 1;

            if (i + P[i] > mr)
            {
                mr = i + P[i];
                mc = i;
            }
        }

        int ret = 0;
        for (int i = 0; i < n; i++)
            ret += (P[i] + 1) / 2;
        
        return ret;
    }
}

// x {x x x x x x x x x x x} x 
// --------------
//        j     mc    i   mr

// # a # b # b # a #
// # a # b # a #

```
```java
// Java Manacher 6ms(Beats 72.92%), Time O(N ^ 2), Space O(N)
class Solution {
    public int countSubstrings(String s) {
        int ret = 0;
        int n = s.length();
        for (int i = 0; i < n; i++)
        {
            ret += extendedPalindrom(i, i, s);
            ret += extendedPalindrom(i, i + 1, s);
        }

        return ret;
    }

    int extendedPalindrom(int i, int j, String s)
    {
        int count = 0;
        while (i >= 0 && j < s.length() && s.charAt(i) == s.charAt(j))
        {
            count++;
            i--;
            j++;
        }

        return count;
    }
}
```
---

#### 解法1：暴力

考察以i為中心的回文串個數(回文串長度為奇數)
以i和i+1為中心的回文串個數(回文串長為偶數)

考察方法是以中心i往兩邊擴散。一旦發現無法實現中心對稱，則可以跳過這個i
整體的時間複雜度是O(N^2)

#### 解法2：Manacher

此題是Manacher的模板題
注意! 求出T串的`P[i]`之後每一個中心點的最長回文串半徑（去除#字符）其實是`(P[i]+1)/2`


###### tags: `Leetcode` `String` `Manacher`