# Leetcode - 2463. Minimum Total Distance Traveled (M+)

[Leetcode](https://leetcode.com/problems/minimum-total-distance-traveled/)

There are some robots and factories on the X-axis. You are given an integer array `robot` where `robot[i]` is the position of the `ith` robot. You are also given a 2D integer array `factory` where `factory[j] = [positionj, limitj]` indicates that `positionj` is the position of the `jth` factory and that the `jth` factory can repair at most `limitj` robots.

The positions of each robot are **unique**. The positions of each factory are also **unique**. Note that a robot can be **in the same position** as a factory initially.

All the robots are initially broken; they keep moving in one direction. The direction could be the negative or the positive direction of the X-axis. When a robot reaches a factory that did not reach its limit, the factory repairs the robot, and it stops moving.

**At any moment**, you can set the initial direction of moving for **some** robot. Your target is to minimize the total distance traveled by all the robots.

Return _the minimum total distance traveled by all the robots_. The test cases are generated such that all the robots can be repaired.

**Note that**

-   All robots move at the same speed.
-   If two robots move in the same direction, they will never collide.
-   If two robots move in opposite directions and they meet at some point, they do not collide. They cross each other.
-   If a robot passes by a factory that reached its limits, it crosses it as if it does not exist.
-   If the robot moved from a position `x` to a position `y`, the distance it moved is `|y - x|`.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2022/09/15/example1.jpg)
> 
> **Input:** robot = [0,4,6], factory = [[2,2],[6,2]]
> **Output:** 4
> **Explanation:** As shown in the figure:
> - The first robot at position 0 moves in the positive direction. It will be repaired at the first factory.
> - The second robot at position 4 moves in the negative direction. It will be repaired at the first factory.
> - The third robot at position 6 will be repaired at the second factory. It does not need to move.
> The limit of the first factory is 2, and it fixed 2 robots.
> The limit of the second factory is 2, and it fixed 1 robot.
> The total distance is |2 - 0| + |2 - 4| + |6 - 6| = 4. It can be shown that we cannot achieve a better total distance than 4.

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2022/09/15/example-2.jpg)
> 
> **Input:** robot = [1,-1], factory = [[-2,1],[2,1]]
> **Output:** 2
> **Explanation:** As shown in the figure:
> - The first robot at position 1 moves in the positive direction. It will be repaired at the second factory.
> - The second robot at position -1 moves in the negative direction. It will be repaired at the first factory.
> The limit of the first factory is 1, and it fixed 1 robot.
> The limit of the second factory is 1, and it fixed 1 robot.
> The total distance is |2 - 1| + |(-2) - (-1)| = 2. It can be shown that we cannot achieve a better total distance than 2.

**Constraints:**

-   `1 <= robot.length, factory.length <= 100`
-   `factory[j].length == 2`
-   `-109 <= robot[i], positionj <= 109`
-   `0 <= limitj <= robot.length`
-   The input will be generated such that it is always possible to repair every robot.

---
```java
// Java 61ms(Beats 20.00%), Time O(N^3), Space O(N^3)
class Solution {
    public long minimumTotalDistance(List<Integer> robot, int[][] factory) {
        Collections.sort(robot);
        Arrays.sort(factory, (a, b) -> a[0] - b[0]);
        int n = factory.length;
        int m = robot.size();

        long[][] dp = new long[101][101]; // dp[i][j] = minimum cost for first factory [i] serve [j] robots
        long[][][] dest = new long[101][101][101]; // dest[i][j][k] = robot [j]~[k] go to factory[i] 
    
        for (int i = 0; i < n; i++)
            for (int j = 0; j < m; j++)
            {
                long sum = 0;
                for (int k = j; k < m; k++)
                {
                    sum += Math.abs(robot.get(k) - factory[i][0]);
                    dest[i][j][k] = sum;
                }
            }

        // initial dp[0][j]
        for (int j = 1; j <= m; j++)
        {
            if (j <= factory[0][1])
                dp[0][j] = dest[0][0][j-1];
            else
                dp[0][j] = Long.MAX_VALUE / 2; // exceed factory 0 capacity
        }
            
        
        for (int i = 1; i < n; i++)
            for (int j = 1; j <= m; j++)
            {
                dp[i][j] = dp[i-1][j]; // k = 0 
                for (int k = 1; k <= Math.min(factory[i][1], j); k++)
                    dp[i][j] = Math.min(dp[i][j], dp[i-1][j-k] + dest[i][j-k][j-1]);
            }
        
        return dp[n-1][m];
    }
}
```
---

參考: [每日一题】LeetCode 2463. Minimum Total Distance Traveled - YouTube](https://youtu.be/bI2A2QClL0I)

```               --k--
x | x x x | x x | x x x
              j'.     j
```
`dp[i][j] = 前i個工廠覆蓋j個機器人`
動態轉移方程: `dp[i][j] = Min( dp[i][j], dp[i-1][j-k] )`
k代表第i個工廠覆蓋k個機器人

`dest[i][j][k] = 從index j~k的機器人都去工廠i`

###### tags: `Leetcode` `Dynamic Programming` `區間型 I`