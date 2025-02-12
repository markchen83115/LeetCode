# Leetcode - 1234. Replace the Substring for Balanced String (H-)

[Leetcode](https://leetcode.com/problems/replace-the-substring-for-balanced-string/)

You are given a string s of length `n` containing only four kinds of characters: `'Q'`, `'W'`, `'E'`, and `'R'`.

A string is said to be **balanced** if each of its characters appears `n / 4` times where `n` is the length of the string.

Return _the minimum length of the substring that can be replaced with **any** other string of the same length to make _`s`_ **balanced**_. If s is already **balanced**, return `0`.

**Example 1:**

> **Input:** s = "QWER"
> **Output:** 0
> **Explanation:** s is already balanced.

**Example 2:**

> **Input:** s = "QQWE"
> **Output:** 1
> **Explanation:** We need to replace a 'Q' to 'R', so that "RQWE" (or "QRWE") is balanced.

**Example 3:**

> **Input:** s = "QQQW"
> **Output:** 2
> **Explanation:** We can replace the first "QQ" to "ER". 

**Constraints:**

-   `n == s.length`
-   `4 <= n <= 105`
-   `n` is a multiple of `4`.
-   `s` contains only `'Q'`, `'W'`, `'E'`, and `'R'`.

---
```java
// Java 10ms(Beats 64.71%), Time O(N), Space O(4)
class Solution {
    public int balancedString(String s) {
        int[] freq = new int[4]; //QWER
        int n = s.length();
        int ret = n;
        for (char ch : s.toCharArray())
        {
            switch(ch)
            {
                case 'Q':
                    freq[0]++; break;
                case 'W':
                    freq[1]++; break;
                case 'E':
                    freq[2]++; break;
                case 'R':
                    freq[3]++; break;
            }
        }

        int j = 0;
        for (int i = 0; i < n; i++)
        {
            while (j < n && !isOK(freq, n / 4))
            {
                switch(s.charAt(j))
                {
                    case 'Q':
                        freq[0]--; break;
                    case 'W':
                        freq[1]--; break;
                    case 'E':
                        freq[2]--; break;
                    case 'R':
                        freq[3]--; break;
                }
                j++;
            }

            if(isOK(freq, n / 4))
                ret = Math.min(ret, j - i);
            
            switch(s.charAt(i))
            {
                case 'Q':
                    freq[0]++; break;
                case 'W':
                    freq[1]++; break;
                case 'E':
                    freq[2]++; break;
                case 'R':
                    freq[3]++; break;
            }
        }

        return ret;
    }

    boolean isOK(int[] freq, int k)
    {
        for (int f : freq)
            if (f > k)
                return false;
        return true;
    }
}
```
---

本題其實雙指針來做更簡單，時間複雜度為o(n). 根據解法1的分析，我們其實只要找到一段窗口，使得窗口外的詞頻統計滿足每種字母的頻率都不超過n/4.

基於此演算法，滑窗的兩個指標是可以交替移動的．比如說當慢指針為i,快指針移動到j，滿足條件．那麼下一步慢指針移動到i+1,而快指針則不用動．為什麼快指針不需要考察小於ｊ的位置？其實如果視窗`[i+1,k]`滿足條件的話`（k<j)`，那麼一定有`[i,k]`也滿足條件，所以快指針根本就不會走到j的位置了．所以我們可以保證，並沒有一個`k<j`使得`[i+1,k]`滿足條件．所以快指針不需要回調．

所以我們只要遍歷左邊界i，相應地單向朝右移動右邊界j，使得[i:j]是一個滿足條件的區間。探索所有的這樣的區間，找一個最短的。


###### tags: `Leetcode` `Two Pointers`