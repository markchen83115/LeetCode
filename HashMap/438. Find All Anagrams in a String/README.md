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
// Java O(s*26)  
class Solution {  
    public List<Integer> findAnagrams(String s, String p) {  
        List<Integer> ret = new LinkedList<>();  
        int[] freqP = new int[26];  
        int[] freqS = new int[26];  
        for (char c : p.toCharArray()) {  
            freqP[c - 'a']++;  
        }  
  
        for (int i = 0; i < s.length(); i++) {  
            freqS[s.charAt(i) - 'a']++;  
            if (i >= p.length()) {  
                freqS[s.charAt(i - p.length()) - 'a']--;  
            }  
            if (Arrays.equals(freqS, freqP)) {  
                ret.add(i - p.length() + 1);  
            }  
        }  
        return ret;  
    }  
}
```


```java
// Java O(s+p)  
class Solution {  
    public List<Integer> findAnagrams(String s, String p) {  
        List<Integer> ret = new LinkedList<>();  
        HashMap<Character, Integer> map = new HashMap<>();  
        for (char c : p.toCharArray()) {  
            map.put(c, map.getOrDefault(c, 0) + 1);  
        }  
        int needMatch = map.size();  
  
        for (int i = 0; i < s.length(); i++) {  
            char c = s.charAt(i);  
            if (map.containsKey(c)) {  
                if (map.get(c) == 1) {  
                    needMatch--;  
                }  
                map.put(c, map.get(c) - 1);  
            }  
            if (i >= p.length()) {  
                char d = s.charAt(i - p.length());  
                if (map.containsKey(d)) {  
                    if (map.get(d) == 0) {  
                        needMatch++;  
                    }  
                    map.put(d, map.get(d) + 1);  
                }  
            }  
            if (needMatch == 0) {  
                ret.add(i - p.length() + 1);  
            }  
        }  
        return ret;  
    }  
}

```


---

利用frequency來比較p跟s的字串內容,  
找出相同的frequency


###### tags: `Leetcode` `HashMap`
