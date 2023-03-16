# Leetcode - 22. Generate Parentheses (M)

[Leetcode](https://leetcode.com/problems/generate-parentheses/description/)

Given `n` pairs of parentheses, write a function to _generate all combinations of well-formed parentheses_.

**Example 1:**
```
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```
**Example 2:**
```
Input: n = 1
Output: ["()"]
```
**Constraints:**

-   `1 <= n <= 8`

---
```java
// Java
class Solution {
    List<String> ret = new ArrayList<>();
    public List<String> generateParenthesis(int n) {
        backtracking(n, 0, 0, new StringBuilder());
        return ret;
    }

    public void backtracking(int n, int currLeft, int currRight, StringBuilder sb) {
        if (sb.length() == n * 2) {
            ret.add(sb.toString());
            return;
        }

        if (currLeft < n) {
            sb.append('(');
            backtracking(n, currLeft + 1, currRight, sb);
            sb.deleteCharAt(sb.length() - 1);
        }

        if (currRight < currLeft) {
            sb.append(')');
            backtracking(n, currLeft, currRight + 1, sb);
            sb.deleteCharAt(sb.length() - 1);
        }
    }
}
```



---

###### tags: `Leetcode` `Math` `Combinatorics`
