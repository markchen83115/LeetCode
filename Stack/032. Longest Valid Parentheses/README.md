# Leetcode - 32. Longest Valid Parentheses (H)

[Leetcode](https://leetcode.com/problems/longest-valid-parentheses/description/)

Given a string containing just the characters `'('` and `')'`, return *the length of the longest valid (well-formed) parentheses substring*.

**Example 1:**
```
Input: s = "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()".
```
**Example 2:**
```
Input: s = ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()".
```
**Example 3:**
```
Input: s = ""
Output: 0
```
**Constraints:**

-   `0 <= s.length <= 3 * 104`
-   `s[i]` is `'('`, or `')'`.

---
```java
// Java 3ms(Beats 75.40%), Time O(N), Space O(N)
class Solution {
    public int longestValidParentheses(String s) {
        int ret = 0;
        ArrayDeque<Integer> stack = new ArrayDeque<>();
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                stack.push(i);
            } else {
                if (!stack.isEmpty() && s.charAt(stack.peek()) == '(') {
                    stack.pop();
                    ret = Math.max(ret, i - (stack.isEmpty() ? -1 : stack.peek()));
                } else {
                    stack.push(i);
                }
            }
        }

        return ret;
    }
}
```
```java
// Java 1ms(Beats 99.72%), Time O(N), Space O(1)
class Solution {
    public int longestValidParentheses(String s) {
        int ret = 0;
        int n = s.length();
        int open = 0, close = 0;
        // left -> right
        for (int i = 0; i < n; i++)
        {
            if (s.charAt(i) == '(')
                open++;
            else
                close++;
            
            if (open == close)
                ret = Math.max(ret, open * 2);
            else if (close > open)
                open = close = 0;
        }

        open = close = 0;

        // right -> left
        for (int i = n - 1; i >= 0; i--)
        {
            if (s.charAt(i) == '(')
                open++;
            else
                close++;
            
            if (close == open)
                ret = Math.max(ret, close * 2);
            else if (open > close)
                open = close = 0;
        }

        return ret;
    }
}
```
---

[wisdompeak/YouTube解說](https://www.youtube.com/watch?v=677VaZhd4dg)

###### tags: `Leetcode` `Stack`