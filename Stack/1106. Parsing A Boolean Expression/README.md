# Leetcode - 1106. Parsing A Boolean Expression (H-)

[Leetcode](https://leetcode.com/problems/parsing-a-boolean-expression/)

A **boolean expression** is an expression that evaluates to either `true` or `false`. It can be in one of the following shapes:

-   `'t'` that evaluates to `true`.
-   `'f'` that evaluates to `false`.
-   `'!(subExpr)'` that evaluates to **the logical NOT** of the inner expression `subExpr`.
-   `'&(subExpr1, subExpr2, ..., subExprn)'` that evaluates to **the logical AND** of the inner expressions `subExpr1, subExpr2, ..., subExprn` where `n >= 1`.
-   `'|(subExpr1, subExpr2, ..., subExprn)'` that evaluates to **the logical OR** of the inner expressions `subExpr1, subExpr2, ..., subExprn` where `n >= 1`.

Given a string `expression` that represents a **boolean expression**, return _the evaluation of that expression_.

It is **guaranteed** that the given expression is valid and follows the given rules.

**Example 1:**
```
Input: expression = "&(|(f))"
Output: false
Explanation: 
First, evaluate |(f) --> f. The expression is now "&(f)".
Then, evaluate &(f) --> f. The expression is now "f".
Finally, return false.
```
**Example 2:**
```
Input: expression = "|(f,f,f,t)"
Output: true
Explanation: The evaluation of (false OR false OR false OR true) is true.
```
**Example 3:**
```
Input: expression = "!(&(f,t))"
Output: true
Explanation: 
First, evaluate &(f,t) --> (false AND true) --> false --> f. The expression is now "!(f)".
Then, evaluate !(f) --> NOT false --> true. We return true.
```
**Constraints:**

-   `1 <= expression.length <= 2 * 104`
-   expression[i] is one following characters: `'('`, `')'`, `'&'`, `'|'`, `'!'`, `'t'`, `'f'`, and `','`.

---
```java
// Java 10ms(Beats 69.27%), Time O(N), Space O(N)
class Solution {
    public boolean parseBoolExpr(String expression) {
        Deque<Character> stack = new ArrayDeque<>();
        char[] exp = expression.toCharArray();
        for (int i = 0; i < exp.length; i++)
        {
            if (exp[i] == ',')
                continue;
                
            if (exp[i] == ')')
            {
                HashSet<Character> seen = new HashSet<>();
                while (stack.peek() != '(')
                    seen.add(stack.pop());
                stack.pop();    // pop '('
                char logic = stack.pop();
                switch(logic)
                {
                    case '|':
                        logic = seen.contains('t') ? 't' : 'f';
                        break;
                    case '&':
                        logic = seen.contains('f') ? 'f' : 't';
                        break;
                    case '!':
                        logic = seen.contains('t') ? 'f' : 't';
                        break;
                }

                stack.push(logic);
            }
            else
            {
                stack.push(exp[i]);
            }
            
        }

        return stack.pop() == 't';
    }
}
```
---
維護一個stack, `,` `)`都不放入`stack`
當看到`)`時, 開始pop out `stack`直到看見`(`
並計算操作結果 再次存入`stack`中


###### tags: `Leetcode` `Stack` `parse expression`