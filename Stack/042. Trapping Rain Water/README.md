# Leetcode - 42. Trapping Rain Water (H/H-)

[Leetcode](https://leetcode.com/problems/trapping-rain-water/description/)

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)
```
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```
**Example 2:**
```
Input: height = [4,2,0,3,2,5]
Output: 9
```
**Constraints:**

-   `n == height.length`
-   `1 <= n <= 2 * 104`
-   `0 <= height[i] <= 105`

---

```java
// Three Pass 1ms, Time O(N), Space O(N)
class Solution {
    public int trap(int[] height) {
        int n = height.length;
        int[] leftMax = new int[n]; // leftMax[i] = the max height from 0 to i - 1
        int[] rightMax = new int[n];
        int ret = 0;
        for (int i = 1; i < n; i++)
            leftMax[i] = Math.max(leftMax[i - 1], height[i - 1]);

        for (int i = n - 2; i >= 0; i--)
            rightMax[i] = Math.max(rightMax[i + 1], height[i + 1]);

        for (int i = 0; i < n; i++) {
            int h = Math.min(leftMax[i], rightMax[i]) - height[i];
            if (h > 0)
                ret += h;
        }
        return ret;
    }
}
```

```java
// Monotonic stack 3ms, Time O(N), Space O(N)
class Solution {
    public int trap(int[] height) {
        ArrayDeque<Integer> stack = new ArrayDeque();
        int ret = 0;
        for (int i = 0; i < height.length; i++) {
            while (!stack.isEmpty() && height[stack.peek()] < height[i]) {
                int base = height[stack.pop()];
                if (stack.isEmpty()) continue;
                int h = Math.min(height[stack.peek()], height[i]) - base;
                int w = i - stack.peek() - 1;
                ret += h * w;
            }
            stack.push(i);
        }

        return ret;
    }
}
```

---

[wisdompeak/YouTube解說](https://www.youtube.com/watch?v=LArMcFpCK-M)

> 方法1:

格子i的儲水高度為`min(i左邊最高的高度, i右邊最高的高度) - i的高度`
ex: `[2, 1, 3]`, 格子1的儲水高度為`min(2, 3) - 1 = 1`
因此先算出`leftMax[i]: i左邊最高的高度`與`rightMax[i]: i右邊最高的高度`, 


###### tags: `Leetcode` `Stack` `Monotonic stack: next greater / smaller` `Greedy` `Three Pass`