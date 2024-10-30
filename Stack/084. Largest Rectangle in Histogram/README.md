# Leetcode - 84. Largest Rectangle in Histogram (H)

[Leetcode](https://leetcode.com/problems/largest-rectangle-in-histogram/description/)

Given an array of integers `heights` representing the histogram's bar height where the width of each bar is `1`, return _the area of the largest rectangle in the histogram_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/04/histogram.jpg)
```
Input: heights = [2,1,5,6,2,3]
Output: 10
Explanation: The above is a histogram where width of each bar is 1.
The largest rectangle is shown in the red area, which has an area = 10 units.
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/04/histogram-1.jpg)
```
Input: heights = [2,4]
Output: 4
```
**Constraints:**

-   `1 <= heights.length <= 105`
-   `0 <= heights[i] <= 104`

---
```java
// Java 37ms(78.79%), Time O(N), Space O(N)
class Solution {
    public int largestRectangleArea(int[] heights) {
        int n = heights.length;
        int[] prevSmall = new int [n];
        int[] nextSmall = new int [n];
        ArrayDeque<Integer> stack = new ArrayDeque<>(); // increasing stack
        int ret = 0;
        // find next smaller index
        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && heights[stack.peek()] > heights[i]) {
                int index = stack.pop();
                nextSmall[index] = i;
            }
            stack.push(i);
        }
        while (!stack.isEmpty())
            nextSmall[stack.pop()] = n;
        
        // find previous smaller index
        for (int i = n - 1; i >= 0; i--) {
            while (!stack.isEmpty() && heights[stack.peek()] > heights[i]) {
                int index = stack.pop();
                prevSmall[index] = i;
            }
            stack.push(i);
        }
        while (!stack.isEmpty())
            prevSmall[stack.pop()] = -1;
        
        // calculate
        for (int i = 0; i < n; i++)
            ret = Math.max(ret, heights[i] * (nextSmall[i] - prevSmall[i] - 1));

        return ret;
    }
}
```


```java
// Java 10ms(97.23%), Time O(N), Space O(N)
class Solution {
    public int largestRectangleArea(int[] heights) {
        int n = heights.length;
        int[] prevSmall = new int [n];
        int[] nextSmall = new int [n];
        int ret = 0;
        prevSmall[0] = -1;
        nextSmall[n - 1] = n;
        // find previous smaller index
        for (int i = 1; i < n; i++) {
            int p = i - 1;
            while (p >= 0 && heights[p] >= heights[i])
                p = prevSmall[p];

            prevSmall[i] = p;
        }
        
        // find next smaller index
        for (int i = n - 2; i >= 0; i--) {
            int p = i + 1;
            while (p < n && heights[i] <= heights[p])
                p = nextSmall[p];

            nextSmall[i] = p;
        }
        
        // calculate
        for (int i = 0; i < n; i++)
            ret = Math.max(ret, heights[i] * (nextSmall[i] - prevSmall[i] - 1));

        return ret;
    }
}
```
---

[wisdompeak/YouTube解說](https://www.youtube.com/watch?v=mesaogfSjD4)

對於每個`heights[i]`而言, 它能產生的最大長方形為`高度 * (右邊界 - 左邊界)`,
左右邊界代表為`< heights[i]`的位置

使用Three-pass來計算, 
1. 在`i的左邊`, 找出`最接近i且長度 < heights[i]`
2. 在`i的右邊`, 找出`最接近i且長度 < heights[i]`
3. 對於每個`i`計算, `面積 = heights[i] * (右邊界 - 左邊界 - 1)`

###### tags: `Leetcode` `Stack` `Monotonic stack: next greater / smaller`