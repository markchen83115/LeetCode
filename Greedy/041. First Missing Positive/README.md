# Leetcode - 41. First Missing Positive (H)

[Leetcode](https://leetcode.com/problems/first-missing-positive/description/)

Given an unsorted integer array `nums`, return the smallest missing positive integer.

You must implement an algorithm that runs in `O(n)` time and uses constant extra space.

**Example 1:**
```
Input: nums = [1,2,0]
Output: 3
Explanation: The numbers in the range [1,2] are all in the array.
```
**Example 2:**
```
Input: nums = [3,4,-1,1]
Output: 2
Explanation: 1 is in the array but 2 is missing.
```
**Example 3:**
```
Input: nums = [7,8,9,11,12]
Output: 1
Explanation: The smallest positive integer 1 is missing.
```
**Constraints:**

-   `1 <= nums.length <= 105`
-   `-231 <= nums[i] <= 231 - 1`

---
```java
// Java
class Solution {
    public int firstMissingPositive(int[] nums) {
        int n = nums.length;
        int i = 0;
        while (i < n) {
            if (0 < nums[i] && nums[i] <= n && nums[nums[i] - 1] != nums[i]) {
                swap(nums, i, nums[i] - 1);
            } else {
                i++;
            }
        }
        for (int j = 0; j < n; j++) {
            if (nums[j] != j + 1)
                return j + 1;
        }
        return n + 1;
    }

    public void swap(int[] nums, int i, int j) {
        nums[i] ^= nums[j];
        nums[j] ^= nums[i];
        nums[i] ^= nums[j];
    }
}
```
---

利用Indexing Sort來處理, `nums[index] = index + 1`
```
nums : 1 2 3 4 5
index: 0 1 2 3 4
```

需要注意為if判斷式`nums[nums[i] - 1] != nums[i]`滿足時才交換
是由`if (nums[i] != i + 1 && nums[i] != nums[nums[i] - 1])`而來

因為當`nums[i] != i + 1`時, 需要交換到`nums[i] - 1`的index位置
ex: `nums[1] = 5`, 則5需要被交換到`nums[4]`的位置

但若`nums[4]`已經是5則不交換, 代表已經排好, 否則為無限迴圈, ex: `nums = [1, 1]`


###### tags: `Leetcode` `Greedy` `Indexing Sort`
