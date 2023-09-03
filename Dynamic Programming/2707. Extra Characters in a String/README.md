# Leetcode - 2707. Extra Characters in a String (M)

[Leetcode](https://leetcode.com/problems/extra-characters-in-a-string/)

You are given a **0-indexed** string `s` and a dictionary of words `dictionary`. You have to break `s` into one or more **non-overlapping** substrings such that each substring is present in `dictionary`. There may be some **extra characters** in `s` which are not present in any of the substrings.

Return _the **minimum** number of extra characters left over if you break up _`s`_ optimally._

**Example 1:**
```
Input: s = "leetscode", dictionary = ["leet","code","leetcode"]
Output: 1
Explanation: We can break s in two substrings: "leet" from index 0 to 3 and "code" from index 5 to 8. There is only 1 unused character (at index 4), so we return 1.
```
**Example 2:**
```
Input: s = "sayhelloworld", dictionary = ["hello","world"]
Output: 3
Explanation: We can break s in two substrings: "hello" from index 3 to 7 and "world" from index 8 to 12. The characters at indices 0, 1, 2 are not used in any substring and thus are considered as extra characters. Hence, we return 3.
```
**Constraints:**

-   `1 <= s.length <= 50`
-   `1 <= dictionary.length <= 50`
-   `1 <= dictionary[i].length <= 50`
-   `dictionary[i]` and `s` consists of only lowercase English letters
-   `dictionary` contains distinct words

---
```java
// Java DP 42ms(Beats 73.84%), Time O(N^2), Space O(N)
class Solution {
    public int minExtraChar(String s, String[] dictionary) {
        int n = s.length();
        // dp[i] = end at i, the minimun of extra characters not in dictionary 
        int[] dp = new int[n + 1];
        Arrays.fill(dp, n + 1);
        dp[0] = 0;

        Set<String> set = new HashSet<String>(Arrays.asList(dictionary));

        for (int i = 1; i <= n; i++)
        {
            dp[i] = dp[i - 1] + 1;
            for (int j = 1; j <= i; j++) {
                if (set.contains(s.substring(j-1, i)))
                    dp[i] = Math.min(dp[i], dp[j-1]);
            }
        }

        return dp[n];
    }
}
```
---
`dp[i]`代表以`i`為結尾的非字典內最少字元的個數
```
x x x x l e e t x x x x x
      j       i
dp[i] = dp[j]
```


###### tags: `Leetcode` `Dynamic Programming`