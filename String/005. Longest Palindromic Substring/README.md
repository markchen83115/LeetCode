# Leetcode - 5. Longest Palindromic Substring (H)

[Leetcode](https://leetcode.com/problems/longest-palindromic-substring/)

Given a string `s`, return _the longest palindromic substring _in `s`.

**Example 1:**
```
Input: s = "babad"  
Output: "bab"  
Explanation: "aba" is also a valid answer.
```
**Example 2:**
```
Input: s = "cbbd"  
Output: "bb"
```
**Constraints:**

-   `1 <= s.length <= 1000`
-   `s` consist of only digits and English letters.

---

```java
// Java  
// ManaCher algo.  
class Solution {  
    public String longestPalindrome(String s) {  
        String t = "#";  
        for (char c : s.toCharArray()) {  
            t += c;  
            t += "#";  
        }  
  
        int n = t.length();  
        int maxRight = -1;  
        int maxCenter = -1;  
        int[] maxRadius = new int[n];  
  
        for (int i = 0; i < n; i++) {  
            int radius = 0;  
            //caculate every radius  
            if (i < maxRight) {  
                int j = maxCenter * 2 - i;  
                radius = Math.min(maxRadius[j], maxRight - i);  
                while (i - radius >= 0 && i + radius < n   
                    && t.charAt(i - radius) == t.charAt(i + radius)) {  
                    radius++;  
                }  
            } else {  
                radius = 0;  
                while (i - radius >= 0 && i + radius < n   
                    && t.charAt(i - radius) == t.charAt(i + radius)) {  
                    radius++;  
                }  
            }  
            maxRadius[i] = radius - 1;  
  
            //update maxRight, maxCenter  
            if (i + maxRadius[i] > maxRight) {  
                maxRight = i + maxRadius[i];  
                maxCenter = i;  
            }  
        }  
  
        //find max length  
        int maxLen = 0;  
        int center = 0;  
        for (int i = 0; i < n; i++) {  
            if (maxRadius[i] > maxLen) {  
                maxLen = maxRadius[i];  
                center = i;  
            }  
        }  
  
        //return s.substring(center - maxLen, center + maxLen + 1);  
        return s.substring((center-maxLen)/2, (center-maxLen)/2 + maxLen);  
    }  
  
    // Manacher algo:   
    //for odd:  
    // maxRight  
    // maxCenter  
  
    //               mc         mr  
    // x {* o x o *  x  * o x o *} x x  
    //        j      mc     i  
    // _______________  
  
  
    //for even:  
    // c b b d  
    //         c.  radius = 2   
    // # c # a # b # a #  
    //     --------- len = 5  
    // recover => s.substring(center/2 - radius/2, center/2 + radius/2 + 1);  
  
} 
```

```java
// Java  
// ManaCher algo.  
class Solution {  
    public String longestPalindrome(String s) {  
        String t = "#";  
        for (char c : s.toCharArray()) {  
            t += c + "#";  
        }  
  
        int n = t.length();  
        int maxRight = -1;  
        int maxCenter = -1;  
        int[] maxRadius = new int[n];  
        int maxLen = 0;  
        int center = 0;  
  
        for (int i = 0; i < n; i++) {  
            int radius = 0;  
            //caculate every radius  
            if (i < maxRight) {  
                int j = maxCenter * 2 - i;  
                radius = Math.min(maxRadius[j], maxRight - i);  
            }  
            while (i - radius >= 0 && i + radius < n   
                && t.charAt(i - radius) == t.charAt(i + radius)) {  
                radius++;  
            }  
            maxRadius[i] = radius - 1;  
  
            //update maxRight, maxCenter  
            if (i + maxRadius[i] > maxRight) {  
                maxRight = i + maxRadius[i];  
                maxCenter = i;  
            }  
  
            //find max length  
            if (maxRadius[i] > maxLen) {  
                maxLen = maxRadius[i];  
                center = i;  
            }  
        }  
        return s.substring((center-maxLen)/2, (center-maxLen)/2 + maxLen);  
    }  
}
```

---

> Manacher 演算法 O(N)

[wisdompeek/Youtube解說](https://www.youtube.com/watch?v=Co5-YaJwq64)

```css
先只看奇數的迴文 ex: abcba  
假設{}內的是迴文字串  
中心點為maxCenter  
迴文最右邊的點為maxRight  
  
maxRadius[i]為以i為中心, 左右的迴文距離  
ex: zabcbaq, i = "c", maxRadius[3] = 2  
  
              mC        mR
x {x x x x x  x  x x x x x} x x
       j             i
_______________ => j的迴文長度為3 

當i < maxRight時, 能以maxCenter為中心, 找到一個迴文對應點j

則
   第一個*可透過對應, 將其他點對應為*
   第一個o可透過對應, 將其他點對應為o
x {* o x o *  x  * o x o *} x x
       j      mC     i   mR
因此, 可以確定i的迴文長度至少為2  
Min(maxRadius[j], maxRight - i) = maxRadius[i]  
Min(3, 2)                       = 2  
故從2開始向外找迴文  
最後的長度為substring( center-maxLen, center+maxLen+1 )  
  
  
再來看偶數的迴文 ex: cbbd  
可變型為 # c # b # b # d #  
再透過轉換為原始長度  
最後的長度為substring( (center-maxLen)/2, center-maxLen)/2 + maxLen )  
*注意: 不可使用substring( (center-maxLen)/2, center+maxLen/2 ) -> 會有奇數/2的問題
```



###### tags: `Leetcode` `String` `Manacher`
