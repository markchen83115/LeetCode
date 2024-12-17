# Leetcode - 2182. Construct String With Repeat Limit (M+)

[Leetcode](https://leetcode.com/problems/construct-string-with-repeat-limit/)

You are given a string `s` and an integer `repeatLimit`. Construct a new string `repeatLimitedString` using the characters of `s` such that no letter appears **more than** `repeatLimit` times **in a row**. You do **not** have to use all characters from `s`.

Return _the **lexicographically largest** _`repeatLimitedString` _possible_.

A string `a` is **lexicographically larger** than a string `b` if in the first position where `a` and `b` differ, string `a` has a letter that appears later in the alphabet than the corresponding letter in `b`. If the first `min(a.length, b.length)` characters do not differ, then the longer string is the lexicographically larger one.

**Example 1:**

> **Input:** s = "cczazcc", repeatLimit = 3
> **Output:** "zzcccac"
> **Explanation:** We use all of the characters from s to construct the repeatLimitedString "zzcccac".
> The letter 'a' appears at most 1 time in a row.
> The letter 'c' appears at most 3 times in a row.
> The letter 'z' appears at most 2 times in a row.
> Hence, no letter appears more than repeatLimit times in a row and the string is a valid repeatLimitedString.
> The string is the lexicographically largest repeatLimitedString possible so we return "zzcccac".
> Note that the string "zzcccca" is lexicographically larger but the letter 'c' appears more than 3 times in a row, so it is not a valid repeatLimitedString.

**Example 2:**

> **Input:** s = "aababab", repeatLimit = 2
> **Output:** "bbabaa"
> **Explanation:** We use only some of the characters from s to construct the repeatLimitedString "bbabaa". 
> The letter 'a' appears at most 2 times in a row.
> The letter 'b' appears at most 2 times in a row.
> Hence, no letter appears more than repeatLimit times in a row and the string is a valid repeatLimitedString.
> The string is the lexicographically largest repeatLimitedString possible so we return "bbabaa".
> Note that the string "bbabaaa" is lexicographically larger but the letter 'a' appears more than 2 times in a row, so it is not a valid repeatLimitedString.

**Constraints:**

-   `1 <= repeatLimit <= s.length <= 105`
-   `s` consists of lowercase English letters.

---
```java
// Java 17ms(Beats 92.80%), Time O(N), Space O(N)
class Solution {
    public String repeatLimitedString(String s, int repeatLimit) {
        int n = s.length();
        int[] freq = new int[26];
        StringBuilder sb = new StringBuilder();
        int next = 25;
        for (char c : s.toCharArray())
            freq[c - 'a']++;
        
        for (int i = 25; i >= 0; i--)
        {
            if (freq[i] == 0)
                continue;

            next = Math.min(i - 1, next);
            while (freq[i] > 0)
            {
                int count = Math.min(repeatLimit, freq[i]);
                for (int j = 0; j < count; j++)
                    sb.append((char) ('a' + i));
                freq[i] -= count;
                if (freq[i] == 0)
                    continue;

                while (next >= 0 && freq[next] == 0)
                    next--;
                if (next < 0)
                    break;

                sb.append((char) ('a' + next));
                freq[next]--;
            }
        }

        return sb.toString();
    }
}
```
---

將英文字母由大到小開始使用
若使用次數已達`repeatLimit`, 則使用次大的英文字母
需注意若已達`repeatLimit`, 但沒有次大的英文字母可使用, 則直接`return`


###### tags: `Leetcode` `Greedy`