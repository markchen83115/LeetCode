# Leetcode - 3403. Find the Lexicographically Largest String From the Box I (M)

[Leetcode](https://leetcode.com/problems/find-the-lexicographically-largest-string-from-the-box-i/)

You are given a string `word`, and an integer `numFriends`.

Alice is organizing a game for her `numFriends` friends. There are multiple rounds in the game, where in each round:

-   `word` is split into `numFriends` **non-empty** strings, such that no previous round has had the **exact** same split.
-   All the split words are put into a box.

Find the **lexicographically largest** string from the box after all the rounds are finished.

A string `a` is **lexicographically smaller** than a string `b` if in the first position where `a` and `b` differ, string `a` has a letter that appears earlier in the alphabet than the corresponding letter in `b`.  
If the first `min(a.length, b.length)` characters do not differ, then the shorter string is the lexicographically smaller one.

**Example 1:**

> **Input:** word = "dbca", numFriends = 2
> 
> **Output:** "dbc"
> 
> **Explanation:** 
> 
> All possible splits are:
> 
> -   `"d"` and `"bca"`.
> -   `"db"` and `"ca"`.
> -   `"dbc"` and `"a"`.

**Example 2:**

> **Input:** word = "gggg", numFriends = 4
> 
> **Output:** "g"
> 
> **Explanation:** 
> 
> The only possible split is: `"g"`, `"g"`, `"g"`, and `"g"`.

**Constraints:**

-   `1 <= word.length <= 5 * 103`
-   `word` consists only of lowercase English letters.
-   `1 <= numFriends <= word.length`

---
```java
// Java 12ms, Time O(N), Space O(1)
class Solution {
    public String answerString(String word, int numFriends) {
        if (numFriends == 1)
            return word;
        int n = word.length();
        int k = n - numFriends + 1;
        String ret = "";
        for (int i = 0; i < n; i++)
        {
            String str = word.substring(i, Math.min(i + k, n));
            if (ret.compareTo(str) < 0)
                ret = str;
        }

        return ret;
    }
}
```
---

本題要特別注意的是edge case: `numFriends == 1` , 直接`return word`

因為要分成`numFriends`組, 因此最長的長度為`n - numFriends + 1` (其他組長度為1)
將以每個單字為頭的最長String做比較, 並注意substring長度可能會越界


###### tags: `Leetcode` `Greedy`