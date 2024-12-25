# Leetcode - 090. Subsets II (M+)

[Leetcode](https://leetcode.com/problems/subsets-ii/)

Given an integer array `nums` that may contain duplicates, return _all possible_  

_subsets_

_ (the power set)_.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

**Example 1:**

> **Input:** nums = [1,2,2]
> **Output:** [[],[1],[1,2],[1,2,2],[2],[2,2]]

**Example 2:**

> **Input:** nums = [0]
> **Output:** [[],[0]]

**Constraints:**

-   `1 <= nums.length <= 10`
-   `-10 <= nums[i] <= 10`

---
```java
// Java 1ms(Beats 99.79%), Time O(2^N), Space O(N)
class Solution {
    List<List<Integer>> ret = new ArrayList<>();
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        boolean[] visited = new boolean[nums.length];
        helper(new ArrayList<>(), nums, 0, visited);
        return ret;
    }
    
    public void helper(List<Integer> list, int[] nums, int idx, boolean[] visited) {
        ret.add(new ArrayList<>(list));
        
        for (int i = idx; i < nums.length; i++) {
            if (i >= 1 && nums[i] == nums[i-1] && !visited[i-1])
                continue;
            list.add(nums[i]);
            visited[i] = true;
            helper(list, nums, i + 1, visited);     
            list.remove(list.size() - 1);
            visited[i] = false;
        }
    }
}
```
---

這題的重點是去重, `1 2 2 2 3 3` 取兩個2的話有三種方法, 但對於答案來說都一樣

去重的技巧：對於n個連續的相同元素，如果我們打算取k個，那永遠只取前k個
```java
for (int i = cur; i < nums.size(); i++)
{
    if (i >= 1 && nums[i] == nums[i-1] && visited[i-1] == false)
        continue;
    ...
}
```
意思是，如果nums[i]和它前面一個元素相同，但是前面一個元素沒有被選中，那nums[i]本身也不應該被選中。否則的話，就違背了我們之前的指導方針：對於相同的元素我们不能跳躍著選取


###### tags: `Leetcode` `DFS` `search in an array`