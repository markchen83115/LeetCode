# Leetcode - 853. Car Fleet (M)

[Leetcode](https://leetcode.com/problems/car-fleet/)

There are `n` cars at given miles away from the starting mile 0, traveling to reach the mile `target`.

You are given two integer array `position` and `speed`, both of length `n`, where `position[i]` is the starting mile of the `ith` car and `speed[i]` is the speed of the `ith` car in miles per hour.

A car cannot pass another car, but it can catch up and then travel next to it at the speed of the slower car.

A **car fleet** is a car or cars driving next to each other. The speed of the car fleet is the **minimum** speed of any car in the fleet.

If a car catches up to a car fleet at the mile `target`, it will still be considered as part of the car fleet.

Return the number of car fleets that will arrive at the destination.

**Example 1:**

> **Input:** target = 12, position = mod[10,8,0,5,3mod], speed = mod[2,4,1,1,3mod]
> 
> **Output:** 3
> 
> **Explanation:**
> 
> -   The cars starting at 10 (speed 2) and 8 (speed 4) become a fleet, meeting each other at 12. The fleet forms at `target`.
> -   The car starting at 0 (speed 1) does not catch up to any other car, so it is a fleet by itself.
> -   The cars starting at 5 (speed 1) and 3 (speed 3) become a fleet, meeting each other at 6. The fleet moves at speed 1 until it reaches `target`.

**Example 2:**

> **Input:** target = 10, position = mod[3mod], speed = mod[3mod]
> 
> **Output:** 1
> 
> **Explanation:**
> 
> There is only one car, hence there is only one fleet.

**Example 3:**

> **Input:** target = 100, position = mod[0,2,4mod], speed = mod[4,2,1mod]
> 
> **Output:** 1
> 
> **Explanation:**
> 
> -   The cars starting at 0 (speed 4) and 2 (speed 2) become a fleet, meeting each other at 4. The car starting at 4 (speed 1) travels to 5.
> -   Then, the fleet at 4 (speed 2) and the car at position 5 (speed 1) become one fleet, meeting each other at 6. The fleet moves at speed 1 until it reaches `target`.

**Constraints:**

-   `n == position.length == speed.length`
-   `1 <= n <= 105`
-   `0 < target <= 106`
-   `0 <= position[i] < target`
-   All the values of `position` are **unique**.
-   `0 < speed[i] <= 106`

---
```java
// Java 77ms(Beats 65.77%), Time O(NlogN), Space O(N)
class Solution {
    public int carFleet(int target, int[] position, int[] speed) {
        int n = position.length;
        int ret = 0;
        double[][] cars = new double[n][];
        for (int i = 0; i < n; i++)
            cars[i] = new double[] {position[i], (target - position[i]) * 1.0 / speed[i]};
        Arrays.sort(cars, (a, b) -> Double.compare(b[0], a[0]));

        for (int i = 1; i < n; i++)
        {
            if (cars[i-1][1] < cars[i][1])
                ret++;
            else
                cars[i][1] = cars[i-1][1];
        }

        return ret + 1;
    }
}
```
---

將距離/速度`target - position[i]) / speed[i]`計算出每台車到達終點所需時間, 並以位置做排序

從最靠近終點的車子開始
若`前一台車到達終點時間 < 這一台車到達終點時間`代表前一台車自己成為一個fleet
若`前一台車到達終點時間 >= 這一台車到達終點時間`代表會成為同個fleet


###### tags: `Leetcode` `Others` `Collision`