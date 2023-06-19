# Leetcode - 215. Kth Largest Element in an Array (M)

[Leetcode](https://leetcode.com/problems/kth-largest-element-in-an-array/description/)

Given an integer array `nums` and an integer `k`, return _the_ `kth` _largest element in the array_.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

You must solve it in `O(n)` time complexity.

**Example 1:**
```
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
```
**Example 2:**
```
Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
Output: 4
```
**Constraints:**

-   `1 <= k <= nums.length <= 105`
-   `-104 <= nums[i] <= 104`

---
```java
// Java 6ms(Beats 95.34%), Time O(N)~O(N^2), Space O(1) 
class Solution {
    public int findKthLargest(int[] nums, int k) {
        return quickSelect(nums, 0, nums.length - 1, k);
    }

    public int quickSelect(int[] nums, int a, int b, int k) {
        /*
        S S S S O O X X X L L L
                ^   ^   ^
                i   t   j
        */
        int pivot = nums[(a + b) / 2];
        int i = a, t = a, j = b;
        while (t <= j) {
            if (nums[t] < pivot) {
                swap(nums, i, t);
                t++;
                i++;
            } else if (nums[t] > pivot) {
                swap(nums, t, j);
                j--;
            } else {
                t++;
            }
        }

        /*
        S S S S O O O O L L L
        ^       ^     ^     ^  
        a       i     j     b
        */
        if (b - j >= k) 
            return quickSelect(nums, j + 1, b, k);
        else if (b - i + 1 >= k)
            return pivot;
        else
            return quickSelect(nums, a, i - 1, k - (b - i + 1));
    }

    public void swap(int[] nums, int a, int b) {
        int tmp = nums[a];
        nums[a] = nums[b];
        nums[b] = tmp;
    }
}

        /*
        S S S S O O O O L L L
           ^       ^      ^
           A       B      C

        if (C >= k) => find k-th largest in C
        else if (B+C >= k) => k-th largest is pivot
        else => find k-(b+c) th largest in A
        */



```
---

[wisdompeak/YouTube](https://www.youtube.com/watch?v=dMq9_EkfSEc)

看到尋找第K大的題目, 可使用`Quick Select`
平均為`O(N)`, 最差是`O(N^2)`

做法是類似題目Sort Color
```
/*
S S S S O O X X X L L L
        ^   ^   ^
        i   t   j
*/
```
看見比pivot小的丟到左邊, 比pivot大的丟到右邊
整理完後, 檢查目前比pivot大的數有幾個
```
/*
S S S S O O O O L L L
   ^       ^      ^
   A       B      C
*/
```
if (C >= k) => 在C裡面找第K大的數
else if (B+C >= k) => 第K大的數是pivot
else => 在A裡面找第k-(B+C)大的數 (已經被過濾掉B+C個數字)


###### tags: `Leetcode` `Binary Search` `Find K-th Element`
