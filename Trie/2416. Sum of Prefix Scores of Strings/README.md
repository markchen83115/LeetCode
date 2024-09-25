# Leetcode - 2416. Sum of Prefix Scores of Strings (M)

[Leetcode](https://leetcode.com/problems/sum-of-prefix-scores-of-strings/)

You are given an array `words` of size `n` consisting of **non-empty** strings.

We define the **score** of a string `word` as the **number** of strings `words[i]` such that `word` is a **prefix** of `words[i]`.

-   For example, if `words = ["a", "ab", "abc", "cab"]`, then the score of `"ab"` is `2`, since `"ab"` is a prefix of both `"ab"` and `"abc"`.

Return _an array _`answer`_ of size _`n`_ where _`answer[i]`_ is the **sum** of scores of every **non-empty** prefix of _`words[i]`.

**Note** that a string is considered as a prefix of itself.

**Example 1:**
```
Input: words = ["abc","ab","bc","b"]
Output: [5,4,3,2]
Explanation: The answer for each string is the following:
- "abc" has 3 prefixes: "a", "ab", and "abc".
- There are 2 strings with the prefix "a", 2 strings with the prefix "ab", and 1 string with the prefix "abc".
The total is answer[0] = 2 + 2 + 1 = 5.
- "ab" has 2 prefixes: "a" and "ab".
- There are 2 strings with the prefix "a", and 2 strings with the prefix "ab".
The total is answer[1] = 2 + 2 = 4.
- "bc" has 2 prefixes: "b" and "bc".
- There are 2 strings with the prefix "b", and 1 string with the prefix "bc".
The total is answer[2] = 2 + 1 = 3.
- "b" has 1 prefix: "b".
- There are 2 strings with the prefix "b".
The total is answer[3] = 2.
```
**Example 2:**
```
Input: words = ["abcd"]
Output: [4]
Explanation:
"abcd" has 4 prefixes: "a", "ab", "abc", and "abcd".
Each prefix has a score of one, so the total is answer[0] = 1 + 1 + 1 + 1 = 4.
```
**Constraints:**

-   `1 <= words.length <= 1000`
-   `1 <= words[i].length <= 1000`
-   `words[i]` consists of lowercase English letters.

---
```java
// Java 297ms(Beats 68.39%), Time O(N^2), Space O(N)

class Trie 
{
    Trie[] next = new Trie[26];
    int weight = 0;
}

class Solution {
    public int[] sumPrefixScores(String[] words) {
        Trie trie = new Trie();
        int[] ret = new int[words.length];
        for (String w : words)
        {
            Trie node = trie;
            for (char c : w.toCharArray())
            {
                if (node.next[c - 'a'] == null)
                    node.next[c - 'a'] = new Trie();
                node.next[c - 'a'].weight++;
                node = node.next[c - 'a'];
            }
        }

        for (int i = 0; i < words.length; i++)
        {
            Trie node = trie;
            int score = 0;
            for (char c : words[i].toCharArray())
            {
                score += node.next[c - 'a'].weight;
                node = node.next[c - 'a'];
            }
            ret[i] = score;
        }

        return ret;
    }
}
```
---
將其中一個array轉換為字典樹, 並且記錄走過的次數
將另一個array依序走過字典樹, 並將每一步的權重加總, 即為答案


###### tags: `Leetcode` `Trie`