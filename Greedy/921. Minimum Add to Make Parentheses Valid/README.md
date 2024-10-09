# Leetcode - 921. Minimum Add to Make Parentheses Valid (M+)

[Leetcode](https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/)

A parentheses string is valid if and only if:

-   It is the empty string,
-   It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid strings, or
-   It can be written as `(A)`, where `A` is a valid string.

You are given a parentheses string `s`. In one move, you can insert a parenthesis at any position of the string.

-   For example, if `s = "()))"`, you can insert an opening parenthesis to be `"(()))"` or a closing parenthesis to be `"())))"`.

Return _the minimum number of moves required to make _`s`_ valid_.

**Example 1:**
```
Input: s = "())"
Output: 1
```
**Example 2:**
```
Input: s = "((("
Output: 3
```
**Constraints:**

-   `1 <= s.length <= 1000`
-   `s[i]` is either `'('` or `')'`.

---
```java
// Java 0ms(Beats 100.00%), Time O(N), Space O(N)
class Solution {
    public int minAddToMakeValid(String S) {
        int left = 0, right = 0;
        for (int i = 0; i < S.length(); ++i) 
        {
            if (S.charAt(i) == '(')
                right++;
            else if (right > 0)
                right--;
            else
                left++;
        }
        return left + right;
    }
}
```
```java
// Java 1ms(Beats 58.43%), Time O(N), Space O(N)
class Solution {
    public int minAddToMakeValid(String s) {
        Deque<Character> stack = new ArrayDeque<>();
        for (char c : s.toCharArray())
        {
            if (!stack.isEmpty() && c == ')' && stack.peek() == '(')
            {
                stack.pop();
                continue;
            }
            stack.push(c);
        }

        return stack.size();
    }
}
```
---

遇到`)`時, 檢查前一個是否為`(`, 是的話則可湊為一組
最後的結果為`)))(((`, 則需要insert 6次才可將全部`()`湊齊


###### tags: `Leetcode` `Greedy` `Parenthesis`