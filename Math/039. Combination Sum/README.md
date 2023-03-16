# Leetcode - 39. Combination Sum (M)
[Leetcode](https://leetcode.com/problems/combination-sum/)
Given an array of **distinct** integers `candidates` and a target integer `target`, return _a list of all **unique combinations** of _`candidates`_ where the chosen numbers sum to _`target`_._ You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the `frequency` of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

**Example 1:**
```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
```
**Example 2:**
```
Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]
```
**Example 3:**
```
Input: candidates = [2], target = 1
Output: []
```
**Constraints:**

-   `1 <= candidates.length <= 30`
-   `2 <= candidates[i] <= 40`
-   All elements of `candidates` are **distinct**.
-   `1 <= target <= 40`

---

```java
//Java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> ret = new ArrayList<>();
        List<Integer> arr = new ArrayList<>();
        backtracking(ret, arr, candidates, target, 0);
        return ret;
    }
    
    public void backtracking(List<List<Integer>> ret, List<Integer> arr, int[] candidates, int remain, int start) {
        if (remain == 0) {
            //use new array, because arr will be modify
            ret.add(new ArrayList<>(arr));
            return;
        } else {
            for (int i = start; i < candidates.length; i++) {
                if (candidates[i] <= remain) {
                    arr.add(candidates[i]);
                    //not i + 1, because element can be duplicate
                    backtracking(ret, arr, candidates, remain - candidates[i], i); 
                    arr.remove(arr.size() - 1);
                }
            }
        }
    }
}
```

```csharp
//C#
public class Solution {
    public IList<IList<int>> CombinationSum(int[] candidates, int target) {
        List<IList<int>> ret = new List<IList<int>>();
        List<int> arr = new List<int>();
        backtracking(ret, arr, candidates, target, 0);
        return ret;
    }
    
    public void backtracking(IList<IList<int>> ret, 
                             IList<int> arr, 
                             int[] candidates, 
                             int remain, 
                             int start) {
        if (remain == 0) {
            ret.Add(arr.ToList());
            return;
        } else {
            for (int i = start; i < candidates.Length; i++) {
                if (candidates[i] <= remain) {
                  arr.Add(candidates[i]);
                  //not i + 1, because element can be duplicate 
                  backtracking(ret, arr, candidates, remain - candidates[i], i);
                  arr.RemoveAt(arr.Count() - 1); //use RemoveAt, rather than Remove
                }
            }
        }
    }
}
```


###### tags: `Leetcode` `DFS`
