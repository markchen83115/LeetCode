# Leetcode - 31. Next Permutation (M)

[Leetcode](https://leetcode.com/problems/next-permutation/description/)

A **permutation** of an array of integers is an arrangement of its members into a sequence or linear order.

-   For example, for `arr = [1,2,3]`, the following are all the permutations of `arr`: `[1,2,3], [1,3,2], [2, 1, 3], [2, 3, 1], [3,1,2], [3,2,1]`.

The **next permutation** of an array of integers is the next lexicographically greater permutation of its integer. More formally, if all the permutations of the array are sorted in one container according to their lexicographical order, then the **next permutation** of that array is the permutation that follows it in the sorted container. If such arrangement is not possible, the array must be rearranged as the lowest possible order (i.e., sorted in ascending order).

-   For example, the next permutation of `arr = [1,2,3]` is `[1,3,2]`.
-   Similarly, the next permutation of `arr = [2,3,1]` is `[3,1,2]`.
-   While the next permutation of `arr = [3,2,1]` is `[1,2,3]` because `[3,2,1]` does not have a lexicographical larger rearrangement.

Given an array of integers `nums`, _find the next permutation of_ `nums`.

The replacement must be **[in place](http://en.wikipedia.org/wiki/In-place_algorithm)** and use only constant extra memory.

**Example 1:**
```
Input: nums = [1,2,3]
Output: [1,3,2]
```
**Example 2:**
```
Input: nums = [3,2,1]
Output: [1,2,3]
```
**Example 3:**
```
Input: nums = [1,1,5]
Output: [1,5,1]
```
**Constraints:**

-   `1 <= nums.length <= 100`
-   `0 <= nums[i] <= 100`

---
```java
// Java 0ms(Beats 100%), Time O(N), Space O(N)
class Solution {
    public void nextPermutation(int[] nums) {
        if (nums.length == 1)  return;
        int n = nums.length;
        int i = n - 2;
        while (i >= 0 && nums[i] >= nums[i + 1]) {  // Find 1st id i that breaks descending order
            i--;
        }
        if (i >= 0) {   // If not entirely descending
            int j = n - 1;
            while (j > i && nums[i] >= nums[j]) {   // Find rightmost first larger id j
                j--;
            }
            swap(nums, i , j);  // Switch i and j
        }
        reverse(nums, i + 1, n - 1);    // Reverse the descending sequence
    }

    public void swap(int[] nums, int i , int j) {
        nums[i] ^= nums[j];
        nums[j] ^= nums[i];
        nums[i] ^= nums[j];
    }

    public void reverse(int[] nums, int i , int j) {
        while (i < j) {
            swap(nums, i++, j--);
        }
    }
}
```
```java
// Java 0ms(Beats 100%), Time O(N), Space O(N)
class Solution {
    public void nextPermutation(int[] nums) {
        int i = nums.length - 1;
        while(i >= 1 && nums[i - 1] >= nums[i])
            i--;

        // 如果整個數列為遞增數列 -> 答案為從小排到大 (example 2)
        if (i == 0)
        {
            Arrays.sort(nums);
            return;
        }

        i--;
        // 找到第一個大於nums[i]的數字 並將其交換, 剩餘數字由小排到大
        // 1 5 3 5 4 3 2
        // 1 5 4 5 3 3 2
        int j = nums.length - 1;
        while (nums[i] >= nums[j])   
            j--;
        
        swap(nums, i ,j);
        sort(nums, i + 1, nums.length - 1);
    }

    void swap(int[] nums, int i, int j)
    {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }

    void sort(int[] nums, int i, int j)
    {
        while (i < j)
            swap(nums, i++, j--);
    }
}

```

---
[參考官方解答](https://leetcode.com/problems/next-permutation/solutions/127524/next-permutation/?orderBy=most_votes)
![](https://leetcode.com/media/original_images/31_Next_Permutation.gif)



###### tags: `Leetcode` `Greedy` `Lexicographical Sequence`