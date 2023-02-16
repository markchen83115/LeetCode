# Leetcode - 217. Contains Duplicate (E)

[Leetcode](https://leetcode.com/problems/contains-duplicate/)

Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct.

**Example 1:**
```
Input: nums = [1,2,3,1]  
Output: true
```
**Example 2:**
```
Input: nums = [1,2,3,4]  
Output: false
```
**Example 3:**
```
Input: nums = [1,1,1,3,3,4,3,2,4,2]  
Output: true
```
**Constraints:**

-   `1 <= nums.length <= 105`
-   `-109 <= nums[i] <= 109`

---
```java
//Java  
class Solution {  
    public boolean containsDuplicate(int[] nums) {  
        HashSet hs = new HashSet();  
        for (int i = 0; i < nums.length; i++) {  
            if(hs.add(nums[i]) == false) {  
                return true;  
            }  
        }  
        return false;  
    }  
}
```

```csharp
//C#  
public class Solution {  
    public bool ContainsDuplicate(int[] nums) {  
        if (nums.Distinct().Count() == nums.Length)  
            return false;  
        return true;  
    }  
}
```

---

Java:  
HashSet.add(), 當HashSet內沒有相同value會return true  
反之, HashSet內有相同value會return false

C#:  
使用Linq, Distinct()取得未重複數列, 再Count()計算個數



###### tags: `Leetcode` `HashMap`
