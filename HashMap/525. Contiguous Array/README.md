# Leetcode - 525. Contiguous Array (M)

[Leetcode](https://leetcode.com/problems/contiguous-array/)

Given a binary array `nums`, return _the maximum length of a contiguous subarray with an equal number of _`0`_ and _`1`.

**Example 1:**
```
Input: nums = [0,1]  
Output: 2  
Explanation: [0, 1] is the longest contiguous subarray with an equal number of 0 and 1.
```
**Example 2:**
```
Input: nums = [0,1,0]  
Output: 2  
Explanation: [0, 1] (or [1, 0]) is a longest contiguous subarray with equal number of 0 and 1.
```
**Constraints:**

-   `1 <= nums.length <= 105`
-   `nums[i]` is either `0` or `1`.

---

```java
// Java  
class Solution {  
    public int findMaxLength(int[] nums) {  
        /*  
        prefix sum  
        x x x x [x x x x x x]  
                 j         i  = presum[i] - presum[j -1]   
        */  
        HashMap<Integer, Integer> hm = new HashMap();  
        int prefix = 0, max = 0;  
        hm.put(0, -1);  
        for (int i = 0; i < nums.length; i++) {  
            if (nums[i] == 1)  
                prefix ++;  
            else  
                prefix--;  
              
            if (hm.containsKey(prefix)) {  
                max = Math.max(max, i - hm.get(prefix));  
            } else {  
                hm.put(prefix, i);  
            }  
        }  
        return max;  
  
    }  
}
```

```csharp
// C#  
public class Solution {  
    public int FindMaxLength(int[] nums) {  
        Dictionary<int, int> dict = new Dictionary<int, int>();  
        int presum = 0, max = 0;  
        dict.Add(0, -1);  
  
        for (int i = 0; i < nums.Length; i++) {  
            if (nums[i] == 1)   
                presum++;  
            else  
                presum--;  
  
            if (dict.ContainsKey(presum))  
                max = Math.Max(max, i - dict[presum]);  
            else  
                dict.Add(presum, i);  
        }  
  
        return max;  
    }  
}
```

---

**題目要尋找有相同個數的0, 1連續數列**  
**  
將**`[0, 1, 0, 0, 1]` 視為`[-1, 1, -1, -1, 1]`  
**等於題目希望找出全部相加為0的連續數列  
**因此可使用Prefix Sum

Prefix Sum:  
nums:   x x x x x [x x x x x x]  
index:  0          j         i  
[x x x x x x]的總和為prefix[i] - prefix[j-1]  
  
＊使用Prefix Sum要注意起始值  
此題起始值為Prefix[0] = -1  
否則會漏掉[0, 1]這種答案


###### tags: `Leetcode` `HashMap` `Hash+Prefix`
