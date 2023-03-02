# Leetcode - 383. Ransom Note (E)

[Leetcode](https://leetcode.com/problems/ransom-note/description/)

Given two strings `ransomNote` and `magazine`, return `true`_ if _`ransomNote`_ can be constructed by using the letters from _`magazine`_ and _`false`_ otherwise_.

Each letter in `magazine` can only be used once in `ransomNote`.

**Example 1:**
```
Input: ransomNote = "a", magazine = "b"
Output: false
```
**Example 2:**
```
Input: ransomNote = "aa", magazine = "ab"
Output: false
```
**Example 3:**
```
Input: ransomNote = "aa", magazine = "aab"
Output: true
```
**Constraints:**

-   `1 <= ransomNote.length, magazine.length <= 105`
-   `ransomNote` and `magazine` consist of lowercase English letters.

---

```java
// Java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        int[] letters = new int[26];
        for (char c : magazine.toCharArray()) {
            letters[c - 'a']++;
        }
        for (char c: ransomNote.toCharArray()) {
            if (--letters[c - 'a'] < 0)
                return false;
        }
        return true;
    }
}
```

---

###### tags: `Leetcode` `HashMap`
