# Leetcode - 1671. Minimum Number of Removals to Make Mountain Array (M+)

[Leetcode](https://leetcode.com/problems/minimum-number-of-removals-to-make-mountain-array/)

You may recall that an array `arr` is a **mountain array** if and only if:

-   `arr.length >= 3`
-   There exists some index `i` (**0-indexed**) with `0 < i < arr.length - 1` such that:
    -   `arr[0] < arr[1] < ... < arr[i - 1] < arr[i]`
    -   `arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`

Given an integer array `nums`​​​, return _the **minimum** number of elements to remove to make _`nums_​​​_` _a **mountain array**._

**Example 1:**

> **Input:** nums = [1,3,1]
> **Output:** 0
> **Explanation:** The array itself is a mountain array so we do not need to remove any elements.

**Example 2:**

> **Input:** nums = [2,1,1,5,6,2,3,1]
> **Output:** 3
> **Explanation:** One solution is to remove the elements at indices 0, 1, and 5, making the array nums = [1,5,6,3,1].

**Constraints:**

-   `3 <= nums.length <= 1000`
-   `1 <= nums[i] <= 109`
-   It is guaranteed that you can make a mountain array out of `nums`.

---
```java
// Java 35ms(Beats 74.29%), Time O(N^2), Space O(N)
class Solution {
    public int minimumMountainRemovals(int[] nums) {
        int n = nums.length;
        int[] dpGreater = new int[n];   // 以i為開頭的最大遞增長度
        int[] dpSmaller = new int[n];   // 以i為結尾的最大遞減長度
        Arrays.fill(dpGreater, 1);
        Arrays.fill(dpSmaller, 1);
        for (int i = 1; i < n; i++)
            for (int j = i - 1; j >= 0; j--)
                if (nums[j] < nums[i])
                    dpGreater[i] = Math.max(dpGreater[i], 1 + dpGreater[j]);

         for (int i = n - 2; i >= 0; i--)
            for (int j = i + 1; j < n; j++)
                if (nums[i] > nums[j])
                    dpSmaller[i] = Math.max(dpSmaller[i], 1 + dpSmaller[j]);

        int longest = 0;
        for (int i = 1; i < n - 1; i++)
            if (dpGreater[i] > 1 && dpSmaller[i] > 1)
                longest = Math.max(longest, dpGreater[i] + dpSmaller[i] - 1);
        
        return n - longest;
    }
}
```
---

分別求出
以每個點為開頭的最大遞增序列`Longest Increasing Subsequence (LIS)`
以每個點為結尾的最大遞減序列`Longest Decreasing Subsequence (LDS)`
最後計算以每個點往兩側的`最大mountain array`
注意 此題限制LIS與LDS長度最少為2

###### tags: `Leetcode` `Greedy` `Three-pass`