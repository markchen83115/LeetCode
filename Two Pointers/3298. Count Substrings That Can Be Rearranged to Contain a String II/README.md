# Leetcode - 3298. Count Substrings That Can Be Rearranged to Contain a String II (M+)

[Leetcode](https://leetcode.com/problems/count-substrings-that-can-be-rearranged-to-contain-a-string-ii/)

You are given two strings `word1` and `word2`.

A string `x` is called **valid** if `x` can be rearranged to have `word2` as a  

prefix

.

Return the total number of **valid**  

substrings

 of `word1`.

**Note** that the memory limits in this problem are **smaller** than usual, so you **must** implement a solution with a _linear_ runtime complexity.

**Example 1:**

Input: word1 = "bcca", word2 = "abc"
```
Output: 1

Explanation:

The only valid substring is `"bcca"` which can be rearranged to `"abcc"` having `"abc"` as a prefix.
```
**Example 2:**
```
Input: word1 = "abcabc", word2 = "abc"

Output: 10

Explanation:

All the substrings except substrings of size 1 and size 2 are valid.
```
**Example 3:**
```
Input: word1 = "abcabc", word2 = "aaabc"

Output: 0
```
**Constraints:**

-   `1 <= word1.length <= 106`
-   `1 <= word2.length <= 104`
-   `word1` and `word2` consist only of lowercase English letters.

---
```java
// Java 44ms(Beats 94.38%), Time O(N), Space O(N)

class Solution {
    public long validSubstringCount(String word1, String word2) {
        long ret = 0;
        int j = 0;
        int n = word1.length();
        int k = word2.length();
        int[] freq = new int[26];
        for (char c : word2.toCharArray())
            freq[c - 'a']++;
        
        for (int i = 0; i < n; i++)
        {
            while (k > 0 && j < n)
            {
                char ch = word1.charAt(j);
                freq[ch - 'a']--;
                if (freq[ch - 'a'] >= 0)
                    k--;
                j++;
            }
            
            if (k == 0)
                ret += n - j + 1;
            
            char ch = word1.charAt(i);
            freq[ch - 'a']++;
            if (freq[ch - 'a'] > 0)
                k++;
        }
        
        return ret;
    }
}
```
---
使用sliding window求出可行區間`[i, j]`, 而位於`j`之後substring皆為可行解


###### tags: `Leetcode` `Two Pointers` `Sliding window`