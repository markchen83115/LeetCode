# Leetcode - 214. Shortest Palindrome (H)

[Leetcode](https://leetcode.com/problems/shortest-palindrome/)

You are given a string `s`. You can convert `s` to a  

palindrome

 by adding characters in front of it.

Return _the shortest palindrome you can find by performing this transformation_.

**Example 1:**
```
Input: s = "aacecaaa"
Output: "aaacecaaa"
```
**Example 2:**
```
Input: s = "abcd"
Output: "dcbabcd"
```
**Constraints:**

-   `0 <= s.length <= 5 * 104`
-   `s` consists of lowercase English letters only.

---
```java
// Java Manacher 257ms (Beats 24.42%), Time O(N), Space O(N)
class Solution {
    public String shortestPalindrome(String s) {
        String t = "#";
        for (int i = 0; i < s.length(); i++)
            t += s.charAt(i) + "#";

        int n = t.length();
        int[] p = new int[n];
        int maxRight = -1, maxCenter = -1;
        int l = 0;  // s從頭開始的最長回文長度
        for (int i = 0; i < n; i++)
        {
            int r = 0;
            if (maxRight > i)
            {
                int j = maxCenter * 2 - i;
                r = Math.min(p[j], maxRight - i);
            }

            while (i - r >= 0 && i + r < n && t.charAt(i-r) == t.charAt(i+r))
                r++;
            p[i] = r - 1;

            if (i + p[i] > maxRight)
            {
                maxRight = i + p[i];
                maxCenter = i;
            }

            if (i - p[i] == 0)
                l = (p[i] * 2 + 1) / 2; // (p[i]*2 + 1) = t的回文長度
        }

        StringBuilder sb = new StringBuilder(s.substring(l));
        String tmp = sb.reverse().toString();
        return tmp + s;
    }
}

/*
maxRight
maxCenter

j = mc * 2 - i

x x [x x x x  x  x x x x] x x
  -------j-------- i   
              mc       mr 

p[i] = min(p[j], mr - i)
*/
```


---
### 解法2：Manacher

[(1147) 【每日一题】LeetCode 214. Shortest Palindrome, 8/30/2021 - YouTube](https://youtu.be/84RVnu5yhHc)
本題為在s中找一個最長的、起點在左端點的回文字串, 把s除去回文字串的後段複製+翻轉, 拼接在原s字串的前面，就是答案。

所以題目的核心就是Manacher的模板提`005.Longest-Palindromic-Substring`，求出每個位置做為中心能構成的最長回文字串，檢查這個回文字串是否能抵達s的最左端，是的話紀錄下來這個回文字串的長度L。輸出的答案是：

```java
string tmp = new StringBuilder(s.substring(L)).reverse().toString();
return tmp + s;
```


###### tags: `Leetcode` `String` `Manacher`