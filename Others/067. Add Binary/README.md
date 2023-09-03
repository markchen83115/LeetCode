# Leetcode - 67. Add Binary (E+)

[Leetcode](https://leetcode.com/problems/add-binary/)

Given two binary strings `a` and `b`, return _their sum as a binary string_.

**Example 1:**
```
Input: a = "11", b = "1"
Output: "100"
```
**Example 2:**
```
Input: a = "1010", b = "1011"
Output: "10101"
```
**Constraints:**

-   `1 <= a.length, b.length <= 104`
-   `a` and `b` consist only of `'0'` or `'1'` characters.
-   Each string does not contain leading zeros except for the zero itself.

---
```java
// Java 1ms(Beats 100.00%), Time O(N), Space O(N)
class Solution {
    public String addBinary(String a, String b) {
        int carry = 0;
        int i = a.length() - 1;
        int j = b.length() - 1;
        StringBuilder sb = new StringBuilder();
        while (i >= 0 || j >= 0)
        {
            int sum = carry;
            if (i >= 0) sum += a.charAt(i--) - '0';
            if (j >= 0) sum += b.charAt(j--) - '0';
            sb.append(sum & 1);
            carry = sum / 2;
        }
        if (carry > 0) sb.append(carry);
        
        return sb.reverse().toString();
    }
}
```

```csharp
// C# 75ms (Beats 53.42%), Time O(N), Space O(N)
public class Solution {
    public string AddBinary(string a, string b) {
        int carry = 0;
        int i = a.Length - 1;
        int j = b.Length - 1;
        string ret = "";
        while (i >= 0 || j >= 0)
        {
            int sum = carry;
            if (i >= 0) sum += a[i--] - '0';
            if (j >= 0) sum += b[j--] - '0';
            ret = (sum & 1) + ret;
            carry = sum / 2;
        }
        if (carry > 0) ret = carry + ret;

        return ret;
    }
}
```
---

###### tags: `Leetcode` `Others` `位元計算`