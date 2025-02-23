# Leetcode - 713. Subarray Product Less Than K (M+)

[Leetcode](https://leetcode.com/problems/subarray-product-less-than-k/)

Given an array of integers `nums` and an integer `k`, return _the number of contiguous subarrays where the product of all the elements in the subarray is strictly less than _`k`.

**Example 1:**

> **Input:** nums = [10,5,2,6], k = 100
> **Output:** 8
> **Explanation:** The 8 subarrays that have product less than 100 are:
> [10], [5], [2], [6], [10, 5], [5, 2], [2, 6], [5, 2, 6]
> Note that [10, 5, 2] is not included as the product of 100 is not strictly less than k.

**Example 2:**

> **Input:** nums = [1,2,3], k = 0
> **Output:** 0

**Constraints:**

-   `1 <= nums.length <= 3 * 104`
-   `1 <= nums[i] <= 1000`
-   `0 <= k <= 106`

---
```java
// Java 5ms(Beats 22.89%), Time O(N), Space O(1)
// 固定左指針i 往右滑動右指針j
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        if (k < 1) return 0;
        int n = nums.length;
        long prod = 1;
        int ret = 0;
        int j = 0;
        for (int i = 0; i < n; i++)
        {
            if (j < i)
            {
                j = i;
                prod = 1;
            }

            while (j < n && prod * nums[j] < k)
            {
                prod *= nums[j];
                j++;
            }

            ret += j - i;

            prod /= nums[i];
        }

        return ret;
    }
}
```
```java
// Java 4ms(Beats 99.95%), Time O(N), Space O(1)
// 固定右指針i 往右滑動左指針j 不超過i
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        if (k <= 1) return 0;

        int n = nums.length;
        long prod = 1;
        int ret = 0;
        int j = 0;
        for (int i = 0; i < n; i++)
        {
            prod *= nums[i];

            while (prod >= k)
                prod /= nums[j++];

            ret += i - j + 1;
        }

        return ret;
    }
}
```
---

本題有很明顯的滑窗的特徵，所以基本想法是用雙指針。確保一個乘積小於k的最大窗口，這個窗口內可以構成subarray的數目就是j-i+1;

本題要注意的一點是，當nums[i]>k時，右指針動不了，而左指針依然會順移，所以可能會出現j<i的情況，此時只需要重置這兩個指針即可：

```java
if (j < i)
{
    j = i;
    product = 1;
}
```


###### tags: `Leetcode` `Two Pointers`