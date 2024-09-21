# Leetcode - 241. Different Ways to Add Parentheses (M+)

[Leetcode](https://leetcode.com/problems/different-ways-to-add-parentheses/)

Given a string `expression` of numbers and operators, return _all possible results from computing all the different possible ways to group numbers and operators_. You may return the answer in **any order**.

The test cases are generated such that the output values fit in a 32-bit integer and the number of different results does not exceed `104`.

**Example 1:**
```
Input: expression = "2-1-1"
Output: [0,2]
Explanation:
((2-1)-1) = 0 
(2-(1-1)) = 2
```
**Example 2:**
```
Input: expression = "2*3-4*5"
Output: [-34,-14,-10,-10,10]
Explanation:
(2*(3-(4*5))) = -34 
((2*3)-(4*5)) = -14 
((2*(3-4))*5) = -10 
(2*((3-4)*5)) = -10 
(((2*3)-4)*5) = 10
```
**Constraints:**

-   `1 <= expression.length <= 20`
-   `expression` consists of digits and the operator `'+'`, `'-'`, and `'*'`.
-   All the integer values in the input expression are in the range `[0, 99]`.
-   The integer values in the input expression do not have a leading `'-'` or `'+'` denoting the sign.

---
```java
// Java 2ms(Beats 63.35%), Time O(N^2), Space O(N)

class Solution {
    public List<Integer> diffWaysToCompute(String t) {
        List<Integer> ret = new ArrayList<>();
        int n = t.length();
        for (int i = 0; i < n; i++)
        {
            char ch = t.charAt(i);
            if (ch != '+' && ch != '-' && ch != '*')
                continue;
            List<Integer> retA = diffWaysToCompute(t.substring(0, i));
            List<Integer> retB = diffWaysToCompute(t.substring(i+1));
            for (int a : retA)
                for (int b : retB)
                    switch(ch)
                    {
                        case '+': ret.add(a+b); break;
                        case '-': ret.add(a-b); break;
                        case '*': ret.add(a*b); break;
                    }
        }

        if (ret.size() == 0)
            ret.add(Integer.parseInt(t));
        
        return ret;
    }
}
```

---

###### tags: `Leetcode` `Recursion` `Evaluate Expressions`