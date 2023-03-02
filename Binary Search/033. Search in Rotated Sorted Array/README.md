# Leetcode - 33. Search in Rotated Sorted Array (M)

[Leetcode](https://leetcode.com/problems/search-in-rotated-sorted-array/)

There is an integer array `nums` sorted in ascending order (with **distinct** values).

Prior to being passed to your function, `nums` is **possibly rotated** at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.

Given the array `nums` **after** the possible rotation and an integer `target`, return _the index of _`target`_ if it is in _`nums`_, or _`-1`_ if it is not in _`nums`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**
```
Input: nums = [4,5,6,7,0,1,2], target = 0  
Output: 4
```
**Example 2:**
```
Input: nums = [4,5,6,7,0,1,2], target = 3  
Output: -1
```
**Example 3:**
```
Input: nums = [1], target = 0  
Output: -1
```
**Constraints:**

-   `1 <= nums.length <= 5000`
-   `-104 <= nums[i] <= 104`
-   All values of `nums` are **unique**.
-   `nums` is an ascending array that is possibly rotated.
-   `-104 <= target <= 104`

---

```java
// Java
class Solution {  
    public int search(int[] nums, int target) {  
        //[7,8,9,10,0,1,2,3,4,5,6]  
        int low = 0, high = nums.length - 1, mid=0;  
        while (low <= high) {  
            mid = low + (high - low) / 2;  //避免overflow  
            if (nums[mid] == target) {  
                return mid;  
            }  
            if (nums[low] <= nums[mid]) {  
                //確認target在左半邊 其餘皆右半  
                if (nums[low] <= target && target < nums[mid]) {  
                    high = mid - 1;  
                } else {  
                    low = mid + 1;  
                }  
            } else {  
                //確認target在右半邊 其餘皆左半  
                if (nums[mid] < target && target <= nums[high]) {  
                    low = mid + 1;  
                } else {  
                    high = mid - 1;  
                }  
            }  
        }  
        return -1;  
    }  
}
```

---

利用Binary Search  
調整target在左半邊或右半邊的判斷式

Binary Search的mid使用 `**mid = low + (high - low) / 2**` 可避免overflow  
不使用`mid = (low + high) / 2`

利用low ≤ mid判斷這兩種狀況  
`[7,8,9,10,0,1,2,3,4,5,6]`  
`[3,4,5,6,7,8,9,10,0,1,2]`

1.  `[7,8,9,10,0,***1**,2,3,4,5,6]`  
    若`1 < target ≤ 6`, 則為右半, 其餘為左半
2.  `[3,4,5,6,7,***8**,9,10,0,1,2]`  
    若`3 ≤ target < 8`, 則為左半, 其餘為右半


###### tags: `Leetcode` `Binary Search`
