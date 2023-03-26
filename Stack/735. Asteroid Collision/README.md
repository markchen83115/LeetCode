# Leetcode - 735. Asteroid Collision (M)

[Leetcode](https://leetcode.com/problems/asteroid-collision/description/)

We are given an array `asteroids` of integers representing asteroids in a row.

For each asteroid, the absolute value represents its size, and the sign represents its direction (positive meaning right, negative meaning left). Each asteroid moves at the same speed.

Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.

**Example 1:**
```
Input: asteroids = [5,10,-5]
Output: [5,10]
Explanation: The 10 and -5 collide resulting in 10. The 5 and 10 never collide.
```
**Example 2:**
```
Input: asteroids = [8,-8]
Output: []
Explanation: The 8 and -8 collide exploding each other.
```
**Example 3:**
```
Input: asteroids = [10,2,-5]
Output: [10]
Explanation: The 2 and -5 collide resulting in -5. The 10 and -5 collide resulting in 10.
```
**Constraints:**

-   `2 <= asteroids.length <= 104`
-   `-1000 <= asteroids[i] <= 1000`
-   `asteroids[i] != 0`

---
```java
// Java 4ms(90.60%), Time O(N), Space O(N)
class Solution {
    public int[] asteroidCollision(int[] asteroids) {
        ArrayDeque<Integer> stack = new ArrayDeque<>();
        for (int asteroid : asteroids) {
            while (!stack.isEmpty() && asteroid < 0 && stack.peek() > 0) {
                int prev = stack.pop();
                int diff = prev - Math.abs(asteroid);
                if (diff == 0) {
                    asteroid = 0;
                    break;
                } else if (diff > 0) {
                    asteroid = prev;
                }
            }
            if (asteroid != 0)
                stack.push(asteroid);
        }

        int[] ret = new int[stack.size()];
        for (int i = stack.size() - 1; i >= 0; i--)
            ret[i] = stack.pop();

        return ret;
    }
}
```
---

注意`direction: positive meaning right, negative meaning left`
如果第一個為`-2`, 代表向左, 因此永遠不會跟其他行星相撞
只有當前一個是`>0`, 後一個是`<0`時才會相撞


###### tags: `Leetcode` `Stack`