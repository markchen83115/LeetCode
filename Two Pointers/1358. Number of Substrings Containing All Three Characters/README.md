# Leetcode - 1358. Number of Substrings Containing All Three Characters (M)

[Leetcode](https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/)

Given a string `s` consisting only of characters _a_, _b_ and _c_.

Return the number of substrings containing **at least** one occurrence of all these characters _a_, _b_ and _c_.

**Example 1:**

> **Input:** s = "abcabc"
> **Output:** 10
> **Explanation:** The substrings containing at least one occurrence of the characters _a_, _b_ and _c are "_abc_", "_abca_", "_abcab_", "_abcabc_", "_bca_", "_bcab_", "_bcabc_", "_cab_", "_cabc_"_ and _"_abc_"_ (**again**)_._ 

**Example 2:**

> **Input:** s = "aaacb"
> **Output:** 3
> **Explanation:** The substrings containing at least one occurrence of the characters _a_, _b_ and _c are "_aaacb_", "_aacb_"_ and _"_acb_"._   

**Example 3:**

> **Input:** s = "abc"
> **Output:** 1

**Constraints:**

-   `3 <= s.length <= 5 x 10^4`
-   `s` only consists of _a_, _b_ or _c _characters.

---
```java
// Java 7ms(Beats 99.47%), Time O(N), Space O(3)
class Solution {
    public int numberOfSubstrings(String s) {
        int ret = 0;
        int n = s.length();
        int[] freq = new int[3];
        int need = 3;
        int j = 0;
        for (int i = 0; i < n; i++)
        {
            while (j < n && need > 0)
            {
                if (++freq[s.charAt(j) - 'a'] == 1)
                    need--;
                j++;
            }

            if (need == 0)
                ret += n - j + 1;
            
            if (--freq[s.charAt(i) - 'a'] == 0)
                need++;
        }

        return ret;
    }
}
```
---

我們固定滑窗的左端點i，向右探索右端點j。當我們發現移動到某處的j，使得[i:j]恰好至少包含a,b,c各一個的時候，那麼意味著右端點其實可以直至在從j到n-1的任何位置，都滿足條件。這樣的區間有n-j個。

此時我們查看下一個i作為左端點，同樣為了滿足[i:j]恰好至少包含a,b,c各一個，j必然向右移動。同理，可以計算出以i為左端點、符合條件的區間的個數。

最終答案就是以每個i作為左端點時，符合條件的右端點的數目的總和。


###### tags: `Leetcode` `Two Pointers` `Sliding window`