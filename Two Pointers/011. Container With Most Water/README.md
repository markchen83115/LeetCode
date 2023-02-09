Leetcode - 11. Container With Most Water (M+)
=================================================

You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `ith` line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return _the maximum amount of water a container can store_.

**Notice** that you may not slant the container.

**Example 1:**

![](https://miro.medium.com/max/700/0*IkLVAaoW_UXbNlmK.jpg)

**Input:** height = \[1,8,6,2,5,4,8,3,7\]  
**Output:** 49  
**Explanation:** The above vertical lines are represented by array \[1,8,6,2,5,4,8,3,7\]. In this case, the max area of water (blue section) the container can contain is 49.

**Example 2:**

**Input:** height = \[1,1\]  
**Output:** 1

**Constraints:**

-   `n == height.length`
-   `2 <= n <= 105`
-   `0 <= height[i] <= 104`

---

```java
// Java  
class Solution {  
    public int maxArea(int[] height) {  
        int maxWater = 0;  
        int left = 0, right = height.length-1;  
        while (left < right) {  
            maxWater = Math.max(maxWater, (right - left) * Math.min(height[right], height[left]));  
            if (height[left] < height[right]) {  
                left++;  
            } else {  
                right--;  
            }  
        }  
        return maxWater;  
    }  
}
```

```csharp
// C#  
public class Solution {  
    public int MaxArea(int[] height) {  
        int l = 0, r = height.Length - 1;  
        int water = 0;  
        while (l < r) {  
            water = Math.Max(water, (r - l) * Math.Min(height[r], height[l]));  
            if (height[l] < height[r]) {  
                l++;  
            } else {  
                r--;  
            }  
        }  
        return water;  
    }  
}
```

---

max container = 底* 高  
從範圍最大的底開始,  
為了找max值, 每次縮減底之後, 一定要找到更高,  
故數字大的高不動, 移動數字小的高


###### tags: `Leetcode` `Two Pointers`