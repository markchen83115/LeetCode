# Leetcode - 097. Interleaving String (H-)

[Leetcode](https://leetcode.com/problems/interleaving-string/)

Given strings `s1`, `s2`, and `s3`, find whether `s3` is formed by an **interleaving** of `s1` and `s2`.

An **interleaving** of two strings `s` and `t` is a configuration where `s` and `t` are divided into `n` and `m`  

substrings

 respectively, such that:

-   `s = s1 + s2 + ... + sn`
-   `t = t1 + t2 + ... + tm`
-   `|n - m| <= 1`
-   The **interleaving** is `s1 + t1 + s2 + t2 + s3 + t3 + ...` or `t1 + s1 + t2 + s2 + t3 + s3 + ...`

**Note:** `a + b` is the concatenation of strings `a` and `b`.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2020/09/02/interleave.jpg)
> 
> **Input:** s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
> **Output:** true
> **Explanation:** One way to obtain s3 is:
> Split s1 into s1 = "aa" + "bc" + "c", and s2 into s2 = "dbbc" + "a".
> Interleaving the two splits, we get "aa" + "dbbc" + "bc" + "a" + "c" = "aadbbcbcac".
> Since s3 can be obtained by interleaving s1 and s2, we return true.

**Example 2:**

> **Input:** s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
> **Output:** false
> **Explanation:** Notice how it is impossible to interleave s2 with any other string to obtain s3.

**Example 3:**

> **Input:** s1 = "", s2 = "", s3 = ""
> **Output:** true

**Constraints:**

-   `0 <= s1.length, s2.length <= 100`
-   `0 <= s3.length <= 200`
-   `s1`, `s2`, and `s3` consist of lowercase English letters.

**Follow up:** Could you solve it using only `O(s2.length)` additional memory space?

---
```java
// Java 4ms(Beats 51.63%), Time O(N + M), Space O(N * M)
class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        int n1 = s1.length(), n2 = s2.length();
        if (n1 + n2 != s3.length())
            return false;
        
        boolean[][] dp = new boolean[n1 + 1][n2 + 1];
        s1 = "#" + s1;
        s2 = "#" + s2;
        s3 = "#" + s3;

        for (int i = 0; i <= n1; i++)
            if (s1.charAt(i) == s3.charAt(i))
                dp[i][0] = true;
            else
                break;

        for (int j = 0; j <= n2; j++)
            if (s2.charAt(j) == s3.charAt(j))
                dp[0][j] = true;
            else
                break;

        for (int i = 1; i <= n1; i++)
            for (int j = 1; j <= n2; j++)
            {
                if (dp[i - 1][j] && s1.charAt(i) == s3.charAt(i + j))
                    dp[i][j] = true;
                if (dp[i][j - 1] && s2.charAt(j) == s3.charAt(i + j))
                    dp[i][j] = true;
            }

        return dp[n1][n2];
    }
}
```
---

一開始先判斷長度s1 + s2 == s3

dp[i][j] = s1的前i個字母、s2的前j個字母、s3的前i+j個字母, 是否滿足交叉子串的關係
動態轉移方程為:
```java
for (int i = 1; i <= n1; i++)
    for (int j = 1; j <= n2; j++)
    {
        if (dp[i - 1][j] && s1.charAt(i) == s3.charAt(i + j))
            dp[i][j] = true;
        if (dp[i][j - 1] && s2.charAt(j) == s3.charAt(i + j))
            dp[i][j] = true;
    }
```

接著處理初始值
dp[i][0] = s1取前i個字母是否與s3前i個字母相同
```java
for (int i = 0; i <= n1; i++)
    if (s1.charAt(i) == s3.charAt(i))
        dp[i][0] = true;
    else
        break;
```


###### tags: `Leetcode` `Dynamic Programming` `雙序列型`