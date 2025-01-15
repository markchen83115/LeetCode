# Leetcode - 2429. Minimize XOR (M)

[Leetcode](https://leetcode.com/problems/minimize-xor/)

Given two positive integers `num1` and `num2`, find the positive integer `x` such that:

-   `x` has the same number of set bits as `num2`, and
-   The value `x XOR num1` is **minimal**.

Note that `XOR` is the bitwise XOR operation.

Return the integer `x`. The test cases are generated such that `x` is **uniquely determined**.

The number of **set bits** of an integer is the number of `1`'s in its binary representation.

**Example 1:**

> **Input:** num1 = 3, num2 = 5
> **Output:** 3
> **Explanation:**
> The binary representations of num1 and num2 are 0011 and 0101, respectively.
> The integer **3** has the same number of set bits as num2, and the value `3 XOR 3 = 0` is minimal.

**Example 2:**

> **Input:** num1 = 1, num2 = 12
> **Output:** 3
> **Explanation:**
> The binary representations of num1 and num2 are 0001 and 1100, respectively.
> The integer **3** has the same number of set bits as num2, and the value `3 XOR 1 = 2` is minimal.

**Constraints:**

-   `1 <= num1, num2 <= 109`

---
```java
// Java 0ms(Beats 100.00%), Time O(N), Space O(1)
class Solution {
    public int minimizeXor(int num1, int num2) {
        int n1 = Integer.bitCount(num1);
        int n2 = Integer.bitCount(num2);
        
        int ret = num1;
        int diff = n2 - n1;
        int i = 0;
        while (diff > 0)
        {
            while ((num1 & (1 << i)) > 0)
                i++;
            ret += (1 << i);
            i++;
            diff--;
        }

        while (diff < 0)
        {
            while ((num1 & (1 << i)) == 0)
                i++;
            ret -= (1 << i);
            i++;
            diff++;
        }

        return ret;   
    }
}
```
---

要使得`x XOR num1`最小, 代表`x的1 bit`位置 = `num1的1 bit`位置
所以比較num1跟num2各自擁有幾個1 bits

若`num2`擁有比較多`1 bit`, 計算`num2`比`num1`多幾個`1 bit`
以`num1`為基底, 若遇到`0 bit`則改為`1`
```
num1 = 1001
num2 =  111
x    = 1011 
```

若`num1`擁有比較多`1 bit`, 計算`num1`比`num2`多幾個`1 bit`
以`num1`為基底, 若遇到`1 bit`則改為`0`
```
num1 = 1011
num2 =  110
x    = 1010 
```


###### tags: `Leetcode` `Bit Manipulation` `XOR`