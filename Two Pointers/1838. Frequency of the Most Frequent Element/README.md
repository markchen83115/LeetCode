# Leetcode - 1838. Frequency of the Most Frequent Element (H-)

[Leetcode](https://leetcode.com/problems/frequency-of-the-most-frequent-element/)

The **frequency** of an element is the number of times it occurs in an array.

You are given an integer array `nums` and an integer `k`. In one operation, you can choose an index of `nums` and increment the element at that index by `1`.

Return _the **maximum possible frequency** of an element after performing **at most** _`k`_ operations_.

**Example 1:**

> **Input:** nums = [1,2,4], k = 5
> **Output:** 3 **Explanation:** Increment the first element three times and the second element two times to make nums = [4,4,4].
> 4 has a frequency of 3.

**Example 2:**

> **Input:** nums = [1,4,8,13], k = 5
> **Output:** 2
> **Explanation:** There are multiple optimal solutions:
> - Increment the first element three times to make nums = [4,4,8,13]. 4 has a frequency of 2.
> - Increment the second element four times to make nums = [1,8,8,13]. 8 has a frequency of 2.
> - Increment the third element five times to make nums = [1,4,13,13]. 13 has a frequency of 2.

**Example 3:**

> **Input:** nums = [3,9,6], k = 2
> **Output:** 1

**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i] <= 105`
-   `1 <= k <= 105`

---
```java
// Java 28ms(Beats 98.54%), Time O(N), Space O(1)
class Solution {
    public int maxFrequency(int[] nums, int k) {
        Arrays.sort(nums);
        int n = nums.length;
        int ret = 0, j = 0;
        long sum = 0;
        for (int i = 0; i < n; i++)
        {
            while (j < n && (long) nums[j] * (j - i) - sum <= k)
            {
                sum += nums[j];
                j++;
            }

            ret = Math.max(ret, j - i);

            sum -= nums[i];
        }

        return ret;
    }
}
```
---

在有限的操作數下, 要將每個數透過+1來組成最高頻率的數, 必然是找最接近的一團數字最有機會
因此將nums先排序, 再透過sliding window尋找答案

因為操作只能+1, 因此最大的數必然是最高頻率的數
如何求得總共需要多少操作數呢?
`[1 2 2 4 5] => [5 5 5 5 5]`
可透過`nums[j] * (j - i + 1) - sum`求得

因此若操作數還有剩, 則繼續右端點繼續向右擴大滑窗
若操作數已不足, 則左端點向右縮小滑窗


###### tags: `Leetcode` `Two Pointers` `Sliding window`