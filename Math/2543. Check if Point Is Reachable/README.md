# Leetcode - 2543. Check if Point Is Reachable (H)

Leetcode

There exists an infinitely large grid. You are currently at point `(1, 1)`, and you need to reach the point `(targetX, targetY)` using a finite number of steps.

In one **step**, you can move from point `(x, y)` to any one of the following points:

-   `(x, y - x)`
-   `(x - y, y)`
-   `(2 * x, y)`
-   `(x, 2 * y)`

Given two integers `targetX` and `targetY` representing the X-coordinate and Y-coordinate of your final position, return `true` _if you can reach the point from_ `(1, 1)` _using some number of steps, and _`false`_ otherwise_.

**Example 1:**

Input: targetX = 6, targetY = 9
Output: false
Explanation: It is impossible to reach (6,9) from (1,1) using any sequence of moves, so false is returned.

**Example 2:**

Input: targetX = 4, targetY = 7
Output: true
Explanation: You can follow the path (1,1) -> (1,2) -> (1,4) -> (1,8) -> (1,7) -> (2,7) -> (4,7).

**Constraints:**

-   `1 <= targetX, targetY <= 109`

---

```java
// Java
class Solution {
    public boolean isReachable(int targetX, int targetY) {
        while (targetX % 2 == 0) {
                targetX /= 2;
            }
        while (targetY % 2 == 0) {
            targetY /= 2;
        }
        return gcd(targetX, targetY) == 1;  
    }
    
    public int gcd(int a, int b) {
        while (b > 0) {
            int c = a;
            a = b;
            b = c % b;
        }
        return a;
    }
}
```

---

將題目逆向思考 -> 是否能從(X, Y)走到(1, 1)
將四個步驟逆向操作

1.  `(x, y+x)`
2.  `(x+y, y)`
3.  `(x/2, y)`
4.  `(x, y/2)`

只看1,2步驟, 代表求mx+ny=1的x,y解
因此只有x,y的最小公因數為1時才有解

而3,4步驟, 可藉由除2將原始X,Y縮減


###### tags: `Leetcode` `Math` `Numerical Theory`
