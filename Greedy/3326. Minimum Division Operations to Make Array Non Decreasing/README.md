# Leetcode - 3326. Minimum Division Operations to Make Array Non Decreasing (M)

[Leetcode](https://leetcode.com/problems/minimum-division-operations-to-make-array-non-decreasing/)

You are given an integer array `nums`.

Any **positive** divisor of a natural number `x` that is **strictly less** than `x` is called a **proper divisor** of `x`. For example, 2 is a _proper divisor_ of 4, while 6 is not a _proper divisor_ of 6.

You are allowed to perform an **operation** any number of times on `nums`, where in each **operation** you select any _one_ element from `nums` and divide it by its **greatest** **proper divisor**.

Return the **minimum** number of **operations** required to make the array **non-decreasing**.

If it is **not** possible to make the array _non-decreasing_ using any number of operations, return `-1`.

**Example 1:**
```
Input: nums = [25,7]
Output: 1
Explanation:
Using a single operation, 25 gets divided by 5 and `nums` becomes `[5, 7]`.
```
**Example 2:**
```
Input: nums = [7,7,6]
Output: -1
```
**Example 3:**
```
Input: nums = [1,1,1,1]
Output: 0
```
**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i] <= 106`

---
```java
// Java 287ms(Beats 55.34%), Time O(N), Space O(N)
class Solution {
    public int minOperations(int[] nums) {
        int ret = 0;
        int n = nums.length;
        for (int i = n - 1; i > 0; i--)
        {
            if (nums[i - 1] <= nums[i])
                continue;
            
            for (int j = 2; j <= nums[i]; j++)
                if (nums[i - 1] % j == 0)
                {
                    nums[i - 1] = j;
                    break;
                }
            
            if (nums[i - 1] > nums[i])
                return -1;
            ret++;
        }

        return ret;
    }
}
```
---

從最後一個`i`開始與前一個`i-1`相比
若`i-1 > i`, 則找出`i-1`的`greatest proper divisor`並除之
若除完後 仍然`>i` 則代表無解`return -1`


###### tags: `Leetcode` `Greedy`