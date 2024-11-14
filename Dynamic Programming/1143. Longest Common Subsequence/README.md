# Leetcode - 1143. Longest Common Subsequence (M)

[Leetcode](https://leetcode.com/problems/longest-common-subsequence/)

Given two strings `text1` and `text2`, return _the length of their longest **common subsequence**. _If there is no **common subsequence**, return `0`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

-   For example, `"ace"` is a subsequence of `"abcde"`.

A **common subsequence** of two strings is a subsequence that is common to both strings.

**Example 1:**

> **Input:** text1 = "abcde", text2 = "ace" 
> **Output:** 3  
> **Explanation:** The longest common subsequence is "ace" and its length is 3.

**Example 2:**

> **Input:** text1 = "abc", text2 = "abc"
> **Output:** 3
> **Explanation:** The longest common subsequence is "abc" and its length is 3.

**Example 3:**

> **Input:** text1 = "abc", text2 = "def"
> **Output:** 0
> **Explanation:** There is no such common subsequence, so the result is 0.

**Constraints:**

-   `1 <= text1.length, text2.length <= 1000`
-   `text1` and `text2` consist of only lowercase English characters.

---
```java
// Java 19ms(Beats 89.08%), Time O(MN), Space O(MN)
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length();
        int n = text2.length();
        text1 = "#" + text1;
        text2 = "#" + text2;
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 1; i <= m; i++)
            for (int j = 1; j <= n; j++)
            {
                if (text1.charAt(i) == text2.charAt(j))
                    dp[i][j] = dp[i-1][j-1] + 1;
                else
                    dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
            }

        return dp[m][n];
    }
}
// xxxxx i
// yyyy  j
```
---

參考[【每日一题】1143. Longest Common Subsequence, 3/30/2020 - YouTube](https://youtu.be/CEnb7Ho7TYc)

![image](https://hackmd.io/_uploads/r1MAgs7zJl.png)

###### tags: `Leetcode` `Dynamic Programming` `雙序列型`