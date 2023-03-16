# Leetcode - 46. Permutations (M+)

[Leetcode](https://leetcode.com/problems/permutations/description/)

Given an array `nums` of distinct integers, return _all the possible permutations_. You can return the answer in **any order**.

**Example 1:**
```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```
**Example 2:**
```
Input: nums = [0,1]
Output: [[0,1],[1,0]]
```
**Example 3:**
```
Input: nums = [1]
Output: [[1]]
```
**Constraints:**

-   `1 <= nums.length <= 6`
-   `-10 <= nums[i] <= 10`
-   All the integers of `nums` are **unique**.

---
```java
// Java Time O(N*N!)
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> ret = new ArrayList<>();
        boolean[] isUsed = new boolean[nums.length];
        backtracking(nums, ret, new ArrayList<Integer>(), isUsed);
        return ret;
    }

    public void backtracking(int[] nums, List<List<Integer>> ret, List<Integer> curr, boolean[] isUsed) {
        if (curr.size() == nums.length){
            ret.add(new ArrayList(curr));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (isUsed[i]) 
                continue;
            curr.add(nums[i]);
            isUsed[i] = true;
            backtracking(nums, ret, curr, isUsed);
            curr.remove(curr.size() - 1);
            isUsed[i] = false;
        }
    }
}
```


---
使用backtracking遍歷所有答案, 並記錄已使用過的數值

###### tags: `Leetcode` `Math` `Combinatorics`
