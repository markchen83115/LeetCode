# Leetcode - 2563. Count the Number of Fair Pairs (M+)

[Leetcode](https://leetcode.com/problems/count-the-number-of-fair-pairs/description/)

Given a **0-indexed** integer array `nums` of size `n` and two integers `lower` and `upper`, return _the number of fair pairs_.

A pair `(i, j)` is **fair **if:

-   `0 <= i < j < n`, and
-   `lower <= nums[i] + nums[j] <= upper`

**Example 1:**
```
Input: nums = [0,1,7,4,4,5], lower = 3, upper = 6
Output: 6
Explanation: There are 6 fair pairs: (0,3), (0,4), (0,5), (1,3), (1,4), and (1,5).
```
**Example 2:**
```
Input: nums = [1,7,9,2,5], lower = 11, upper = 11
Output: 1
Explanation: There is a single fair pair: (2,3).
```
**Constraints:**

-   `1 <= nums.length <= 105`
-   `nums.length == n`
-   `-109 <= nums[i] <= 109`
-   `-109 <= lower <= upper <= 109`

---
```java
// Java
class Solution {
    public long countFairPairs(int[] nums, int lower, int upper) {
        Arrays.sort(nums);
        return countLess(nums, upper) - countLess(nums, lower - 1);
    }

    public long countLess (int[] nums, int val) {
        long ret = 0;
        int i = 0, j = nums.length - 1;
        while (i < j) {
            if (nums[i] + nums[j] > val) {
                j--;
            } else {
                ret += j - i;
                i++;
            }
        }
        return ret;
    }
}
```
---

題目限制
-   `0 <= i < j < n`
-   `lower <= nums[i] + nums[j] <= upper`

先只看第二個限制, 若全搜尋時會找到
`nums[i] + nums[j]`以及`nums[j] + nums[i]`的答案
因此可將答案全搜尋後再除以2

再搭配`0 <= i < j < n`, 限制了`i != j`

所以將數列先進行排序, 
答案為`所有<= upper的個數 - 所有<= (lower - 1)的個數`, 因為要包含lower



###### tags: `Leetcode` `Binary Search`
