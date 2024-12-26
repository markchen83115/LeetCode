# Leetcode - 678. Valid Parenthesis String (H-)

[Leetcode](https://leetcode.com/problems/valid-parenthesis-string/)

Given a string `s` containing only three types of characters: `'('`, `')'` and `'*'`, return `true` _if_ `s` _is **valid**_.

The following rules define a **valid** string:

-   Any left parenthesis `'('` must have a corresponding right parenthesis `')'`.
-   Any right parenthesis `')'` must have a corresponding left parenthesis `'('`.
-   Left parenthesis `'('` must go before the corresponding right parenthesis `')'`.
-   `'*'` could be treated as a single right parenthesis `')'` or a single left parenthesis `'('` or an empty string `""`.

**Example 1:**

> **Input:** s = "()"
> **Output:** true

**Example 2:**

> **Input:** s = "(*)"
> **Output:** true

**Example 3:**

> **Input:** s = "(*))"
> **Output:** true

**Constraints:**

-   `1 <= s.length <= 100`
-   `s[i]` is `'('`, `')'` or `'*'`.

---
```java
// Java 0ms(Beats 100.00%), Time O(N), Space O(1)
class Solution {
    public boolean checkValidString(String s) {
        int countMax = 0; // max # of unmatched '(', try to use * as '(' if possible 
        int countMin = 0; // min # of unmatched '(', try to use * as ')' if possible 
        for (char c : s.toCharArray())
        {
            if (c == '(')
            {
                countMax++;
                countMin++;
            }
            else if (c == ')')
            {
                countMax--;
                countMin--;
            }
            else
            {
                countMax++;
                countMin--;
            }

            if (countMax < 0)
                return false;
            if (countMin < 0)
                countMin = 0;
        }

        return countMin == 0; 
    }
}
```
---

參考[【每日一题】678. Valid Parenthesis String, 8/16/2020 - YouTube](https://youtu.be/ReR0bp9cAtc)


###### tags: `Leetcode` `Greedy` `Parenthesis`