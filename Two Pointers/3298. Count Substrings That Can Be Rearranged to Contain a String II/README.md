# Leetcode - 3298. Count Substrings That Can Be Rearranged to Contain a String II (M+)

[Leetcode](https://leetcode.com/problems/count-substrings-that-can-be-rearranged-to-contain-a-string-ii/)

You are given two strings `word1` and `word2`.

A string `x` is called **valid** if `x` can be rearranged to have `word2` as a prefix.

Return the total number of **valid** substrings of `word1`.

**Note** that the memory limits in this problem are **smaller** than usual, so you **must** implement a solution with a _linear_ runtime complexity.

**Example 1:**

> **Input:** word1 = "bcca", word2 = "abc"
> 
> **Output:** 1
> 
> **Explanation:**
> 
> The only valid substring is `"bcca"` which can be rearranged to `"abcc"` having `"abc"` as a prefix.

**Example 2:**

> **Input:** word1 = "abcabc", word2 = "abc"
> 
> **Output:** 10
> 
> **Explanation:**
> 
> All the substrings except substrings of size 1 and size 2 are valid.

**Example 3:**

> **Input:** word1 = "abcabc", word2 = "aaabc"
> 
> **Output:** 0

**Constraints:**

-   `1 <= word1.length <= 106`
-   `1 <= word2.length <= 104`
-   `word1` and `word2` consist only of lowercase English letters.


---
```java
// Java 46ms(Beats 94.78%), Time O(N), Space O(26)
class Solution {
    public long validSubstringCount(String word1, String word2) {
        long ret = 0;
        int[] freq = new int[26];
        int need = 0;
        for (char ch : word2.toCharArray())
            if (++freq[ch - 'a'] == 1)
                need++;
        
        int n = word1.length();
        int j = 0;
        for (int i = 0; i < n; i++)
        {
            while (j < n && need > 0)
            {
                if (--freq[word1.charAt(j) - 'a'] == 0)
                    need--;
                j++;
            }

            if (need == 0)
                ret += n - j + 1;
            
            if (++freq[word1.charAt(i) - 'a'] == 1)
                need++;
        }

        return ret;
    }
}
```
---

使用sliding window求出可行區間`[i, j]`, 而位於`j`之後substring皆為可行解


###### tags: `Leetcode` `Two Pointers` `Sliding window`