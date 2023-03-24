# Leetcode - 739. Daily Temperatures (H-)

[Leetcode](https://leetcode.com/problems/daily-temperatures/description/)

Given an array of integers `temperatures` represents the daily temperatures, return _an array_ `answer` _such that_ `answer[i]` _is the number of days you have to wait after the_ `ith` _day to get a warmer temperature_. If there is no future day for which this is possible, keep `answer[i] == 0` instead.

**Example 1:**

**Input:** temperatures = \[73,74,75,71,69,72,76,73\]
**Output:** \[1,1,4,2,1,1,0,0\]

**Example 2:**

**Input:** temperatures = \[30,40,50,60\]
**Output:** \[1,1,1,0\]

**Example 3:**

**Input:** temperatures = \[30,60,90\]
**Output:** \[1,1,0\]

**Constraints:**

-   `1 <= temperatures.length <= 105`
-   `30 <= temperatures[i] <= 100`

---
```java
// Java 27ms(86.44%), Time O(N), Space O(N)
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        int n = temperatures.length;
        int[] ret = new int[n];
        ArrayDeque<Integer> stack = new ArrayDeque<>();   // index
        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && temperatures[stack.peek()] < temperatures[i]) {
                int index = stack.pop();
                ret[index] = i - index;
            }
            stack.push(i);
        }
        return ret;
    }
}
```

---


###### tags: `Leetcode` `Stack` `Monotonic stack: next greater / smaller`