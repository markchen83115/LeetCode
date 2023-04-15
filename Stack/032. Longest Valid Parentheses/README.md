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
---

[wisdompeak/YouTube解說](https://www.youtube.com/watch?v=677VaZhd4dg)

###### tags: `Leetcode` `Stack`