# Leetcode - 128. Longest Consecutive Sequence (H-)

[Leetcode](https://leetcode.com/problems/longest-consecutive-sequence/)
Given an unsorted array of integers `nums`, return _the length of the longest consecutive elements sequence._

You must write an algorithm that runs in `O(n)` time.

**Example 1:**
```
Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```
**Example 2:**
```
Input: nums = [0,3,7,2,5,8,4,6,0,1]
Output: 9
```

**Constraints:**

-   `0 <= nums.length <= 105`
-   `-109 <= nums[i] <= 109`

---
```java
// Java  
class Solution {  
    public int longestConsecutive(int\[\] nums) {  
        HashSet<Integer> set = new HashSet();  
        int ret = 0;  
        for (int i = 0;  i < nums.length; i++) {  
            set.add(nums\[i\]);  
        }  
  
        for (int ele : set) {  
            if (!set.contains(ele - 1)) {   //左邊有值的 跳過  
                int count = 1;  
                int currentNum = ele;  
                while (set.contains(++currentNum)) {    //往右找連續  
                    count++;  
                }  
                ret = Math.max(ret, count);  
            }  
        }  
        return ret;  
    }  
}
```

```csharp
// C#  
public class Solution {  
    public int LongestConsecutive(int\[\] nums) {  
        HashSet<int\> hset = new HashSet<int>();  
        int maxLen = 0;  
        for (int i = 0; i < nums.Length; i++) {  
            hset.Add(nums\[i\]);  
        }  
  
        foreach (int ele in hset) {  
            if (!hset.Contains(ele - 1)) {  //左邊有值 跳過  
                int count = 1;  
                int currentNum = ele;  
                while (hset.Contains(++currentNum)) {   //往右計算連續  
                    count++;  
                }  
                maxLen = Math.Max(maxLen, count);  
            }  
        }  
        return maxLen;  
    }  
}
```
---
										
題目要求必須為O(n), 因此無法使用`Arrays.sort()` O(nlogn)

透過HashSet將相同元素過濾  
尋找連續開頭的第一個元素, 故左邊有值的跳過(代表非第一個),  
只找左邊沒值的, 計算最大連續長度


###### tags: `Leetcode` `Union Find`