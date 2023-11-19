# Leetcode - 1838. Frequency of the Most Frequent Element (H-)

[Leetcode](https://leetcode.com/problems/frequency-of-the-most-frequent-element/)

The **frequency** of an element is the number of times it occurs in an array.

You are given an integer array `nums` and an integer `k`. In one operation, you can choose an index of `nums` and increment the element at that index by `1`.

Return _the **maximum possible frequency** of an element after performing **at most** _`k`_ operations_.

**Example 1:**
```
Input: nums = [1,2,4], k = 5
Output: 3 Explanation: Increment the first element three times and the second element two times to make nums = [4,4,4].
4 has a frequency of 3.
```
**Example 2:**
```
Input: nums = [1,4,8,13], k = 5
Output: 2
Explanation: There are multiple optimal solutions:
- Increment the first element three times to make nums = [4,4,8,13]. 4 has a frequency of 2.
- Increment the second element four times to make nums = [1,8,8,13]. 8 has a frequency of 2.
- Increment the third element five times to make nums = [1,4,13,13]. 13 has a frequency of 2.
```
**Example 3:**
```
Input: nums = [3,9,6], k = 2
Output: 1
```
**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i] <= 105`
-   `1 <= k <= 105`

---
```java
// Java 29ms(Beats 72.34%), Time O(NlogN), Space O(1)
class Solution {
    public int maxFrequency(int[] nums, int k) {
        int i = 0, n = nums.length;
        long sum = 0;
        int ret = 0;

        Arrays.sort(nums);
        for (int j = 0; j < n; j++)
        {
            sum += nums[j];
            while (sum + k < nums[j] * (j - i + 1))
            {
                sum -= nums[i];
                i++;
            }
            ret = Math.max(ret, j - i + 1);
        }
        return ret;
    }
}
```
---
檢查當前window是否符合條件
`sum + k < (long)A[j] * (j - i + 1)`
```
範例：
[i] + [i+1] + [i+2] + [i+3] + [j]可用k個操作,
來達成[j] + [j] + [j] + [j] + [j], 也就是 5 * [j]
```


###### tags: `Leetcode` `Two Pointers` `Sliding window`