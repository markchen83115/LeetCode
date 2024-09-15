# Leetcode - 1371. Find the Longest Substring Containing Vowels in Even Counts (H-)

[Leetcode](https://leetcode.com/problems/find-the-longest-substring-containing-vowels-in-even-counts/)

Given the string `s`, return the size of the longest substring containing each vowel an even number of times. That is, 'a', 'e', 'i', 'o', and 'u' must appear an even number of times.

**Example 1:**
```
Input: s = "eleetminicoworoep"
Output: 13
Explanation: The longest substring is "leetminicowor" which contains two each of the vowels: e, i and o and zero of the vowels: a and u.
```
**Example 2:**
```
Input: s = "leetcodeisgreat"
Output: 5
Explanation: The longest substring is "leetc" which contains two e's.
```
**Example 3:**
```
Input: s = "bcbcbc"
Output: 6
Explanation: In this case, the given string "bcbcbc" is the longest because all vowels: a, e, i, o and u appear zero times.
```
**Constraints:**

-   `1 <= s.length <= 5 x 10^5`
-   `s` contains only lowercase English letters.

---
```java
// Java 74ms (Beats 21.71%), Time O(N), Space O(N)
class Solution {
    public int findTheLongestSubstring(String s) {
        // 將 [uoiea] 存為二進制 = [1 1 1 1 1]
        HashMap<Character, Integer> voewlIndex = new HashMap<>();
        voewlIndex.put('a', 0);
        voewlIndex.put('e', 1);
        voewlIndex.put('i', 2);
        voewlIndex.put('o', 3);
        voewlIndex.put('u', 4);

        HashMap<Integer, Integer> stateIndex = new HashMap<>();
        stateIndex.put(0, -1);
        int state = 0;
        int ret = 0;
        for (int i = 0; i < s.length(); i++)
        {
            char c = s.charAt(i);
            if (voewlIndex.containsKey(c))
                state ^= (1 << voewlIndex.get(c));
            
            stateIndex.putIfAbsent(state, i);
            
            ret = Math.max(ret, i - stateIndex.get(state));
        }

        return ret;
    }
}
```
```java
// Java 74ms (Beats 21.71%), Time O(N), Space O(N)
class Solution {
    public int findTheLongestSubstring(String s) {
        HashMap<Character, Integer> voewlIndex = new HashMap<>();
        voewlIndex.put('a', 1);
        voewlIndex.put('e', 2);
        voewlIndex.put('i', 4);
        voewlIndex.put('o', 8);
        voewlIndex.put('u', 16);

        HashMap<Integer, Integer> stateIndex = new HashMap<>();
        stateIndex.put(0, -1);
        int state = 0;
        int ret = 0;
        for (int i = 0; i < s.length(); i++)
        {
            char c = s.charAt(i);
            if (voewlIndex.containsKey(c))
                state ^= voewlIndex.get(c);
            
            stateIndex.putIfAbsent(state, i);
            
            ret = Math.max(ret, i - stateIndex.get(state));
        }

        return ret;
    }
}

```
---
題目要求最長的substring, 其母音的次數為偶數
因此採用prefix的方法, 區間的母音數量為`prefix[i:j] = prefix[j] - prefix[i - 1]`

將母音的奇偶數存於二進制`aeiou = 11111`, 1=奇數, 0=偶數
透過XOR來翻轉為奇數or偶數


###### tags: `Leetcode` `Hash Map` `Hash+Prefix`