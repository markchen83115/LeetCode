# Leetcode - 2116. Check if a Parentheses String Can Be Valid (H-)

[Leetcode](https://leetcode.com/problems/check-if-a-parentheses-string-can-be-valid/)

A parentheses string is a **non-empty** string consisting only of `'('` and `')'`. It is valid if **any** of the following conditions is **true**:

-   It is `()`.
-   It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid parentheses strings.
-   It can be written as `(A)`, where `A` is a valid parentheses string.

You are given a parentheses string `s` and a string `locked`, both of length `n`. `locked` is a binary string consisting only of `'0'`s and `'1'`s. For **each** index `i` of `locked`,

-   If `locked[i]` is `'1'`, you **cannot** change `s[i]`.
-   But if `locked[i]` is `'0'`, you **can** change `s[i]` to either `'('` or `')'`.

Return `true` _if you can make `s` a valid parentheses string_. Otherwise, return `false`.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2021/11/06/eg1.png)
> 
> **Input:** s = "))()))", locked = "010100"
> **Output:** true
> **Explanation:** locked[1] == '1' and locked[3] == '1', so we cannot change s[1] or s[3].
> We change s[0] and s[4] to '(' while leaving s[2] and s[5] unchanged to make s valid.

**Example 2:**

> **Input:** s = "()()", locked = "0000"
> **Output:** true
> **Explanation:** We do not need to make any changes because s is already valid.

**Example 3:**

> **Input:** s = ")", locked = "0"
> **Output:** false
> **Explanation:** locked permits us to change s[0]. 
> Changing s[0] to either '(' or ')' will not make s valid.

**Constraints:**

-   `n == s.length == locked.length`
-   `1 <= n <= 105`
-   `s[i]` is either `'('` or `')'`.
-   `locked[i]` is either `'0'` or `'1'`.

---
```java
// Java 10ms(Beats 98.76%), Time O(N), Space O(1)
class Solution {
    public boolean canBeValid(String s, String locked) {
        int countMax = 0, countMin = 0;
        // countMax # of unmatched '(', try to use locked[i] = '0' as '(' if possible 
        // countMin # of unmatched '(', try to use locked[i] = '0' as ')' if possible 
        int n = s.length();
        if (n % 2 == 1)
            return false;

        for (int i = 0; i < n; i++)
        {
            if (locked.charAt(i) == '0')
            {
                countMax++;
                countMin--;
            }
            else if (s.charAt(i) == '(')
            {
                countMax++;
                countMin++;
            }
            else
            {
                countMax--;
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


其實就是`678. Valid Parenthesis String`的`follow up`

當`locked[i] = '0'`, 可視為`(`與`)`
因此我們用`countMax = 0, countMin = 0`
`countMax`代表`# of unmatched '(', try to use locked[i] = '0' as '(' if possible`
`countMin`代表`# of unmatched '(', try to use locked[i] = '0' as ')' if possible`

當遇到`locked[i] = '0'`時
`countMax++;`
`countMin--;`

若`countMin < 0`, 代表我們把太多`locked[i] = '0'`看成`)`
因此調整回`countMin = 0`

若`countMax < 0`, 代表我們把前面全部`locked[i] = '0'`看成`(`, 還是無法解救
因此`return false`



###### tags: `Leetcode` `Greedy` `Parenthesis`