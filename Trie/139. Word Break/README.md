# Leetcode - 139. Word Break (M+)

[Leetcode](https://leetcode.com/problems/word-break/description/)

Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

**Example 1:**
```
Input: s = "leetcode", wordDict = ["leet","code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```
**Example 2:**
```
Input: s = "applepenapple", wordDict = ["apple","pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
Note that you are allowed to reuse a dictionary word.
```
**Example 3:**
```
Input: s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
Output: false
```
**Constraints:**

-   `1 <= s.length <= 300`
-   `1 <= wordDict.length <= 1000`
-   `1 <= wordDict[i].length <= 20`
-   `s` and `wordDict[i]` consist of only lowercase English letters.
-   All the strings of `wordDict` are **unique**.

---
```java
// Java 2ms(Beats 95.77%), Time O(N^2+M*K), Space O(N+M*K)
class Solution {
    class TrieNode {
        public boolean isEnd;
        public TrieNode[] next = new TrieNode[26];
    }

    TrieNode root = new TrieNode();
    int[] memo = new int[301];

    public boolean wordBreak(String s, List<String> wordDict) {
        for (String word : wordDict) {
            TrieNode node = root;
            for (char c : word.toCharArray()) {
                int index = c - 'a';
                if (node.next[index] == null)
                    node.next[index] = new TrieNode();
                node = node.next[index];
            }
            node.isEnd = true;
        }

        return dfs(s, 0);
    }

    public boolean dfs(String s, int curr) {
        if (curr == s.length())
            return true;
        if (memo[curr] == 1)
            return false;
        TrieNode node = root;
        for (int i = curr; i < s.length(); i++) {
            int index = s.charAt(i) - 'a';
            if (node.next[index] == null) {
                memo[curr] = 1;
                break;
            } else {
                node = node.next[index];
                if (node.isEnd && dfs(s, i + 1))
                    return true;
            }
        }
        return false;
    }
}
```

```java
// Java 9ms(Beats 32.18%)
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        // dp[i] = dp[j] && s[j:i] is word
        boolean[] dp = new boolean[s.length() + 1];
        Set<String> set = new HashSet<>();
        set.addAll(wordDict);

        dp[0] = true;
        for (int i = 1; i <= s.length(); i++) {
            for (int j = 0; j < i; j++) {
                dp[i] = dp[j] && set.contains(s.substring(j, i));
                if (dp[i]) break;
            }
        }

        return dp[s.length()];
    }
}
```
---

[wisdompeak/YouTube](https://www.youtube.com/watch?v=eYT-hKQ1au4)

###### tags: `Leetcode` `Trie`