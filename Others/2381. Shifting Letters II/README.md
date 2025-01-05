# Leetcode - 2381. Shifting Letters II (M)

[Leetcode](https://leetcode.com/problems/shifting-letters-ii/)

You are given a string `s` of lowercase English letters and a 2D integer array `shifts` where `shifts[i] = [starti, endi, directioni]`. For every `i`, **shift** the characters in `s` from the index `starti` to the index `endi` (**inclusive**) forward if `directioni = 1`, or shift the characters backward if `directioni = 0`.

Shifting a character **forward** means replacing it with the **next** letter in the alphabet (wrapping around so that `'z'` becomes `'a'`). Similarly, shifting a character **backward** means replacing it with the **previous** letter in the alphabet (wrapping around so that `'a'` becomes `'z'`).

Return _the final string after all such shifts to _`s`_ are applied_.

**Example 1:**

> **Input:** s = "abc", shifts = [[0,1,0],[1,2,1],[0,2,1]]
> **Output:** "ace"
> **Explanation:** Firstly, shift the characters from index 0 to index 1 backward. Now s = "zac".
> Secondly, shift the characters from index 1 to index 2 forward. Now s = "zbd".
> Finally, shift the characters from index 0 to index 2 forward. Now s = "ace".

**Example 2:**

> **Input:** s = "dztz", shifts = [[0,0,0],[1,1,1]]
> **Output:** "catz"
> **Explanation:** Firstly, shift the characters from index 0 to index 0 backward. Now s = "cztz".
> Finally, shift the characters from index 1 to index 1 forward. Now s = "catz".

**Constraints:**

-   `1 <= s.length, shifts.length <= 5 * 104`
-   `shifts[i].length == 3`
-   `0 <= starti <= endi < s.length`
-   `0 <= directioni <= 1`
-   `s` consists of lowercase English letters.

---
```java
// Java 7ms(Beats 74.53%), Time O(N), Space O(N)
class Solution {
    public String shiftingLetters(String s, int[][] shifts) {
        int n = s.length();
        int[] prefix = new int[n+1];
        for (int[] sh : shifts)
        {
            if (sh[2] == 1)
            {
                prefix[sh[0]]++;
                prefix[sh[1] + 1]--;
            }
            else // 0
            {
                prefix[sh[0]]--;
                prefix[sh[1] + 1]++;
            }
        }

        int sum = 0;
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n; i++)
        {
            sum += prefix[i];
            int diff = (((s.charAt(i) - 'a') + sum) % 26 + 26) % 26;
            sb.append((char) ('a' + diff));
        }

        return sb.toString();
    }
}
```
---

用差分數組紀錄左移與右移的index範圍
最後再從頭掃一遍, 並實時更新差分數組當前的總和, 即為該index要左移/右移的總移動數


###### tags: `Leetcode` `Others` `掃描線/差分陣列`