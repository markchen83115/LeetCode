# Leetcode - 912. Sort an Array (M)

[Leetcode](https://leetcode.com/problems/sort-an-array/)

Given an array of integers `nums`, sort the array in ascending order and return it.

You must solve the problem **without using any built-in** functions in `O(nlog(n))` time complexity and with the smallest space complexity possible.

**Example 1:**
```
Input: nums = [5,2,3,1]
Output: [1,2,3,5]
Explanation: After sorting the array, the positions of some numbers are not changed (for example, 2 and 3), while the positions of other numbers are changed (for example, 1 and 5).
```
**Example 2:**
```
Input: nums = [5,1,1,2,0,0]
Output: [0,0,1,1,2,5]
Explanation: Note that the values of nums are not necessairly unique.
```
**Constraints:**

-   `1 <= nums.length <= 5 * 104`
-   `-5 * 104 <= nums[i] <= 5 * 104`

---
```java
// Java merge sort 30ms(Beats 39.29%), Time O(NlogN), Space O(N)
class Solution {
    public int[] sortArray(int[] nums) {
        if (nums.length == 0)
            return nums;
        mergeSort(nums, 0, nums.length - 1);
        return nums;
    }

    void mergeSort(int[] nums, int l, int r) {
        if (l >= r)
            return;
        int mid = l + (r - l) / 2;
        mergeSort(nums, l, mid);
        mergeSort(nums, mid + 1, r);
        merge(nums, l, r);
    }

    void merge(int[] nums, int l, int r) {
        int mid = l + (r - l) / 2;
        int[] tmp = new int[r - l + 1];
        int i = l, j = mid + 1, k = 0;
        while (i <= mid || j <= r) 
        {
            if (i > mid || j <= r && nums[i] > nums[j]) 
            {
                tmp[k++] = nums[j++];
            } 
            else 
            {
                tmp[k++] = nums[i++];
            }
        }
        System.arraycopy(tmp, 0, nums, l, r - l + 1);
    }
}
```
---

###### tags: `Leetcode` `Greedy` `Sort`