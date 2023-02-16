Leetcode - 1. Two Sum (Easy)
============================

Given an array of integers `nums` and an integer `target`, return _indices of the two numbers such that they add up to `target`.

You may assume that each input would have **_exactly_ one solution**, and you may not use the _same_ element twice.

You can return the answer in any order.

**Example 1:**
```
Input: nums = \[2,7,11,15\], target = 9  
Output: \[0,1\]  
Explanation: Because nums\[0\] \+ nums\[1\] == 9, we return \[0, 1\].
```
**Example 2:**
```
Input: nums = \[3,2,4\], target = 6  
Output: \[1,2\]
```
**Example 3:**
```
Input: nums = \[3,3\], target = 6  
Output: \[0,1\]
```
**Constraints:**

-   `2 <= nums.length <= 104`
-   `-109 <= nums[i] <= 109`
-   `-109 <= target <= 109`
-   **Only one valid answer exists.**

---

```java
// Java  
public class Solution {  
     public int[] twoSum(int[] nums, int target) {  
      HashMap<Integer,Integer> hash = new HashMap<Integer,Integer>();  
            for (int i = 0; i < nums.length; i++) {  
                Integer diff = (Integer)(target - nums[i]);  
                if (hash.containsKey(diff)) {  
                    int[] toReturn = {hash.get(diff), i};  
                    return toReturn;  
                }  
                hash.put(nums[i], i);  
            }  
            return null;  
     }  
}
```

```csharp
// C#  
public class Solution {  
    public int[] TwoSum(int[] nums, int target) {  
        if (nums == null || nums.Length < 2) {  
            return new int[2];  
        }  
        Dictionary<int, int> dt = new Dictionary<int, int>();  
        for (int i = 0; i < nums.Length; i++) {  
            if (dt.ContainsKey(nums[i])) {  
                return new int[] {i, dt[nums[i]]};  
            } else if (!dt.ContainsKey(target - nums[i])){  
                dt.Add(target - nums[i], i);  
            }  
        }  
        return new int[2];  
    }  
}
```


###### tags: `Leetcode` `HashMap`
