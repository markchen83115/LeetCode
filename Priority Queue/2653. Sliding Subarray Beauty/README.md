# Leetcode - 2653. Sliding Subarray Beauty (M+)

[Leetcode](https://leetcode.com/problems/sliding-subarray-beauty/)

Given an integer array `nums` containing `n` integers, find the **beauty** of each subarray of size `k`.

The **beauty** of a subarray is the `xth`** smallest integer **in the subarray if it is **negative**, or `0` if there are fewer than `x` negative integers.

Return _an integer array containing _`n - k + 1` _integers, which denote the _**beauty**_ of the subarrays **in order** from the first index in the array._

-   A subarray is a contiguous **non-empty** sequence of elements within an array.
    

**Example 1:**

> **Input:** nums = [1,-1,-3,-2,3], k = 3, x = 2
> **Output:** [-1,-2,-2]
> **Explanation:** There are 3 subarrays with size k = 3. 
> The first subarray is `[1, -1, -3]` and the 2nd smallest negative integer is -1. 
> The second subarray is `[-1, -3, -2]` and the 2nd smallest negative integer is -2. 
> The third subarray is `[-3, -2, 3] `and the 2nd smallest negative integer is -2.

**Example 2:**

> **Input:** nums = [-1,-2,-3,-4,-5], k = 2, x = 2
> **Output:** [-1,-2,-3,-4]
> **Explanation:** There are 4 subarrays with size k = 2.
> For `[-1, -2]`, the 2nd smallest negative integer is -1.
> For `[-2, -3]`, the 2nd smallest negative integer is -2.
> For `[-3, -4]`, the 2nd smallest negative integer is -3.
> For `[-4, -5]`, the 2nd smallest negative integer is -4. 

**Example 3:**

> **Input:** nums = [-3,1,2,-3,0,-3], k = 2, x = 1
> **Output:** [-3,0,-3,-3,-3]
> **Explanation:** There are 5 subarrays with size k = 2**.**
> For `[-3, 1]`, the 1st smallest negative integer is -3.
> For `[1, 2]`, there is no negative integer so the beauty is 0.
> For `[2, -3]`, the 1st smallest negative integer is -3.
> For `[-3, 0]`, the 1st smallest negative integer is -3.
> For `[0, -3]`, the 1st smallest negative integer is -3.

**Constraints:**

-   `n == nums.length `
-   `1 <= n <= 105`
-   `1 <= k <= n`
-   `1 <= x <= k `
-   `-50 <= nums[i] <= 50 `

---
```java
// Java Sliding Window 33ms(Beats 70.18%), Time O(50*N), Space O(50)
class Solution {
    public int[] getSubarrayBeauty(int[] nums, int k, int x) {
        int n = nums.length;
        int[] ret = new int[n - k + 1];
        int[] freq = new int[51];
        for (int i = 0; i < n; i++)
        {
            if (nums[i] < 0)
                freq[-nums[i]]++;

            if (i >= k && nums[i - k] < 0)
                freq[-nums[i - k]]--;
            
            if (i >= k - 1)
            {
                int count = x;
                for (int p = 50; p >= 1; p--)
                {
                    count -= freq[p];
                    if (count <= 0)
                    {
                        ret[i - k + 1] = -p;
                        break;
                    }
                }
            }
        }

        return ret;
    }
}
```
---

#### 註: C++有mulitset可以使用, java則無

參考[【每日一题】LeetCode 2653. Sliding Subarray Beauty - YouTube](https://youtu.be/OfSkwAkE_R8)

本題如果利用`-50 <= nums[i] <= 50`的條件，那麼可以變得很容易。在這裡我們只講更一般的解法。

和`Dual PQ`的想法一樣，設計兩個有序容器，分別是裝「最小的x的元素」Set1，和「剩餘的元素」Set2。對於新元素nums[i]，我們需要操作的步驟是：

1. 判斷應該將nums[i]放入Set1或Set2. 如果比Set1的最大元素還大，就放入Set2；否則就將Set1的最大元素轉移到Set2，並將nums[i]放入Set1
2. 如果i>=x-1，輸出Set1裡的最大元素作為答案
3. 將nums[i-x+1]從集合中移除。需要判斷nums[i-x+1]此時在Set1裡還是Set2裡。如果是前者的話，需要將Set2裡的元素轉移一個過去


###### tags: `Leetcode` `Sorted Container` `Dual Multiset` `Priority Queue` `Dual PQ`