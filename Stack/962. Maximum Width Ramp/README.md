# Leetcode - 962. Maximum Width Ramp (H)

[Leetcode](https://leetcode.com/problems/maximum-width-ramp/)

A **ramp** in an integer array `nums` is a pair `(i, j)` for which `i < j` and `nums[i] <= nums[j]`. The **width** of such a ramp is `j - i`.

Given an integer array `nums`, return _the maximum width of a **ramp** in _`nums`. If there is no **ramp** in `nums`, return `0`.

**Example 1:**
```
Input: nums = [6,0,8,2,1,5]
Output: 4
Explanation: The maximum width ramp is achieved at (i, j) = (1, 5): nums[1] = 0 and nums[5] = 5.
```
**Example 2:**
```
Input: nums = [9,8,1,0,1,9,4,0,4,1]
Output: 7
Explanation: The maximum width ramp is achieved at (i, j) = (2, 9): nums[2] = 1 and nums[9] = 1.
```
**Constraints:**

-   `2 <= nums.length <= 5 * 104`
-   `0 <= nums[i] <= 5 * 104`

---
```java
// Java 11ms(Beats 76.12%), Time O(N), Space O(N)
class Solution {
    public int maxWidthRamp(int[] nums) {
        Deque<Integer> stack = new ArrayDeque<>();
        int n = nums.length;
        for (int i = 0; i < n; i++)
            if (stack.isEmpty() || nums[stack.peek()] > nums[i])
                stack.push(i);
        
        int ret = 0;
        for (int i = n - 1; i >= 0; i--)
            while (!stack.isEmpty() && nums[stack.peek()] <= nums[i])
                ret = Math.max(ret, i - stack.pop());
        
        return ret;
    }
}
```
---

拿取越靠兩端的點, 則寬度越長
因此維護一個`單調遞減隊列` -> 數字越小且越靠前 可作為`左端點`
ex: `9,10,7,...,10`
`9`為左端點**必定**比`10`為左端點的寬度更大
ex: `9,7,...,10,8`
`7`為左端點則**有機會**比`9`為左端點的寬度更大


再從右到左作為`右端點` -> 與`單調遞減隊列`中的數字來求得最大寬度


###### tags: `Leetcode` `Stack` `monotonic stack: other usages`