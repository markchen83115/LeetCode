# Leetcode - 191. Number of 1 Bits (E+)

[Leetcode](https://leetcode.com/problems/number-of-1-bits/)

Write a function that takes the binary representation of an unsigned integer and returns the number of '1' bits it has (also known as the [Hamming weight](http://en.wikipedia.org/wiki/Hamming_weight)).

**Note:**

-   Note that in some languages, such as Java, there is no unsigned integer type. In this case, the input will be given as a signed integer type. It should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
-   In Java, the compiler represents the signed integers using [2's complement notation](https://en.wikipedia.org/wiki/Two%27s_complement). Therefore, in **Example 3**, the input represents the signed integer. `-3`.

**Example 1:**
```
Input: n = 00000000000000000000000000001011
Output: 3
Explanation: The input binary string 00000000000000000000000000001011 has a total of three '1' bits.
```
**Example 2:**
```
Input: n = 00000000000000000000000010000000
Output: 1
Explanation: The input binary string 00000000000000000000000010000000 has a total of one '1' bit.
```
**Example 3:**
```
Input: n = 11111111111111111111111111111101
Output: 31
Explanation: The input binary string 11111111111111111111111111111101 has a total of thirty one '1' bits.
```
**Constraints:**

-   The input must be a **binary string** of length `32`.

**Follow up:** If this function is called many times, how would you optimize it?

---
```java
// Java 0ms(Beats 100%)
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int ret = 0;
        while (n != 0)
        {
            ret += (n & 1);
            n = n >>> 1;
        }
        return ret;
    }
}
```
```csharp
// C# 23ms(Beats 49.14%)
public class Solution {
    public int HammingWeight(uint n) {
        uint ret = 0;
        while (n > 0)
        {
            ret += (n & 1);
            n = n >> 1;
        }
        return (int)ret;
    }
}
```
```csharp
// C# Real World Does, 22ms(Beats 58.23%) 
public class Solution {
    public int HammingWeight(uint n) {
        return System.Numerics.BitOperations.PopCount(n);
    }
}
```

---

###### tags: `Leetcode` `Others` `位元計算`