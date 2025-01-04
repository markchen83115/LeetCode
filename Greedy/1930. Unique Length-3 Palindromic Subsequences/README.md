# Leetcode - 1930. Unique Length-3 Palindromic Subsequences (M-)

[Leetcode](https://leetcode.com/problems/unique-length-3-palindromic-subsequences/)

Given a string `s`, return _the number of **unique palindromes of length three** that are a **subsequence** of _`s`.

Note that even if there are multiple ways to obtain the same subsequence, it is still only counted **once**.

A **palindrome** is a string that reads the same forwards and backwards.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

-   For example, `"ace"` is a subsequence of `"abcde"`.

**Example 1:**

> **Input:** s = "aabca"
> **Output:** 3
> **Explanation:** The 3 palindromic subsequences of length 3 are:
> - "aba" (subsequence of "aabca")
> - "aaa" (subsequence of "aabca")
> - "aca" (subsequence of "aabca")

**Example 2:**

> **Input:** s = "adc"
> **Output:** 0
> **Explanation:** There are no palindromic subsequences of length 3 in "adc".

**Example 3:**

> **Input:** s = "bbcbaba"
> **Output:** 4
> **Explanation:** The 4 palindromic subsequences of length 3 are:
> - "bbb" (subsequence of "bbcbaba")
> - "bcb" (subsequence of "bbcbaba")
> - "bab" (subsequence of "bbcbaba")
> - "aba" (subsequence of "bbcbaba")

**Constraints:**

-   `3 <= s.length <= 105`
-   `s` consists of only lowercase English letters.

---
```java
// Java 172ms(Beats 71.99%), Time O(N), Space O(N)
class Solution {
    public int countPalindromicSubsequence(String s) {
        int n = s.length();
        int ret = 0;
        for (char ch = 'a'; ch <= 'z'; ch++)
        {
            int i = s.indexOf(ch);
            int j = s.lastIndexOf(ch);
            if (j - i < 2)
                continue;
            HashSet<Character> set = new HashSet<>();
            for (int k = i + 1; k <= j - 1; k++)
                set.add(s.charAt(k));
            ret += set.size();
        }

        return ret;
    }
}
```
---
```
bbcbaba
-- -
 - - -
```
若取用的index相同, 但subsequences的結果相同, 也只算一種結果
因此我們計算從第一個出現的字母跟最後一個出現的相同字母之間, 可以組成多少個不同subsequences結果
從`a`到`z`分別跑過一遍, 便可得到答案


###### tags: `Leetcode` `Greedy`