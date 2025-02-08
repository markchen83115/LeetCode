# Leetcode - 2730. Find the Longest Semi-Repetitive Substring (M+)

[Leetcode](https://leetcode.com/problems/find-the-longest-semi-repetitive-substring/)

You are given a digit string `s` that consists of digits from 0 to 9.

A string is called **semi-repetitive** if there is **at most** one adjacent pair of the same digit. For example, `"0010"`, `"002020"`, `"0123"`, `"2002"`, and `"54944"` are semi-repetitive while the following are not: `"00101022"` (adjacent same digit pairs are 00 and 22), and `"1101234883"` (adjacent same digit pairs are 11 and 88).

Return the length of the **longest semi-repetitive **substring** of `s`.

**Example 1:**

> **Input:** s = "52233"
> 
> **Output:** 4
> 
> **Explanation:**
> 
> The longest semi-repetitive substring is "5223". Picking the whole string "52233" has two adjacent same digit pairs 22 and 33, but at most one is allowed.

**Example 2:**

> **Input:** s = "5494"
> 
> **Output:** 4
> 
> **Explanation:**
> 
> `s` is a semi-repetitive string.

**Example 3:**

> **Input:** s = "1111111"
> 
> **Output:** 2
> 
> **Explanation:**
> 
> The longest semi-repetitive substring is "11". Picking the substring "111" has two adjacent same digit pairs, but at most one is allowed.

**Constraints:**

-   `1 <= s.length <= 50`
-   `'0' <= s[i] <= '9'`

---
```java
// Java 6ms(Beats 24.76%), Time O(N), Space O(1)
class Solution {
    public int longestSemiRepetitiveSubstring(String s) {
        int n = s.length();
        int count = 0;
        int ret = 0;
        int j = 0;
        for (int i = 0; i < n; i++)
        {
            while (j < n && count <= 1)
            {
                if (j > 0 && s.charAt(j) == s.charAt(j - 1))
                    count++;
                j++;

                if (count <= 1)
                    ret = Math.max(ret, j - i);
            }

            if (i < n - 1 && s.charAt(i) == s.charAt(i + 1))
                count--;
        }

        return ret;
    }
}
```
---

典型的雙指針滑窗。我們試圖維護一個合法的`[i,j)`的滑窗

基本的演算法是，固定一個`i`的位置，向右滑動右邊界`j`，直至發現加入`j`後會使得`[i:j]`範圍內`consecutive pair`的`count`達到`2`，此時停止j的移動
然後我們再開始下一個回合（`i`指向右邊的位置）前，注意`count`是否需要因為`i`的移動而`-1`


###### tags: `Leetcode` `Two Pointers` `Sliding window`