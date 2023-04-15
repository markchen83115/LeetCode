# Leetcode - 227. Basic Calculator II (H-)

[Leetcode](https://leetcode.com/problems/basic-calculator-ii/description/)

Given a string `s` which represents an expression, _evaluate this expression and return its value_. 

The integer division should truncate toward zero.

You may assume that the given expression is always valid. All intermediate results will be in the range of `[-231, 231 - 1]`.

**Note:** You are not allowed to use any built-in function which evaluates strings as mathematical expressions, such as `eval()`.

**Example 1:**
```
Input: s = "3+2*2"
Output: 7
```
**Example 2:**
```
Input: s = " 3/2 "
Output: 1
```
**Example 3:**
```
Input: s = " 3+5 / 2 "
Output: 5
```
**Constraints:**

-   `1 <= s.length <= 3 * 105`
-   `s` consists of integers and operators `('+', '-', '*', '/')` separated by some number of spaces.
-   `s` represents **a valid expression**.
-   All the integers in the expression are non-negative integers in the range `[0, 231 - 1]`.
-   The answer is **guaranteed** to fit in a **32-bit integer**.

---
```java
// Java 15ms(Beats 83.8%), Time O(N), Space O(N)
class Solution {
    public int calculate(String s) {
        s += "+";
        ArrayDeque<Integer> stack = new ArrayDeque<>();
        int num = 0;
        int ret = 0;
        char sign = '+';
        for (char c : s.toCharArray()) {
            if (c == ' ') continue;
            if (Character.isDigit(c)) {num = num * 10 + c - '0'; continue;}

            // previous sign
            if (sign == '+') stack.push(num);
            else if (sign == '-') stack.push(-num);
            else if (sign == '*') stack.push(stack.pop() * num);
            else if (sign == '/') stack.push(stack.pop() / num);

            sign = c;
            num = 0;
        }

        for (int k : stack)
            ret += k;
        
        return ret;
    }
}
```

---

###### tags: `Leetcode` `Stack` `Parse expression`