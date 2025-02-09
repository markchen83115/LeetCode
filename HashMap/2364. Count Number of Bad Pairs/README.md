# Leetcode - 2364. Count Number of Bad Pairs (M)

[Leetcode](https://leetcode.com/problems/count-number-of-bad-pairs/)

You are given a **0-indexed** integer array `nums`. A pair of indices `(i, j)` is a **bad pair** if `i < j` and `j - i != nums[j] - nums[i]`.

Return_ the total number of **bad pairs** in _`nums`.

**Example 1:**

> **Input:** nums = [4,1,3,3]
> **Output:** 5
> **Explanation:** The pair (0, 1) is a bad pair since 1 - 0 != 1 - 4.
> The pair (0, 2) is a bad pair since 2 - 0 != 3 - 4, 2 != -1.
> The pair (0, 3) is a bad pair since 3 - 0 != 3 - 4, 3 != -1.
> The pair (1, 2) is a bad pair since 2 - 1 != 3 - 1, 1 != 2.
> The pair (2, 3) is a bad pair since 3 - 2 != 3 - 3, 1 != 0.
> There are a total of 5 bad pairs, so we return 5.

**Example 2:**

> **Input:** nums = [1,2,3,4,5]
> **Output:** 0
> **Explanation:** There are no bad pairs.

**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i] <= 109`

---
```java
class Solution {
    public long countBadPairs(int[] nums) {
        int n = nums.length;
        long ret = 0;
        HashMap<Integer, Integer> prefix = new HashMap<>();
        for (int i = 0; i < n; i++)
        {
            ret += i - prefix.getOrDefault(i - nums[i], 0);
            prefix.merge(i - nums[i], 1, Integer::sum);
        }

        return ret;
    }
}
```
---

題目要求`j - i != nums[j] - nums[i]`的bad pair
先找出good pair `j - i == nums[j] - nums[i]`
利用 `bad pair = 全部 - good pair`

`j - i == nums[j] - nums[i]` 轉為 `j - nums[j] == i - nums[i]`
因此可利用prefix來計算



###### tags: `Leetcode` `HashMap` `Hash+Prefix`