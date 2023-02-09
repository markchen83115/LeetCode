# Leetcode - 3. Longest Substring Without Repeating Characters (E+)

[Leetcode](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

Given a string `s`, find the length of the **longest substring** without repeating characters.

**Example 1:**
```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

**Example 2:**
```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**
```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

---

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int i = 0, j = 0, max = 0;
        HashSet<Character> set = new HashSet<Character> ();
        while (j < s.length()) {
            if (!set.contains(s.charAt(j))) {
                set.add(s.charAt(j++));
                max = Math.max(set.size(), max);
            } else {
                set.remove(s.charAt(i++));
            }
        }
        return max;
    }
}
```

---

參考演算法的 pattern — Sliding Window
https://blog.techbridge.cc/2019/09/28/leetcode-pattern-sliding-window/

利用一個 set 來儲存目前 window 裡面有的 char，每次都要確定 window 裡已經沒有重複的 char，才會繼續擴張 window

**Sliding Window 的模板 — 設置 windowStart 跟 windowEnd，最外面的 for 迴圈每一輪都擴張 windowEnd，但是當某些條件滿足時，就要移動 windowStart 來縮減 window**

###### tags: `Leetcode` `Two Pointers` `Sliding window : Distinct Characters`
