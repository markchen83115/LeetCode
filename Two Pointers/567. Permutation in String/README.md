# Leetcode - 567. Permutation in String (M)

[Leetcode](https://leetcode.com/problems/permutation-in-string/)

Given two strings `s1` and `s2`, return `true` if `s2` contains a  

permutation

 of `s1`, or `false` otherwise.

In other words, return `true` if one of `s1`'s permutations is the substring of `s2`.

**Example 1:**
```
Input: s1 = "ab", s2 = "eidbaooo"
Output: true
Explanation: s2 contains one permutation of s1 ("ba").
```
**Example 2:**
```
Input: s1 = "ab", s2 = "eidboaoo"
Output: false
```
**Constraints:**

-   `1 <= s1.length, s2.length <= 104`
-   `s1` and `s2` consist of lowercase English letters.

---
```java
// Java 4ms(Beats 99.19%), Time O(N), Space O(N)
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int[] freq = new int[26];
        int count = 0;
        for (char c : s1.toCharArray())
            if (++freq[c - 'a'] == 1)
                count++;

        int j = 0;
        int n1 = s1.length();
        int n2 = s2.length();
        for (int i = 0; i < n2; i++)
        {
            while (j < n2 && count > 0)
            {
                char ch = s2.charAt(j);
                if (--freq[ch - 'a'] == 0)
                    count--;
                j++;
            }

            if (count == 0 && j - i == n1)
                return true;
            
            char ch = s2.charAt(i);
            if (++freq[ch - 'a'] == 1)
                count++;
        }

        return false;
    }
}
```

---

使用`sliding window`求解
注意當達到所需的字母數時(`count = 0`), 同時檢查當前長度是否為`s1`的長度
若非`s1`長度, 代表當前`sliding window`含有其他字母

###### tags: `Leetcode` `Two Pointers` `Sliding window`