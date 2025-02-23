# Leetcode - 2537. Count the Number of Good Subarrays (M+)

[Leetcode](https://leetcode.com/problems/count-the-number-of-good-subarrays/)

Given an integer array `nums` and an integer `k`, return _the number of **good** subarrays of_ `nums`.

A subarray `arr` is **good** if there are **at least **`k` pairs of indices `(i, j)` such that `i < j` and `arr[i] == arr[j]`.

A **subarray** is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

> **Input:** nums = [1,1,1,1,1], k = 10
> **Output:** 1
> **Explanation:** The only good subarray is the array nums itself.

**Example 2:**

> **Input:** nums = [3,1,4,3,2,2,4], k = 2
> **Output:** 4
> **Explanation:** There are 4 different good subarrays:
> - [3,1,4,3,2,2] that has 2 pairs.
> - [3,1,4,3,2,2,4] that has 3 pairs.
> - [1,4,3,2,2,4] that has 2 pairs.
> - [4,3,2,2,4] that has 2 pairs.

**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i], k <= 109`

---
```java
// Java 55ms(Beats 73.47%), Time O(N), Space O(N)
class Solution {
    public long countGood(int[] nums, int k) {
        long ret = 0;
        int n = nums.length;
        HashMap<Integer, Integer> map = new HashMap<>(); // num, freq
        int j = 0;
        for (int i = 0; i < n; i++)
        {
            while (j < n && k > 0)
            {
                k -= map.getOrDefault(nums[j], 0);
                map.merge(nums[j], 1, Integer::sum);
                j++;
            }

            if (k <= 0)
                ret += n - j + 1;
            
            map.merge(nums[i], -1, Integer::sum);
            k += map.get(nums[i]);
        }

        return ret;
    }
}
```
---

pairs的計算取決於每種數字出現的頻率。如果一個subarray裡某個數值出現了m次，那麼它就能貢獻`m/(m-1)/2`個pairs. 很明顯，視窗越大，就能夠得到越多的pairs。

因此我們遍歷每個元素`i`作為窗口的左端點，然後探索右端點的位置j使得窗口內恰好能有k對pairs，那麼意味著右端點從`j`到`n-1`都是可行的，故有`n-j`個以`i`為左端點的合法滑窗。每一個回合，向右移動`i`一位，`j`必然是單調右移的。故雙指針`O(N)`時間可解。


###### tags: `Leetcode` `Two Pointers` `Sliding window : Distinct Characters`