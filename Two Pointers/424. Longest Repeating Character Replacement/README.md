# Leetcode - 424. Longest Repeating Character Replacement (H-)

[Leetcode](https://leetcode.com/problems/longest-repeating-character-replacement/)

You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.

Return _the length of the longest substring containing the same letter you can get after performing the above operations_.

**Example 1:**
```
Input: s = "ABAB", k = 2  
Output: 4  
Explanation: Replace the two 'A's with two 'B's or vice versa.
```
**Example 2:**
```
Input: s = "AABABBA", k = 1  
Output: 4  
Explanation: Replace the one 'A' in the middle with 'B' and form "AABBBBA".  
The substring "BBBB" has the longest repeating letters, which is 4.
```
**Constraints:**

-   `1 <= s.length <= 105`
-   `s` consists of only uppercase English letters.
-   `0 <= k <= s.length`

---


```java
// Java  
class Solution {  
    public int characterReplacement(String s, int k) {  
        int start = 0, end = 0;  
        int[] freq = new int[26];  
        int maxLen = 0, maxFreq = 0;  
        while (end < s.length()) {  
            freq[s.charAt(end) - 'A']++;  
            maxFreq = Math.max(maxFreq, freq[s.charAt(end) - 'A']);  
            //length - max frequency <= k furfill  
            while (end - start + 1 - maxFreq > k) {  
                freq[s.charAt(start) - 'A']--;  
                maxFreq = Math.max(maxFreq, freq[s.charAt(start) - 'A']);  
                start++;  
            }  
            maxLen = Math.max(maxLen, end - start + 1);  
            end++;  
        }  
        return maxLen;  
    }  
}
```

---

question means  
`find the longest length which has k element != maxFrequency element`  
ex: `k = 3`, `AAA**B**AAA**CD**AA`  
maxFrequency element is `A` , other elements can appear k times

`current length = end - start + 1`  
`current length - maxFrequency <= k` will fulfill the requirement


###### tags: `Leetcode` `Sliding window`