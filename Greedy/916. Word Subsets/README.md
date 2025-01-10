# Leetcode - 916. Word Subsets (M+)

[Leetcode](https://leetcode.com/problems/word-subsets/)

You are given two string arrays `words1` and `words2`.

A string `b` is a **subset** of string `a` if every letter in `b` occurs in `a` including multiplicity.

-   For example, `"wrr"` is a subset of `"warrior"` but is not a subset of `"world"`.

A string `a` from `words1` is **universal** if for every string `b` in `words2`, `b` is a subset of `a`.

Return an array of all the **universal** strings in `words1`. You may return the answer in **any order**.

**Example 1:**

> **Input:** words1 = ["amazon","apple","facebook","google","leetcode"], words2 = ["e","o"]
> **Output:** ["facebook","google","leetcode"]

**Example 2:**

> **Input:** words1 = ["amazon","apple","facebook","google","leetcode"], words2 = ["l","e"]
> **Output:** ["apple","google","leetcode"]

**Constraints:**

-   `1 <= words1.length, words2.length <= 104`
-   `1 <= words1[i].length, words2[i].length <= 10`
-   `words1[i]` and `words2[i]` consist only of lowercase English letters.
-   All the strings of `words1` are **unique**.

---
```java
//Java 14ms(Beats 50.00%), Time O(N), Space O(N)
class Solution {
    public List<String> wordSubsets(String[] words1, String[] words2) {
        int n1 = words1.length;
        int n2 = words2.length;
        List<String> ret = new ArrayList<>();
        int[] w2Max = new int[26];
        for (String w2 : words2)
        {
            int[] freq = new int[26];
            for (char ch : w2.toCharArray())
                freq[ch - 'a']++;
            
            for (int i = 0; i < 26; i++)
                w2Max[i] = Math.max(w2Max[i], freq[i]);
        }
            
        search : for (String w1 : words1)
        {
            int[] freq = new int[26];
            for (char ch : w1.toCharArray())
                freq[ch - 'a']++;

            for (int k = 0; k < 26; k++)
            {
                if (freq[k] < w2Max[k])
                    continue search;
            }

            ret.add(w1);
        }

        return ret;
    }
}
```
---

題目只要求words2內的字母要是words1的subset, 不管順序, 只管字母數
需要注意到的是因為要符合`words2`所有word的`frequency`
因此可以直接取用每個word的`最大frequeny`, 因為只要一個word不符合就不行
`w2Max[i] = Math.max(w2Max[i], freq[i])`


###### tags: `Leetcode` `Greedy`