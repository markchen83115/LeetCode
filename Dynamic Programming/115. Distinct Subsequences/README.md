# Leetcode - 115. Distinct Subsequences (H-)

[Leetcode](https://leetcode.com/problems/distinct-subsequences/)

Given two strings s and t, return _the number of distinct_ **_subsequences_**_ of _s_ which equals _t.

The test cases are generated so that the answer fits on a 32-bit signed integer.

**Example 1:**

> **Input:** s = "rabbbit", t = "rabbit"
> **Output:** 3
> **Explanation:**
> As shown below, there are 3 ways you can generate "rabbit" from s.
> `**rabb**b**it**`
> `**ra**b**bbit**`
> `**rab**b**bit**`

**Example 2:**

> **Input:** s = "babgbag", t = "bag"
> **Output:** 5
> **Explanation:**
> As shown below, there are 5 ways you can generate "bag" from s.
> `**ba**b**g**bag`
> `**ba**bgba**g**`
> `**b**abgb**ag**`
> `ba**b**gb**ag**`
> `babg**bag**`

**Constraints:**

-   `1 <= s.length, t.length <= 1000`
-   `s` and `t` consist of English letters.

---
```java
// Java 19ms(Beats 59.35%), Time O(MN), Space O(MN)
class Solution {
    public int numDistinct(String s, String t) {
        int m = s.length();
        int n = t.length();
        int[][] dp = new int[m+1][n+1];
        s = "#" + s;
        t = "#" + t;
        
        for (int i = 0; i <= m; i++)
            dp[i][0] = 1;


        for (int i = 1; i <= m; i++)
            for (int j = 1; j <= n; j++)
            {
                dp[i][j] += dp[i-1][j];
                if (s.charAt(i) == t.charAt(j))
                    dp[i][j] += dp[i-1][j-1];
            }

        return dp[m][n];
    }
}
```
```java
// Java 15ms(Beats 80.55%), Time O(MN), Space O(MN)
class Solution {
    public int numDistinct(String s, String t) {
        int m = s.length();
        int n = t.length();

        if (m < n)
            return 0;
        if (m == n)
            return s.equals(t) ? 1 : 0;

        int[][] dp = new int[m+1][n+1];
        s = "#" + s;
        t = "#" + t;
        
        for (int i = 0; i <= m; i++)
            dp[i][0] = 1;


        for (int i = 1; i <= m; i++)
            for (int j = 1; j <= n; j++)
            {
                dp[i][j] += dp[i-1][j];
                if (s.charAt(i) == t.charAt(j))
                    dp[i][j] += dp[i-1][j-1];
            }

        return dp[m][n];
    }
}
```
---

當雙序列型的題目做多了之後, 很明顯知道這屬於雙序列型的套路
`dp[i][j]`: `s[1:i]`有多少個子序列的相同於`t[1:j]`

動態轉移方程:
```
dp[i][j] += dp[i-1][j]
```

當`s[i] == t[j]`
```
dp[i][j] += dp[i-1][j-1]
```

邊界條件:
`dp[0~m][0]`皆為`1`
因為用`s[0:i]`個字母組成`t`的空字串, 都只有`1`種組合

###### tags: `Leetcode` `Dynamic Programming` `雙序列型`