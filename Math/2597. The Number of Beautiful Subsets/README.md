# Leetcode - 2597. The Number of Beautiful Subsets (M)

[Leetcode](https://leetcode.com/problems/the-number-of-beautiful-subsets/description/)

You are given an array `nums` of positive integers and a **positive** integer `k`.

A subset of `nums` is **beautiful** if it does not contain two integers with an absolute difference equal to `k`.

Return _the number of **non-empty beautiful **subsets of the array_ `nums`.

A **subset** of `nums` is an array that can be obtained by deleting some (possibly none) elements from `nums`. Two subsets are different if and only if the chosen indices to delete are different.

**Example 1:**
```
Input: nums = [2,4,6], k = 2
Output: 4
Explanation: The beautiful subsets of the array nums are: [2], [4], [6], [2, 6].
It can be proved that there are only 4 beautiful subsets in the array [2,4,6].
```
**Example 2:**
```
Input: nums = [1], k = 1
Output: 1
Explanation: The beautiful subset of the array nums is [1].
It can be proved that there is only 1 beautiful subset in the array [1].
```
**Constraints:**

-   `1 <= nums.length <= 20`
-   `1 <= nums[i], k <= 1000`

---
```java
// Java Backtracking, Time O(2^N), Space O(N)
class Solution {
    int ret = 0;
    public int beautifulSubsets(int[] nums, int k) {
        HashMap<Integer, Integer> map = new HashMap<>();
        backtracking(nums, k, 0, map);
        return ret;
    }
    
    public void backtracking(int[] nums, int k, int p, HashMap<Integer, Integer> map) {
        if (p == nums.length) {
            if (map.size() > 0) ret++;
            return;
        }
        int num = nums[p];
        backtracking(nums, k, p + 1, map);   // not add
        
        if (map.getOrDefault(num + k, 0) <= 0 && map.getOrDefault(num - k, 0) <= 0) {
            map.merge(num, 1, Integer::sum);
            backtracking(nums, k, p + 1, map);
            map.merge(num, -1, Integer::sum);
        }   
    }
}
```

---

利用backtracking窮舉所有答案, 對於每個nums可選擇放或不放

###### tags: `Leetcode` `Math` `Combinatorics`