# Leetcode - 010. Regular Expression Matching (H)

[Leetcode](https://leetcode.com/problems/regular-expression-matching/)

Given an input string `s` and a pattern `p`, implement regular expression matching with support for `'.'` and `'*'` where:

-   `'.'` Matches any single character.
-   `'*'` Matches zero or more of the preceding element.

The matching should cover the **entire** input string (not partial).

**Example 1:**

> **Input:** s = "aa", p = "a"
> **Output:** false
> **Explanation:** "a" does not match the entire string "aa".

**Example 2:**

> **Input:** s = "aa", p = "a*"
> **Output:** true
> **Explanation:** '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".

**Example 3:**

> **Input:** s = "ab", p = ".*"
> **Output:** true
> **Explanation:** ".*" means "zero or more (*) of any character (.)".

**Constraints:**

-   `1 <= s.length <= 20`
-   `1 <= p.length <= 20`
-   `s` contains only lowercase English letters.
-   `p` contains only lowercase English letters, `'.'`, and `'*'`.
-   It is guaranteed for each appearance of the character `'*'`, there will be a previous valid character to match.

---
```java
// Java 2ms(Beats 65.10%), Time O(MN), Space O(MN)
class Solution {
    public boolean isMatch(String s, String p) {
        int m = s.length();
        int n = p.length();
        boolean[][] dp = new boolean[m+1][n+1];
        s = "#" + s;
        p = "#" + p;
        char[] a = s.toCharArray();
        char[] b = p.toCharArray();

        dp[0][0] = true;
        for (int j = 2; j <=n ;j+=2)                // a: 
            dp[0][j] = dp[0][j-2] && b[j] == '*';   // b: Y * Y * Y * Y *

        for (int i = 1; i <= m; i++)
            for (int j = 1; j <= n; j++)
            {
                if (Character.isLetter(b[j]))
                    dp[i][j] = a[i] == b[j] && dp[i-1][j-1];
                else if (b[j] == '.')
                    dp[i][j] = dp[i-1][j-1];
                else if (b[j] == '*')
                {
                    boolean possible1 = dp[i][j-2];
                    boolean possible2 = (a[i] == b[j-1] || b[j-1] == '.') && dp[i-1][j-2];
                    boolean possible3 = (a[i] == b[j-1] || b[j-1] == '.') && dp[i-1][j];
                    dp[i][j] = possible1 || possible2 || possible3;
                }
            }

        return dp[m][n];
    }
```
---

參考[【每日一题】10. Regular Expression Matching, 05/05/2019 - YouTube](https://youtu.be/qWxLyexGW1k)


透過雙序列型`dp[i][j]`: `s[1:i]`與`p[1:j]`相同, 1-indexed
動態轉移方程:
1. 如果`p[j]`是字母，那`s[i]`必須是其相同的字母
```java
dp[i][j] = (a[i] == b[j]) && dp[i-1][j-1]
```
2. 如果`p[j]`是`.`, 那`s[i]`為任何字母都可以
```java
dp[i][j] = dp[i-1][j-1]
```
3. 如果`p[j]`是`*`, 那分成以下狀況

	(1) `Z*`代表空字串
	```java
	// X X X X X X X
	// Y Y Y Y Z *

	dp[i][j] = dp[i][j-2]
	```
	(2) `Z*`代表一個Z
	```java
	// X X X X X X Z
	// Y Y Y Y Z *

	dp[i][j] = (a[i] == b[j-1] || b[j-1] == '.') && dp[i-1][j-2]
	```
	(3) `Z*`代表多個Z
	```java
	// X X X X X X Z
	// Y Y Y Y Z *

	dp[i][j] = (a[i] == b[j-1] || b[j-1] == '.') && dp[i-1][j]
	```
	

###### tags: `Leetcode` `Dynamic Programming` `雙序列型`