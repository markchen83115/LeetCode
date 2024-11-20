# Leetcode - 043. Multiply Strings (M)

[Leetcode](https://leetcode.com/problems/multiply-strings/)

Given two non-negative integers `num1` and `num2` represented as strings, return the product of `num1` and `num2`, also represented as a string.

**Note:** You must not use any built-in BigInteger library or convert the inputs to integer directly.

**Example 1:**

> **Input:** num1 = "2", num2 = "3"
> **Output:** "6"

**Example 2:**

> **Input:** num1 = "123", num2 = "456"
> **Output:** "56088"

**Constraints:**

-   `1 <= num1.length, num2.length <= 200`
-   `num1` and `num2` consist of digits only.
-   Both `num1` and `num2` do not contain any leading zero, except the number `0` itself.

---
```java
// Java 2ms(Beats 98.14%), Time O(N^2), Space O(N^2)
class Solution {
    public String multiply(String num1, String num2) {
        char[] arr1 = num1.toCharArray();
        char[] arr2 = num2.toCharArray();
        int n1 = arr1.length, n2 = arr2.length;
        int[] ret = new int[n1 + n2];
        for (int i = n1 - 1; i >= 0; i--)
            for (int j = n2 - 1; j >= 0; j--)
            {
                int mul = (arr1[i] - '0') * (arr2[j] - '0');
                int p1 = i + j, p2 = i + j + 1;
                int sum = mul + ret[p2];

                ret[p2] = sum % 10;
                ret[p1] += sum / 10;
            }
        
        
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < ret.length; i++)
        {
            if (sb.length() == 0 && ret[i] == 0)
                continue;
            sb.append(ret[i]);
        }

        return sb.length() == 0 ? "0" : sb.toString();
    }
}
```
---

想像我們在做乘法, `num1[i] * num2[j]`的位置正好對應到`[i + j`, `i + j + 1]`
```
index       [0 1 2]
             1 2 3
           x 4 5 6
      --------------
               1 8
             1 2
             6
             1 5
           1 0
         ...
index [0 1 2 3 4 5]
```


###### tags: `Leetcode` `Math`