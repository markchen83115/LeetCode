# Leetcode - 1092. Shortest Common Supersequence (H-)

[Leetcode](https://leetcode.com/problems/shortest-common-supersequence/)

Given two strings `str1` and `str2`, return _the shortest string that has both _`str1`_ and _`str2`_ as **subsequences**_. If there are multiple valid strings, return **any** of them.

A string `s` is a **subsequence** of string `t` if deleting some number of characters from `t` (possibly `0`) results in the string `s`.

**Example 1:**

> **Input:** str1 = "abac", str2 = "cab"
> **Output:** "cabac"
> **Explanation:** 
> str1 = "abac" is a subsequence of "cabac" because we can delete the first "c".
> str2 = "cab" is a subsequence of "cabac" because we can delete the last "ac".
> The answer provided is the shortest such string that satisfies these properties.

**Example 2:**

> **Input:** str1 = "aaaaaaaa", str2 = "aaaaaaaa"
> **Output:** "aaaaaaaa"

**Constraints:**

-   `1 <= str1.length, str2.length <= 1000`
-   `str1` and `str2` consist of lowercase English letters.

---
```java
// Java 22ms(Beats 64.51%), Time O(M*N), Space O(M*N)
class Solution {
    public String shortestCommonSupersequence(String str1, String str2) {
        int m = str1.length(), n = str2.length();
        str1 = "#" + str1;
        str2 = "#" + str2;
        int[][] dp = new int[m+1][n+1];
        // dp[i][j] = length of Shortest Common Supersequence for str1[0:i] and str2[0:j]

        // initial
        for (int i = 1; i <= m; i++)
            dp[i][0] = i;
        for (int j = 1; j <= n; j++)
            dp[0][j] = j;

        // DP
        for (int i = 1; i <= m; i++)
            for (int j = 1; j <= n; j++)
            {
                if (str1.charAt(i) == str2.charAt(j))
                    dp[i][j] = dp[i-1][j-1] + 1;
                else
                    dp[i][j] = Math.min(dp[i-1][j] + 1, dp[i][j-1] + 1);
            }

        // restore Shortest Common Supersequence
        StringBuilder sb = new StringBuilder();
        int i = m, j = n;
        while (i > 0 && j > 0)
        {
            if (str1.charAt(i) == str2.charAt(j))
            {
                sb.append(str1.charAt(i));
                i--;
                j--;
            }
            else if (dp[i][j] == dp[i-1][j] + 1)
            {
                sb.append(str1.charAt(i));
                i--;
            }
            else
            {
                sb.append(str2.charAt(j));
                j--;
            }
        }
        while (i > 0)
            sb.append(str1.charAt(i--));
        while (j > 0)
            sb.append(str2.charAt(j--));

        return sb.reverse().toString();
    }
}
```
```java
// Java LCS 21ms(Beats 85.00%), Time O(M*N), Space O(M*N)
class Solution {
    public String shortestCommonSupersequence(String str1, String str2) {
        int m = str1.length(), n = str2.length();
        str1 = "#" + str1;
        str2 = "#" + str2;
        int[][] dp = new int[m+1][n+1];
        // dp[i][j] = LCS for str1[0:i] and str2[0:j]

        // LCS
        for (int i = 1; i <= m; i++)
            for (int j = 1; j <= n; j++)
            {
                if (str1.charAt(i) == str2.charAt(j))
                    dp[i][j] = dp[i-1][j-1] + 1;
                else
                    dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
            }

        // restore Shortest Common Supersequence
        StringBuilder sb = new StringBuilder();
        int i = m, j = n;
        while (i > 0 && j > 0)
        {
            if (str1.charAt(i) == str2.charAt(j))
            {
                sb.append(str1.charAt(i));
                i--;
                j--;
            }
            else if (dp[i][j] == dp[i-1][j])
            {
                sb.append(str1.charAt(i));
                i--;
            }
            else
            {
                sb.append(str2.charAt(j));
                j--;
            }
        }
        while (i > 0)
            sb.append(str1.charAt(i--));
        while (j > 0)
            sb.append(str2.charAt(j--));

        return sb.reverse().toString();
    }
}
```
---

參考[【每日一题】1092. Shortest Common Supersequence, 3/28/2020 - YouTube](https://youtu.be/Uk9JRbylA0c)


#### 解法1：直接考慮“最短公共超串”

如果只是問Shortest-Common-Supersequence的長度,那麼就是一道非常基本的dp題,典型的"two string conversion"的套路.

現在要求印出這樣的Shortest-Common-Supersequence,無非就是從dp[M][N]逆著往回走,每一步我們都需要判斷dp[i][j]之前的狀態是什麼?其實就是重複一遍給dp賦值的邏輯:

1. 如果dp[i][j]是由dp[i-1][j-1]+1得來,也即是說str1[i]==str2[j]，那麼就意味著當時在建構SCS的時候，最後一步是在dp[i-1][j-1str;
2. 如果dp[i][j]是由dp[i-1][j]+1得來,那麼說明當時是在dp[i-1][j]的基礎上加上str1[i]，現在逆序重構的時候需要先加上str1[i].
3. 如果dp[i][j]是由dp[i][j-1]+1得來,那麼說明當時是在dp[i][j-1]的基礎上加上str2[j]，現在逆序重構的時候需要先加上str2[j].

這樣一直回退到`i==0 && j==0`


#### 解法2：直接考慮“最長公共子字串”

此題目也可以先解決一個最長公共子字串問題（LCS），求得二維數組dp[i][j]，再根據這個dp的定義來重構超級字串。

同樣，我們逆序構造這個超串：

1. 如果dp[i][j]是由dp[i-1][j-1]+1得來,也即是說str1[i]==str2[j]，那麼就表示當時在建構SCS的時候，是在101i-1][j-1]的基礎上加上``12[
2. 如果dp[i][j]是由dp[i-1][j]得來,也即是說str1[i]不是屬於這個LCS的一部分，那麼現在構造SCS的時候，先把它加上。
3. 如果dp[i][j]是由dp[i][j-1]得來,也即是說str2[j]不是屬於這個LCS的一部分，那麼現在構造SCS的時候，先把它加上。



###### tags: `Leetcode` `Dynamic Programming` `雙序列型`