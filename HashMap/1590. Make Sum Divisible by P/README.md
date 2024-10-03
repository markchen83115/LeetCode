# Leetcode - 1590. Make Sum Divisible by P (M+)

[Leetcode](https://leetcode.com/problems/make-sum-divisible-by-p/)

Given an array of positive integers `nums`, remove the **smallest** subarray (possibly **empty**) such that the **sum** of the remaining elements is divisible by `p`. It is **not** allowed to remove the whole array.

Return _the length of the smallest subarray that you need to remove, or _`-1`_ if it's impossible_.

A **subarray** is defined as a contiguous block of elements in the array.

**Example 1:**
```
Input: nums = [3,1,4,2], p = 6
Output: 1
Explanation: The sum of the elements in nums is 10, which is not divisible by 6. We can remove the subarray [4], and the sum of the remaining elements is 6, which is divisible by 6.
```
**Example 2:**
```
Input: nums = [6,3,5,2], p = 9
Output: 2
Explanation: We cannot remove a single element to get a sum divisible by 9. The best way is to remove the subarray [5,2], leaving us with [6,3] with sum 9.
```
**Example 3:**
```
Input: nums = [1,2,3], p = 3
Output: 0
Explanation: Here the sum is 6. which is already divisible by 3. Thus we do not need to remove anything.
```
**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i] <= 109`
-   `1 <= p <= 109`

---
```java
// Java 25ms(Beats 66.67%), Time O(N), Space O(N)
class Solution {
    public int minSubarray(int[] nums, int p) {
        int need = 0;
        for (int num : nums)
            need = (need + num) % p;
        
        if (need == 0)
            return 0;
        
        HashMap<Integer, Integer> map = new HashMap<>(); // (sum, index)
        int prefix = 0;
        map.put(0, -1);
        int n = nums.length;
        int ret = n;
        for (int i = 0; i < n; i++)
        {
            prefix = (prefix + nums[i]) % p;
            int want = (prefix - need + p) % p;
            if (map.containsKey(want))
                ret = Math.min(ret, i - map.get(want));

            map.put(prefix, i);
        }

        return ret < n ? ret : -1;
    }
}
```
---

求出`need = sum(nums) % p`為需要移除多少才能被`p`整除
透過prefix與HashMap找出`sub sum = need`的長度
並且一路更新HashMap中的`(prefix, index i)`


###### tags: `Leetcode` `Hash Map` `Hash+Prefix`