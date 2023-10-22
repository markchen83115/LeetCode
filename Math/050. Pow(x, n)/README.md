# Leetcode - 50. Pow(x, n) (M)

[Leetcode](https://leetcode.com/problems/powx-n/)

Implement [pow(x, n)](http://www.cplusplus.com/reference/valarray/pow/), which calculates `x` raised to the power `n` (i.e., `xn`).

**Example 1:**
```
Input: x = 2.00000, n = 10
Output: 1024.00000
```
**Example 2:**
```
Input: x = 2.10000, n = 3
Output: 9.26100
```
**Example 3:**
```
Input: x = 2.00000, n = -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25
```
**Constraints:**

-   `-100.0 < x < 100.0`
-   `-231 <= n <= 231-1`
-   `n` is an integer.
-   Either `x` is not zero or `n > 0`.
-   `-104 <= xn <= 104`

---
```java
// Java 0ms(Beats 100.00%), Time O(logN), Space O(1)
class Solution {
    public double myPow(double x, int n) {
        double ret = 1;
        if (n < 0)
        {
            n = -n;
            x = 1 / x;
        }

        while (n != 0)
        {
            if ((n & 1) == 1)
                ret *= x;
            
            x *= x;
            n /= 2;
        }

        return ret;
    }
}
```


```java
// Java 0ms(Beats 100.00%), Time O(logN), Space O(1)
class Solution {
    public double myPow(double x, int n) {
        if (n == 0)
            return 1;
        if (n < 0)
            return 1/x * myPow(1/x, -(n+1));
        if (n % 2 == 1)
            return x * myPow(x * x, n/2);
        else
            return myPow(x * x, n/2);
    }
}
```
---


###### tags: `Leetcode` `Math`