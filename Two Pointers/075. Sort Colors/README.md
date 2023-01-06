# Leetcode - 75. Sort Colors (M+)
[Leetcode](https://leetcode.com/problems/sort-colors/)

Given an array `nums` with `n` objects colored red, white, or blue, sort them **[in-place](https://en.wikipedia.org/wiki/In-place_algorithm) **so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers `0`, `1`, and `2` to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

**Example 1:**
```
Input: nums = [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```
**Example 2:**

```
Input: nums = [2,0,1]
Output: [0,1,2]
```


**Constraints:**

-   `n == nums.length`
-   `1 <= n <= 300`
-   `nums[i]` is either `0`, `1`, or `2`.

---
```java
//Java  
class Solution {  
    public void sortColors(int[] nums) {  
        //red, white, and blue.  
        //0 1 2  
        int i = 0;  
        int l = 0, r = nums.length - 1;  
        while (i <= r) {  
            if (nums[i] == 0) {  
                //swap to front  
                nums[i] = nums[l];  
                nums[l] = 0;  
                l++;  
                i++;  
            } else if (nums[i] == 2) {  
                //swap to back  
                nums[i] = nums[r];  
                nums[r] = 2;  
                r--;  
            } else {  
                i++;  
            }  
        }  
    }  
}
```
---

遇到0放前面, 遇到2放後面, 遇到1跳過

注意: 當遇到2時, 與tail位置交換後, 需要再重新檢查相同的位置  
因為不知道換過來的數字是多少




###### tags: `Leetcode` `Two Pointers`