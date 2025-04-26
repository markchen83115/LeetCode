# Leetcode - 2444. Count Subarrays With Fixed Bounds (M+)

[Leetcode](https://leetcode.com/problems/count-subarrays-with-fixed-bounds/)

You are given an integer array `nums` and two integers `minK` and `maxK`.

A **fixed-bound subarray** of `nums` is a subarray that satisfies the following conditions:

-   The **minimum** value in the subarray is equal to `minK`.
-   The **maximum** value in the subarray is equal to `maxK`.

Return _the **number** of fixed-bound subarrays_.

A **subarray** is a **contiguous** part of an array.

**Example 1:**

> **Input:** nums = [1,3,5,2,7,5], minK = 1, maxK = 5
> **Output:** 2
> **Explanation:** The fixed-bound subarrays are [1,3,5] and [1,3,5,2].

**Example 2:**

> **Input:** nums = [1,1,1,1], minK = 1, maxK = 1
> **Output:** 10
> **Explanation:** Every subarray of nums is a fixed-bound subarray. There are 10 possible subarrays.

**Constraints:**

-   `2 <= nums.length <= 105`
-   `1 <= nums[i], minK, maxK <= 106`

---
```java
// Java 7ms(Beats 95.85%), Time O(N), Space O(1)
class Solution {
    public long countSubarrays(int[] nums, int minK, int maxK) {
        long ret = 0;
        int n = nums.length;
        int prevMin = -1;
        int prevMax = -1;
        int prevBad = -1;
        for (int i = 0; i < n; i++)
        {
            if (nums[i] < minK || nums[i] > maxK)
            {
                prevBad = i;
                continue;
            }

            if (nums[i] == minK)
                prevMin = i;
            if (nums[i] == maxK)
                prevMax = i;
            
            ret += Math.max(0L, Math.min(prevMin, prevMax) - prevBad);
        }

        return ret;
    }
}
```
---

參考[【每日一题】LeetCode 2444. Count Subarrays With Fixed Bounds - YouTube](https://www.youtube.com/watch?v=aekkfcxEWYY)

本題的關鍵在於掌握好「數subarray」的訣竅
我們通常都是固定一個端點，查看另一個端點可以在哪些地方

我們考慮如果這個subarray的右邊是nums[i]，那麼這個subarray的左端點之後必須包含至少一個minK和maxK
所以我們只要知道i左邊最近的minK和maxK，取兩者的較小值j，那麼[j:i]就是以i結尾的、最短的符合條件的subarray
左端點從j往左延伸的話，這個subarray依然有效
但特別注意，左端點不能延伸到非法的區域，即小於minK或大於maxK的地方，所以實際左端點移動的範圍是j-boundary，其中boundary是i之前最近的非法位置


###### tags: `Leetcode` `Others` `Count Subarray by Element`