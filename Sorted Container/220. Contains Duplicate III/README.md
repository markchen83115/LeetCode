# Leetcode - 220. Contains Duplicate III (M)

[Leetcode](https://leetcode.com/problems/contains-duplicate-iii/)

You are given an integer array `nums` and two integers `indexDiff` and `valueDiff`.

Find a pair of indices `(i, j)` such that:

-   `i != j`,
-   `abs(i - j) <= indexDiff`.
-   `abs(nums[i] - nums[j]) <= valueDiff`, and

Return `true`_ if such pair exists or _`false`_ otherwise_.

**Example 1:**

> **Input:** nums = [1,2,3,1], indexDiff = 3, valueDiff = 0
> **Output:** true
> **Explanation:** We can choose (i, j) = (0, 3).
> We satisfy the three conditions:
> i != j --> 0 != 3
> abs(i - j) <= indexDiff --> abs(0 - 3) <= 3
> abs(nums[i] - nums[j]) <= valueDiff --> abs(1 - 1) <= 0

**Example 2:**

> **Input:** nums = [1,5,9,1,5,9], indexDiff = 2, valueDiff = 3
> **Output:** false
> **Explanation:** After trying all the possible pairs (i, j), we cannot satisfy the three conditions, so we return false.

**Constraints:**

-   `2 <= nums.length <= 105`
-   `-109 <= nums[i] <= 109`
-   `1 <= indexDiff <= nums.length`
-   `0 <= valueDiff <= 109`

---
```java
// Java 129ms(Beats 54.19%), Time O(NlogN), Space O(N)
class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int indexDiff, int valueDiff) {
        int n = nums.length;
        TreeMap<Integer, Integer> map = new TreeMap<>();
        for (int i = 0; i < n; i++)
        {
            if (i-indexDiff-1 >= 0)
                map.computeIfPresent(nums[i-indexDiff-1], (a, b) -> b > 1 ? b - 1 : null);
            
            Integer ceiling = map.ceilingKey(nums[i] - valueDiff);
            if (ceiling != null && Math.abs(nums[i] - ceiling) <= valueDiff)
                return true;
            
            map.merge(nums[i], 1, Integer::sum);
        }

        return false;
    }
}
```
---

對於`nums[i]`，將i之前長度為k的滑動視窗放入`TreeMap`中
只需要考察這個有序集合裡是否存在數值大小在`[nums[i]-t, nums[i]+t]`間的元素
我們用`map.ceilingKey(nums[i]-t)`找出第一個大於等於`nums[i]-t`的位置
只需要判斷該位置的元素是否存在，以及是否落在期望區間內
我們不需要再考察上界的位置


###### tags: `Leetcode` `Sorted Container`