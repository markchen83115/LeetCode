# Leetcode - 1639. Number of Ways to Form a Target String Given a Dictionary (H-)

[Leetcode](https://leetcode.com/problems/number-of-ways-to-form-a-target-string-given-a-dictionary/)

You are given a list of strings of the **same length** `words` and a string `target`.

Your task is to form `target` using the given `words` under the following rules:

-   `target` should be formed from left to right.
-   To form the `ith` character (**0-indexed**) of `target`, you can choose the `kth` character of the `jth` string in `words` if `target[i] = words[j][k]`.
-   Once you use the `kth` character of the `jth` string of `words`, you **can no longer** use the `xth` character of any string in `words` where `x <= k`. In other words, all characters to the left of or at index `k` become unusuable for every string.
-   Repeat the process until you form the string `target`.

**Notice** that you can use **multiple characters** from the **same string** in `words` provided the conditions above are met.

Return _the number of ways to form `target` from `words`_. Since the answer may be too large, return it **modulo** `109 + 7`.

**Example 1:**

> **Input:** words = ["acca","bbbb","caca"], target = "aba"
> **Output:** 6
> **Explanation:** There are 6 ways to form target.
> "aba" -> index 0 ("acca"), index 1 ("bbbb"), index 3 ("caca")
> "aba" -> index 0 ("acca"), index 2 ("bbbb"), index 3 ("caca")
> "aba" -> index 0 ("acca"), index 1 ("bbbb"), index 3 ("acca")
> "aba" -> index 0 ("acca"), index 2 ("bbbb"), index 3 ("acca")
> "aba" -> index 1 ("caca"), index 2 ("bbbb"), index 3 ("acca")
> "aba" -> index 1 ("caca"), index 2 ("bbbb"), index 3 ("caca")

**Example 2:**

> **Input:** words = ["abba","baab"], target = "bab"
> **Output:** 4
> **Explanation:** There are 4 ways to form target.
> "bab" -> index 0 ("baab"), index 1 ("baab"), index 2 ("abba")
> "bab" -> index 0 ("baab"), index 1 ("baab"), index 3 ("baab")
> "bab" -> index 0 ("baab"), index 2 ("baab"), index 3 ("baab")
> "bab" -> index 1 ("abba"), index 2 ("baab"), index 3 ("baab")

**Constraints:**

-   `1 <= words.length <= 1000`
-   `1 <= words[i].length <= 1000`
-   All strings in `words` have the same length.
-   `1 <= target.length <= 1000`
-   `words[i]` and `target` contain only lowercase English letters.

---
```java
// Java 50ms(Beats 44.69%), Time O(NM), Space O(NM)
class Solution {
    public int numWays(String[] words, String target) {
        // dp[i][k] : how many ways to form target[0:i] by using words[0:k]
        int n = target.length();
        int m = words[0].length();
		long[][] dp = new long[n+1][m+1];	// long[1005][1005];
        int[][] count = new int[m+1][26];	// int[1005][26];
        int mod = (int) 1e9 + 7;

        target = "#" + target;

        // count[k][a-z]
        for (String word : words)
            for (int k = 0; k < m; k++)
                count[k + 1][word.charAt(k) - 'a']++;   // to fit 1-indexed, use k+1
        
        // dp[0][k]
        for (int k = 0; k <= m; k++)
            dp[0][k] = 1;

        for (int i = 1; i <= n; i++)
            for (int k = 1; k <= m; k++)
            {
                dp[i][k] = dp[i][k - 1];    // 1. not use words[k] to form target[i]

                if (count[k][target.charAt(i) - 'a'] > 0)   // 2. use words[k] to form target[i]
                    dp[i][k] += dp[i - 1][k - 1] * count[k][target.charAt(i) - 'a'] % mod;
                dp[i][k] %= mod;
            }
        
        
        return (int) dp[n][m];
    }
}
```
---

參考[【每日一题】1639. Number of Ways to Form a Target String Given a Dictionary, 11/2/2020 - YouTube](https://youtu.be/KDOyzVIAa9w)

定義`dp[i][k] : how many ways to form target[0:i] by using words[0:k]`

動態轉移方程 :
1. `target[0:i]`不使用`words`的第`k`個字母 :
    ```
	dp[i][k] = dp[i][k-1]
	```

2. 若`words`的第`k`個字母與`target`的第`i`個字母相同, 使用`words`的第`k`個字母來組成`target[0:i]` :
    ```
	if (can do)
        dp[i][j] = dp[i-1][k-1] * count(how many words[k] == target[i])
	```


###### tags: `Leetcode` `Dynamic Programming`