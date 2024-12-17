# Leetcode - 72. Edit Distance (H-)

[Leetcode](https://leetcode.com/problems/edit-distance/)

Given two strings `word1` and `word2`, return _the minimum number of operations required to convert `word1` to `word2`_.

You have the following three operations permitted on a word:

-   Insert a character
-   Delete a character
-   Replace a character

**Example 1:**

> **Input:** word1 = "horse", word2 = "ros"
> **Output:** 3
> **Explanation:** 
> horse -> rorse (replace 'h' with 'r')
> rorse -> rose (remove 'r')
> rose -> ros (remove 'e')

**Example 2:**

> **Input:** word1 = "intention", word2 = "execution"
> **Output:** 5
> **Explanation:** 
> intention -> inention (remove 't')
> inention -> enention (replace 'i' with 'e')
> enention -> exention (replace 'n' with 'x')
> exention -> exection (replace 'n' with 'c')
> exection -> execution (insert 'u')

**Constraints:**

-   `0 <= word1.length, word2.length <= 500`
-   `word1` and `word2` consist of lowercase English letters.

---
```java
// Java 5ms(Beats 66.25%), Time O(M*N), Space O(M*N)
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();
        word1 = "#" + word1;
        word2 = "#" + word2;
        int[][] dp = new int[m+1][n+1];
        for (int i = 1; i <= m; i++)
            dp[i][0] = i;
        for (int j = 1; j <= n; j++)
            dp[0][j] = j;

        for (int i = 1; i <= m; i++)
            for (int j = 1; j <=n; j++)
            {
                if (word1.charAt(i) == word2.charAt(j))
                    dp[i][j] = dp[i-1][j-1];
                else
                    dp[i][j] = Math.min(Math.min(dp[i-1][j-1] + 1, dp[i-1][j] + 1), dp[i][j-1] + 1);
            }
        
        return dp[m][n];
    }

    // dp[i][j] = the minimum number of operations required to convert word1[0:i] to word2[0:j]

    // dp[i][j] = dp[i-1][j-1]  if word1[i] = word2[j];

    // dp[i][j] = dp[i-1][j-1] + 1 (replace)
    //          = dp[i][j-1] + 1 (insert)
    //          = dp[i-1][j] + 1 (delete)
}
```
---

`dp[i][j] = the minimum number of operations required to convert word1[0:i] to word2[0:j]`

如果`word1[i] == word2[j]` 則 `dp[i][j] = dp[i-1][j-1]`

如果`word1[i] != word2[j]` 則
```
dp[i][j] = dp[i-1][j-1] + 1 (replace)
         = dp[i][j-1] + 1 (insert)
         = dp[i-1][j] + 1 (delete)
```

###### tags: `Leetcode` `Dynamic Programming` `雙序列型`