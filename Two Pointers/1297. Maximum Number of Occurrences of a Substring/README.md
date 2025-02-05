# Leetcode - 1297. Maximum Number of Occurrences of a Substring (M+)

[Leetcode](https://leetcode.com/problems/maximum-number-of-occurrences-of-a-substring/)

Given a string `s`, return the maximum number of occurrences of **any** substring under the following rules:

-   The number of unique characters in the substring must be less than or equal to `maxLetters`.
-   The substring size must be between `minSize` and `maxSize` inclusive.

**Example 1:**

> **Input:** s = "aababcaab", maxLetters = 2, minSize = 3, maxSize = 4
> **Output:** 2
> **Explanation:** Substring "aab" has 2 occurrences in the original string.
> It satisfies the conditions, 2 unique letters and size 3 (between minSize and maxSize).

**Example 2:**

> **Input:** s = "aaaa", maxLetters = 1, minSize = 3, maxSize = 3
> **Output:** 2
> **Explanation:** Substring "aaa" occur 2 times in the string. It can overlap.

**Constraints:**

-   `1 <= s.length <= 105`
-   `1 <= maxLetters <= 26`
-   `1 <= minSize <= maxSize <= min(26, s.length)`
-   `s` consists of only lowercase English letters.

---
```java
// Java 54ms(Beats 62.05%), Time O(N), Space O(N)
class Solution {
    public int maxFreq(String s, int maxLetters, int minSize, int maxSize) {
        int ret = 0;
        int n = s.length();
        HashMap<Character, Integer> freqMap = new HashMap<>();
        HashMap<String, Integer> stringMap = new HashMap<>();
        for (int i = 0; i < n; i++)
        {
            char c = s.charAt(i);
            freqMap.merge(c, 1, Integer::sum);

            if (i >= minSize)
            {
                c = s.charAt(i - minSize);
                freqMap.computeIfPresent(c, (a, b) -> b > 1 ? b - 1 : null);
            }

            if (i >= minSize - 1 && freqMap.size() <= maxLetters)
            {
                String t = s.substring(i - minSize + 1, i + 1);
                stringMap.merge(t, 1, Integer::sum);
                ret = Math.max(ret, stringMap.get(t));
            }
        }

        return ret;
    }
}
```
---

題目要求長度介於`minSize`跟`maxSize`之間的`substring`

其實只要看`minSize`長度的`substring`即可
若`minSize = 4`, `maxSize = 5`
`abcde`與`abcde`有相同`abcde`, 則他們也都有`abcd`


###### tags: `Leetcode` `Two Pointers` `Sliding window : Distinct Characters`