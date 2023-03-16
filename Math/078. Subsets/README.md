# Leetcode - 78. Subsets (M+)

[Leetcode](https://leetcode.com/problems/subsets/description/)

Given an integer array `nums` of **unique** elements, return _all possible_  

_subsets_

 _(the power set)_.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

**Example 1:**
```
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```
**Example 2:**
```
Input: nums = [0]
Output: [[],[0]]
```
**Constraints:**

-   `1 <= nums.length <= 10`
-   `-10 <= nums[i] <= 10`
-   All the numbers of `nums` are **unique**.

---
```java
// Java Time O(N!)
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> ret = new ArrayList<>();
        backtracking(nums, ret, new ArrayList<Integer>(), 0);
        return ret;
    }

    public void backtracking(int[] nums, List<List<Integer>> ret, List<Integer> curr, int k) {
        ret.add(new ArrayList(curr));

        for (int i = k; i < nums.length; i++) {
            curr.add(nums[i]);
            backtracking(nums, ret, curr, i + 1);
            curr.remove(curr.size() - 1);
        }
    }
}
```


---

###### tags: `Leetcode` `Math` `Combinatorics`
