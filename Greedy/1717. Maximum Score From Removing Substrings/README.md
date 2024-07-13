# Leetcode - 1717. Maximum Score From Removing Substrings (M+)

[Leetcode](https://leetcode.com/problems/maximum-score-from-removing-substrings/)

You are given a string `s` and two integers `x` and `y`. You can perform two types of operations any number of times.

-   Remove substring `"ab"` and gain `x` points.
    -   For example, when removing `"ab"` from `"cabxbae"` it becomes `"cxbae"`.
-   Remove substring `"ba"` and gain `y` points.
    -   For example, when removing `"ba"` from `"cabxbae"` it becomes `"cabxe"`.

Return _the maximum points you can gain after applying the above operations on_ `s`.

**Example 1:**
```
Input: s = "cdbcbbaaabab", x = 4, y = 5
Output: 19
Explanation:
- Remove the "ba" underlined in "cdbcbbaaabab". Now, s = "cdbcbbaaab" and 5 points are added to the score.
- Remove the "ab" underlined in "cdbcbbaaab". Now, s = "cdbcbbaa" and 4 points are added to the score.
- Remove the "ba" underlined in "cdbcbbaa". Now, s = "cdbcba" and 5 points are added to the score.
- Remove the "ba" underlined in "cdbcba". Now, s = "cdbc" and 5 points are added to the score.
Total score = 5 + 4 + 5 + 5 = 19.
```
**Example 2:**
```
Input: s = "aabbaaxybbaabb", x = 5, y = 4
Output: 20
```
**Constraints:**

-   `1 <= s.length <= 105`
-   `1 <= x, y <= 104`
-   `s` consists of lowercase English letters.

---
```java
// Java 55ms Beats(82.86%), Time O(N), Space O(N)
class Solution {
    public int maximumGain(String s, int x, int y) {
        int ret = 0;
        String s1, s2;
        int p1, p2;
        if (y > x)
        {
            s1 = "ba";
            p1 = y;
            s2 = "ab";
            p2 = x;
        }
        else 
        {
            s1 = "ab";
            p1 = x;
            s2 = "ba";
            p2 = y;
        }

        // remove s1
        StringBuilder sb = new StringBuilder();
        for (char c : s.toCharArray())
        {
            if (c == s1.charAt(1) && sb.length() > 0 && sb.charAt(sb.length() - 1) == s1.charAt(0))
            {
                ret += p1;
                sb.setLength(sb.length() - 1);
            }
            else
            {
                sb.append(c);
            }
        }

        //remove s2
        StringBuilder sb2 = new StringBuilder();
        for (char c : sb.toString().toCharArray())
        {
            if (c == s2.charAt(1) && sb2.length() > 0 && sb2.charAt(sb2.length() - 1) == s2.charAt(0))
            {
                ret += p2;
                sb2.setLength(sb2.length() - 1);
            }
            else
            {
                sb2.append(c);
            }
        }

        return ret;
    }
}
```
---

此題就是一個簡單的貪心法。如果ab的收益比ba大，那麼從頭到尾我們就盡量刪除ab。一遍走完之後，剩下的就一定只是bbbaaa的形式，那麼我們就只需要再走一遍刪ba了。

如果ba的收益比ab大，我們有一個比較巧妙的處理方法。就是將s逆序，並把x和y對換。這樣我們依然重複使用上面的程式碼，第一遍刪ab，第二遍刪ba。

###### tags: `Leetcode` `Greedy`