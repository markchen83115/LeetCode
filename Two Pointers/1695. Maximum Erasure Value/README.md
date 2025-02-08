# Leetcode - 1695. Maximum Erasure Value (M-)

[Leetcode](https://leetcode.com/problems/maximum-erasure-value/)

You are given an array of positive integers `nums` and want to erase a subarray containing **unique elements**. The **score** you get by erasing the subarray is equal to the **sum** of its elements.

Return _the **maximum score** you can get by erasing **exactly one** subarray._

An array `b` is called to be a subarray of `a` if it forms a contiguous subsequence of `a`, that is, if it is equal to `a[l],a[l+1],...,a[r]` for some `(l,r)`.

**Example 1:**

> **Input:** nums = [4,2,4,5,6]
> **Output:** 17
> **Explanation:** The optimal subarray here is [2,4,5,6].

**Example 2:**

> **Input:** nums = [5,2,1,2,5,2,1,2,5]
> **Output:** 8
> **Explanation:** The optimal subarray here is [5,2,1] or [1,2,5].

**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i] <= 104`

---
```java
// Java 6ms(Beats 98.07%), Time O(N), Space O(N)
class Solution {
    public int maximumUniqueSubarray(int[] nums) {
        int n = nums.length;
        int sum = 0;
        int[] freq = new int[10005];
        int j = 0;
        int ret = 0;
        boolean duplicate = false;
        for (int i = 0; i < n; i++)
        {
            while (j < n && !duplicate)
            {
                sum += nums[j];
                if (++freq[nums[j]] == 2)
                    duplicate = true;
                j++;

                if (!duplicate)
                    ret = Math.max(ret, sum);
            }

            sum -= nums[i];
            if (--freq[nums[i]] == 1)
                duplicate = false;
        }

        return ret;
    }
}
```
---

典型雙指針題目, 若沒有重複則右指針持續向右
若有重複則右指針停止, 開始移動左指針縮小範圍直到沒有重複


###### tags: `Leetcode` `Two Pointers` `Sliding window : Distinct Characters`