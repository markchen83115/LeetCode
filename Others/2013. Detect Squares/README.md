# Leetcode - 2013. Detect Squares (M+)

[Leetcode](https://leetcode.com/problems/detect-squares/)

You are given a stream of points on the X-Y plane. Design an algorithm that:

-   **Adds** new points from the stream into a data structure. **Duplicate** points are allowed and should be treated as different points.
-   Given a query point, **counts** the number of ways to choose three points from the data structure such that the three points and the query point form an **axis-aligned square** with **positive area**.

An **axis-aligned square** is a square whose edges are all the same length and are either parallel or perpendicular to the x-axis and y-axis.

Implement the `DetectSquares` class:

-   `DetectSquares()` Initializes the object with an empty data structure.
-   `void add(int[] point)` Adds a new point `point = [x, y]` to the data structure.
-   `int count(int[] point)` Counts the number of ways to form **axis-aligned squares** with point `point = [x, y]` as described above.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2021/09/01/image.png)
> 
> **Input**
> ["DetectSquares", "add", "add", "add", "count", "count", "add", "count"]
> [[], [[3, 10]], [[11, 2]], [[3, 2]], [[11, 10]], [[14, 8]], [[11, 2]], [[11, 10]]]
> **Output**
> [null, null, null, null, 1, 0, null, 2]
> 
> **Explanation**
> DetectSquares detectSquares = new DetectSquares();
> detectSquares.add([3, 10]);
> detectSquares.add([11, 2]);
> detectSquares.add([3, 2]);
> detectSquares.count([11, 10]); // return 1. You can choose:
>                                //   - The first, second, and third points
> detectSquares.count([14, 8]);  // return 0. The query point cannot form a square with any points in the data structure.
> detectSquares.add([11, 2]);    // Adding duplicate points is allowed.
> detectSquares.count([11, 10]); // return 2. You can choose:
>                                //   - The first, second, and third points
>                                //   - The first, third, and fourth points

**Constraints:**

-   `point.length == 2`
-   `0 <= x, y <= 1000`
-   At most `3000` calls **in total** will be made to `add` and `count`.

---
```java
// Java 90ms(Beats 49.43%), Time O(N), Space O(N^2)
class DetectSquares {
    int[][] xy;
    int[] dir = new int[] {1, 1, -1, -1, 1};
    public DetectSquares() {
        xy = new int[1005][1005];
    }
    
    public void add(int[] point) {
        int x = point[0], y = point[1];
        xy[x][y] += 1;
    }
    
    public int count(int[] point) {
        int ret = 0;
        int x = point[0], y = point[1];
        for (int i = 0; i <= 1000; i++)
        {
            int diff = Math.abs(x - i);
            if (diff == 0)
                continue;
            int j;
            j = y - diff;
            if (j >= 0 && j <= 1000)
                ret += xy[i][y] * xy[x][j] * xy[i][j];

            j = y + diff;
            if (j >= 0 && j <= 1000)
                ret += xy[i][y] * xy[x][j] * xy[i][j];
        }

        return ret;
    }
}
```
---

給定`(x, y)`, 只要透過`對角點(i, j)`則可確認唯一的正方形
以`橫坐標i`計算正方形邊長 `d = Math.abs(x - i)`
因此`縱座標j`只有兩種可能`y - d`或`y + d`
考慮重合的點, 則可能的正方形數量為`count[x][j] * count[i][y] * count[i][j]`


###### tags: `Leetcode` `Others` `Enumeration`