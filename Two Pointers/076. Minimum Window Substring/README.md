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
// Java 2ms(Beats 99.89%), Time O(N), Space O(N)
class Solution {
    public String minWindow(String s, String t) {
        int[] freq = new int[128];
        int need = 0;
        for (char ch : t.toCharArray())
            if (++freq[ch - 'A'] == 1)
                need++;
        
        int j = 0;
        int n = s.length();
        int start = 0, end = n + 1;
        for (int i = 0; i < n; i++)
        {
            while (j < n && need > 0)
            {
                if (--freq[s.charAt(j) - 'A'] == 0)
                    need--;
                j++;
            }

            if (need == 0 && end - start > j - i)
            {
                start = i;
                end = j;
            }

            if (++freq[s.charAt(i) - 'A'] == 1)
                need++;
        }

        return end == n + 1 ? "" : s.substring(start, end);
    }
}
```
---

典型的雙指針題型。

對於每個新加入的元素`s[j]`，首先更新該字元出現次數的`Map[s[i]]++`
如果更新後，`Map[s[i]]`等於需要出現的次數`Table[s[i]]`，則計數器`count++`，說明有一個字元滿足了出現次數的要求．

當`count`等於`t`中的字元類型數`COUNT`時，表示任務已經實現。此時，讓左指針不斷右移，對應的`Map[s[i]]`就要自減，一旦`Map[s[i]] < Table[s[i]]`，則count需要自減1從而不再滿足`COUNT`，說明需要繼續加入新元素才能滿足任務. 從而j才可以右移繼續遍歷。

在這個過程中如果滿足條件`count==COUNT`，都需要不斷更新和記錄結果。


###### tags: `Leetcode` `Two Pointers` `Sliding window : Distinct Characters`