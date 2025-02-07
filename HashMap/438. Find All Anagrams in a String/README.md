# Leetcode - 438. Find All Anagrams in a String (M+)

[Leetcode](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

Given two strings `s` and `p`, return _an array of all the start indices of _`p`_'s anagrams in _`s`. You may return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**
```
Input: s = "cbaebabacd", p = "abc"  
Output: [0,6]  
Explanation:  
The substring with start index = 0 is "cba", which is an anagram of "abc".  
The substring with start index = 6 is "bac", which is an anagram of "abc".
```
**Example 2:**
```
Input: s = "abab", p = "ab"  
Output: [0,1,2]  
Explanation:  
The substring with start index = 0 is "ab", which is an anagram of "ab".  
The substring with start index = 1 is "ba", which is an anagram of "ab".  
The substring with start index = 2 is "ab", which is an anagram of "ab".
```
**Constraints:**

-   `1 <= s.length, p.length <= 3 * 104`
-   `s` and `p` consist of lowercase English letters.

---


```java
// Java 7ms(Beats 96.00%), Time O(N), Space O(26)
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> ret = new ArrayList<>();
        int n1 = s.length();
        int n2 = p.length();
        int[] freq = new int[26];
        int need = 0;
        for (char ch : p.toCharArray())
            if (++freq[ch - 'a'] == 1)
                need++;
        
        for (int i = 0; i < n1; i++)
        {
            if (--freq[s.charAt(i) - 'a'] == 0)
                need--;
            if (i >= n2 && ++freq[s.charAt(i - n2) - 'a'] == 1)
                need++;
            if (need == 0)
                ret.add(i - n2 + 1);
        }

        return ret;
    }
}
```
---

利用frequency來比較p跟s的字串內容, 找出相同的frequency
若s[i]加入時, --freq[s[i]] == 0, 代表該字母已經足夠, 則need--
若s[i]退出時, --freq[s[i]] == 1, 代表該字母從足夠變為不足夠, 則need++


###### tags: `Leetcode` `HashMap`