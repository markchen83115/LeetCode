# Leetcode - 209. Minimum Size Subarray Sum (M)

[Leetcode](https://leetcode.com/problems/minimum-size-subarray-sum/)

Given an array of positive integers `nums` and a positive integer `target`, return _the **minimal length** of a _subarray_ whose sum is greater than or equal to_ `target`. If there is no such subarray, return `0` instead.

**Example 1:**

> **Input:** target = 7, nums = [2,3,1,2,4,3]
> **Output:** 2
> **Explanation:** The subarray [4,3] has the minimal length under the problem constraint.

**Example 2:**

> **Input:** target = 4, nums = [1,4,4]
> **Output:** 1

**Example 3:**

> **Input:** target = 11, nums = [1,1,1,1,1,1,1,1]
> **Output:** 0

**Constraints:**

-   `1 <= target <= 109`
-   `1 <= nums.length <= 105`
-   `1 <= nums[i] <= 104`

**Follow up:** If you have figured out the `O(n)` solution, try coding another solution of which the time complexity is `O(n log(n))`.

---
```java
// Java 1ms(Beats 99.84%), Time O(N), Space O(1)
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int n = nums.length;
        int j = 0;
        int sum = 0;
        int ret = n + 1;
        for (int i = 0; i < n; i++)
        {
            while (j < n && sum < target)
                sum += nums[j++];
            
            if (sum >= target)
                ret = Math.min(ret, j - i);
            
            sum -= nums[i];
        }

        return ret < n + 1 ? ret : 0;
    }
}
```
---

看到`subarray`先想到滑動窗口
當`sum < target`時, `j++`向右滑, 嘗試使得`sum >= target`
當`sum >= target`時, `i++`向右滑, 嘗試縮短`subarray`長度


###### tags: `Leetcode` `Two Pointers`