# Leetcode - 076. Minimum Window Substring (M+)

[Leetcode](https://leetcode.com/problems/minimum-window-substring/)

Given two strings `s` and `t` of lengths `m` and `n` respectively, return _the _**_minimum window_**

**_substring_**

_of _`s`_ such that every character in _`t`_ (_**_including duplicates_**_) is included in the window_. If there is no such substring, return _the empty string _`""`.

The testcases will be generated such that the answer is **unique**.

**Example 1:**
```
Input: s = "ADOBECODEBANC", t = "ABC"  
Output: "BANC"  
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
```
**Example 2:**
```
Input: s = "a", t = "a"  
Output: "a"  
Explanation: The entire string s is the minimum window.
```
**Example 3:**
```
Input: s = "a", t = "aa"  
Output: ""  
Explanation: Both 'a's from t must be included in the window.  
Since the largest window of s only has one 'a', return empty string.
```
**Constraints:**

-   `m == s.length`
-   `n == t.length`
-   `1 <= m, n <= 105`
-   `s` and `t` consist of uppercase and lowercase English letters.

**Follow up:** Could you find an algorithm that runs in `O(m + n)` time?

---

```java
// Java 110ms  
class Solution {  
    public String minWindow(String s, String t) {  
        String minStr = "";  
        int minLen = Integer.MAX_VALUE;  
        HashMap<Character, Integer> tMap = new HashMap<>();  
        // t char hashmap  
        for (char ch : t.toCharArray()) {  
            tMap.put(ch, tMap.getOrDefault(ch, 0) + 1);  
        }  
  
        int start = 0;  
        int need = t.length();  
        for (int i = 0; i < s.length(); i++) {  
            // increase window  
            char curChar = s.charAt(i);  
            if (tMap.containsKey(curChar)) {  
                if (tMap.get(curChar) > 0) {  
                    need--;  
                }  
                tMap.merge(curChar, -1, Integer::sum);  
            }  
  
            while (need == 0) {  
                // update Min length  
                if ( i - start + 1 < minLen) {  
                    minLen = i - start + 1;  
                    minStr = s.substring(start, i + 1);  
                }  
  
                // decrease window  
                char startChar = s.charAt(start);  
                if (tMap.containsKey(startChar)) {  
                    if (tMap.get(startChar) == 0) {  
                        need++;  
                    }  
                    tMap.merge(startChar, 1, Integer::sum);  
                }  
                start++;  
            }  
        }  
        return minStr;  
    }  
}
```

```java
// Java 2ms  
// use HashMap -> int[]  
// store answer String -> store int  
class Solution {  
    public String minWindow(String s, String t) {  
                int[] map = new int[128];  
        for (int i = 0; i < t.length(); i++) {  
            map[t.charAt(i)]++;  
        }  
        int left = 0;  
        int right = 0;  
        int count = t.length();  
        int min = Integer.MAX_VALUE;  
        int start = 0;  
        while (right < s.length()) {  
            if (map[s.charAt(right++)]-- > 0) {  
                count--;  
            }  
            while (count == 0) {  
                if (right - left < min) {  
                    min = right - left;  
                    start = left;  
                }  
                if (map[s.charAt(left++)]++ == 0) {  
                    count++;  
                }  
            }  
        }  
        return min == Integer.MAX_VALUE ? "" : s.substring(start, start + min);  
    }   
}  
```

---


> **Sliding Window**

calculate t’s frequency,  
**increase window** by every character  
if hit t’s frequency, then **decrease window** until not hit t’s frequency

###### tags: `Leetcode` `Two Pointers` `Sliding window : Distinct Characters`