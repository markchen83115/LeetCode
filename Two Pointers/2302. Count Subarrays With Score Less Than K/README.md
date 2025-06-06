# Leetcode - 2302. Count Subarrays With Score Less Than K (M+)

[Leetcode](https://leetcode.com/problems/count-subarrays-with-score-less-than-k/)

The **score** of an array is defined as the **product** of its sum and its length.

-   For example, the score of `[1, 2, 3, 4, 5]` is `(1 + 2 + 3 + 4 + 5) * 5 = 75`.

Given a positive integer array `nums` and an integer `k`, return _the **number of non-empty subarrays** of_ `nums` _whose score is **strictly less** than_ `k`.

A **subarray** is a contiguous sequence of elements within an array.

**Example 1:**

> **Input:** nums = [2,1,4,3,5], k = 10
> **Output:** 6
> **Explanation:**
> The 6 subarrays having scores less than 10 are:
> - [2] with score 2 * 1 = 2.
> - [1] with score 1 * 1 = 1.
> - [4] with score 4 * 1 = 4.
> - [3] with score 3 * 1 = 3. 
> - [5] with score 5 * 1 = 5.
> - [2,1] with score (2 + 1) * 2 = 6.
> Note that subarrays such as [1,4] and [4,3,5] are not considered because their scores are 10 and 36 respectively, while we need scores strictly less than 10.

**Example 2:**

> **Input:** nums = [1,1,1], k = 5
> **Output:** 5
> **Explanation:**
> Every subarray except [1,1,1] has a score less than 5.
> [1,1,1] has a score (1 + 1 + 1) * 3 = 9, which is greater than 5.
> Thus, there are 5 subarrays having scores less than 5.

**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i] <= 105`
-   `1 <= k <= 1015`

---
```java
// Java 2ms(Beats 100.00%), Time O(N), Space O(1)
class Solution {
    public long countSubarrays(int[] nums, long k) {
        long ret = 0;
        long sum = 0;
        for (int i = 0, j = 0; i < nums.length; i++)
        {
            sum += nums[i];

            while (sum * (i - j + 1) >= k)
            {
                sum -= nums[j];
                j++;
            }

            ret += i - j + 1;
        }

        return ret;
    }
}
```
---

此問題類似於[713. Subarray Product Less Than K](https://leetcode.com/problems/subarray-product-less-than-k/)
使用滑窗紀錄滑窗總和
subarray分數為`sum * （i - j + 1) < k`
若分數`<= k`, 則移動左指針`j`來縮小範圍


###### tags: `Leetcode` `Two Pointers` `Sliding window`