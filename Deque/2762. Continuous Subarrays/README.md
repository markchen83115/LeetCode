# Leetcode - 2762. Continuous Subarrays (M+)

[Leetcode](https://leetcode.com/problems/continuous-subarrays/)

You are given a **0-indexed** integer array `nums`. A subarray of `nums` is called **continuous** if:

-   Let `i`, `i + 1`, ..., `j` be the indices in the subarray. Then, for each pair of indices `i <= i1, i2 <= j`, `0 <= |nums[i1] - nums[i2]| <= 2`.

Return _the total number of **continuous** subarrays._

A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

> **Input:** nums = [5,4,2,4]
> **Output:** 8
> **Explanation:** 
> Continuous subarray of size 1: [5], [4], [2], [4].
> Continuous subarray of size 2: [5,4], [4,2], [2,4].
> Continuous subarray of size 3: [4,2,4].
> Thereare no subarrys of size 4.
> Total continuous subarrays = 4 + 3 + 1 = 8.
> It can be shown that there are no more continuous subarrays.

**Example 2:**

> **Input:** nums = [1,2,3]
> **Output:** 6
> **Explanation:** 
> Continuous subarray of size 1: [1], [2], [3].
> Continuous subarray of size 2: [1,2], [2,3].
> Continuous subarray of size 3: [1,2,3].
> Total continuous subarrays = 3 + 2 + 1 = 6.

**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i] <= 109`

---
```java
// Java 20ms(Beats 72.22%), Time O(N), Space O(N)
class Solution {
    public long continuousSubarrays(int[] nums) {
        long ret = 0;
        Deque<Integer> maxQue = new ArrayDeque<>();
        Deque<Integer> minQue = new ArrayDeque<>();
        int n = nums.length;
        // [i : j] 以j往右走
        int i = 0;
        for (int j = 0; j < n; j++)
        {
            while (!maxQue.isEmpty() && nums[maxQue.peekLast()] <= nums[j])
                maxQue.pollLast();
            maxQue.offerLast(j);

            while (!minQue.isEmpty() && nums[minQue.peekLast()] >= nums[j])
                minQue.pollLast();
            minQue.offerLast(j);

            while (nums[maxQue.peekFirst()] - nums[minQue.peekFirst()] > 2)
            {
                if (!maxQue.isEmpty() && maxQue.peekFirst() <= i)
                    maxQue.pollFirst();
                if (!minQue.isEmpty() && minQue.peekFirst() <= i)
                    minQue.pollFirst();
                i++;
            }

            ret += j - i + 1;
        }

        return ret;
    }
}
```
---
[【每日一题】LeetCode 2762. Continuous Subarrays - YouTube](https://youtu.be/crm-mMf9Kn4)

看到subarray內的數字要在某個範圍大小內
則利用Deque來維護最大最小值
最大值: 維護遞減數列, 第一個為最大值
最小值: 維護遞增數列, 第一個為最小值

###### tags: `Leetcode` `Deque`