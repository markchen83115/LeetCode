# Leetcode - 136. Single Number (M)

[Leetcode](https://leetcode.com/problems/single-number/)

Given a **non-empty** array of integers `nums`, every element appears _twice_ except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.

**Example 1:**
```
Input: nums = [2,2,1]
Output: 1
```
**Example 2:**
```
Input: nums = [4,1,2,1,2]
Output: 4
```
**Example 3:**
```
Input: nums = [1]
Output: 1
```
**Constraints:**

-   `1 <= nums.length <= 3 * 104`
-   `-3 * 104 <= nums[i] <= 3 * 104`
-   Each element in the array appears twice except for one element which appears only once.

---
```java
// Java 1ms (Beats 99.98%)
class Solution {
    public int singleNumber(int[] nums) {
        int ret = 0;
        for (int num : nums)
            ret ^= num;
        
        return ret;
    }
}
```

---

使用`XOR`操作。
```
A^A = 0

A^B = 1

A^B^B = A (交換性commutative)

0^A = A (所以初始值設為0)
```

###### tags: `Leetcode` `Bit Manipulation` `XOR`
