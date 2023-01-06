# Leetcode - 435. Non-overlapping Intervals (M+)

[Leetcode](https://leetcode.com/problems/non-overlapping-intervals/)

Given an array of intervals `intervals` where `intervals[i] = [starti, endi]`, return _the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping_.

**Example 1:**
```
Input: intervals = [[1,2],[2,3],[3,4],[1,3]]  
Output: 1  
Explanation: [1,3] can be removed and the rest of the intervals are non-overlapping.
```
**Example 2:**
```
Input: intervals = [[1,2],[1,2],[1,2]]  
Output: 2  
Explanation: You need to remove two [1,2] to make the rest of the intervals non-overlapping.
```
**Example 3:**
```
Input: intervals = [[1,2],[2,3]]  
Output: 0  
Explanation: You don't need to remove any of the intervals since they're already non-overlapping.
```
**Constraints:**

-   `1 <= intervals.length <= 105`
-   `intervals[i].length == 2`
-   `-5 * 104 <= starti < endi <= 5 * 104`

---

```java
// Java  
class Solution {  
    public int eraseOverlapIntervals(int[][] intervals) {  
        Arrays.sort(intervals, (a, b) -> a[1] - b[1]);  
        int end = intervals[0][1], counts = 1; //at least 1  
        for (int i = 1; i < intervals.length; i++) {  
            //find non-overlap  
            if (end <= intervals[i][0]) {  
                counts++;  
                end = intervals[i][1];  
            }  
        }  
        return intervals.length - counts;  
    }  
}
```

```csharp
// C#  
public class Solution {  
    public int EraseOverlapIntervals(int[][] intervals) {  
        Array.Sort(intervals, (a, b) => a[1] - b[1]);  
        int counts = 1, end = intervals[0][1];  
        for (int i = 1; i < intervals.Length; i++) {  
            if (end <= intervals[i][0]) {  
                counts++;  
                end = intervals[i][1];  
            }  
        }  
        return intervals.Length - counts;  
    }  
}
```

---

題目要移除最少的重複intervals  
等於找不重複的intervals個數  
**重複個數 = 總數 — 不重複個數**


###### tags: `Leetcode` `Greedy` `Intervals`