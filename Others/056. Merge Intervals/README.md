# Leetcode — 56. Merge Intervals (M)
[Leetcode](https://leetcode.com/problems/merge-intervals/)  


Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return _an array of the non-overlapping intervals that cover all the intervals in the input_.

**Example 1:**

**Input:** intervals = \[\[1,3\],\[2,6\],\[8,10\],\[15,18\]\]  
**Output:** \[\[1,6\],\[8,10\],\[15,18\]\]  
**Explanation:** Since intervals \[1,3\] and \[2,6\] overlaps, merge them into \[1,6\].

**Example 2:**

**Input:** intervals = \[\[1,4\],\[4,5\]\]  
**Output:** \[\[1,5\]\]  
**Explanation:** Intervals \[1,4\] and \[4,5\] are considered overlapping.

**Constraints:**

-   `1 <= intervals.length <= 104`
-   `intervals[i].length == 2`
-   `0 <= starti <= endi <= 104`

```java
// Java  
class Solution {  
    public int[][] merge(int[][] intervals) {  
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));  
        ArrayList<int[]> ret = new ArrayList<>();  
        int[] newInterval = intervals[0];  
          
        for (int[] interval : intervals) {  
            //           [1,4]    [4,5]  
            if (newInterval[1] >= interval[0]) {  
                newInterval[1] = Math.max(newInterval[1], interval[1]);  
            } else {  
                ret.add(newInterval);  
                newInterval = interval;  
            }  
        }  
        ret.add(newInterval);  
        return ret.toArray(new int[ret.size()][]);  
    }  
}
```

技巧
--

1.  `Arrays.sort() `預設為小排序到大  
    利用java8的Lambda 可自行定義sort方式且code簡單  
    如果return>0則交換位置  
    `Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));`
2.  `Integer.signum(int x)` 也可用於sort  
    如果指定的值是負數，返回-1  
    如果指定的值是零，返回0  
    如果指定的值是正的，返回1
3.  建立Array  
    `int[] x = new int[n];`  
    `int[][] x = new int[n][n];`
4.  List.toArray 將List轉換為Aarry`  
    return ret.toArray(new int[ret.size()][]);  
    `①不帶引數的toArray方法，是構造的一個Object陣列，然後進行資料拷貝，此時進行轉型就會產生ClassCastException  
    ②帶引數的toArray方法，則是根據引數陣列的型別，構造了一個對應型別的，長度跟ArrayList的size一致的空陣列，雖然方法本身還是以 Object陣列的形式返回結果，不過由於構造陣列使用的ComponentType跟需要轉型的ComponentType一致，就不會產生轉型異常

```java
public Object\[\] toArray() {   
    Object\[\] result = new Object\[size\];     
    System.arraycopy(elementData, 0, result, 0, size);     
    return result;   
}public Object\[\] toArray(Object a\[\]) {     
    if (a.length < size)         
        a = (Object\[\])java.lang.reflect.Array.newInstance(a.getClass().getComponentType(), size);  
    System.arraycopy(elementData, 0, a, 0, size);     
    if (a.length > size)         
        a\[size\] = null;     
    return a;   
}
```

###### tags: `Leetcode` `掃描線/差分陣列`