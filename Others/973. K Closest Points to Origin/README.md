# Leetcode - 973. K Closest Points to Origin (M)

[Leetcode](https://leetcode.com/problems/k-closest-points-to-origin/description/)

Given an array of `points` where `points[i] = [xi, yi]` represents a point on the **X-Y** plane and an integer `k`, return the `k` closest points to the origin `(0, 0)`.

The distance between two points on the **X-Y** plane is the Euclidean distance (i.e., `√(x1 - x2)2 + (y1 - y2)2`).

You may return the answer in **any order**. The answer is **guaranteed** to be **unique** (except for the order that it is in).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/03/closestplane1.jpg)
```
Input: points = [[1,3],[-2,2]], k = 1
Output: [[-2,2]]
Explanation:
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
We only want the closest k = 1 points from the origin, so the answer is just [[-2,2]].
```
**Example 2:**
```
Input: points = [[3,3],[5,-1],[-2,4]], k = 2
Output: [[3,3],[-2,4]]
Explanation: The answer [[-2,4],[3,3]] would also be accepted.
```
**Constraints:**

-   `1 <= k <= points.length <= 104`
-   `-104 < xi, yi < 104`

---
```java
// Java PQ 27ms(Beats 84.28%), Time O(NlogN), Space O(N)
class Solution {
    public int[][] kClosest(int[][] points, int k) {
        int[][] ret = new int[k][]; 
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        for (int i = 0; i < points.length; i++) {
            int sum = points[i][0] * points[i][0] + points[i][1] * points[i][1];
            pq.add(new int[] {sum, i});
        }

        for (int i = 0; i < k; i++) {
            int[] p = pq.poll();
            ret[i] = points[p[1]];
        }

        return ret;
    }
}
```

```java
// Java Sort 28ms(Beats 79.31%)
class Solution {
    public int[][] kClosest(int[][] points, int k) {
        int[][] ret = new int[k][]; 
        Arrays.sort(points, (a, b) -> a[0] * a[0] + a[1] * a[1] - b[0] * b[0] - b[1] * b[1]);
        return Arrays.copyOfRange(points, 0, k);
    }
}
```

```java
// Java quickSelect 8ms(Beats 97.92%), Time O(N), Space O(N)
class Solution {
    public int[][] kClosest(int[][] points, int k) {
        int[][] arr = new int[points.length][];
        int[][] ret = new int[k][];
        for (int i = 0; i < points.length; i++)
            arr[i] = new int[] {points[i][0] * points[i][0] + points[i][1] * points[i][1], i};

        int kth = quickSelect(0, points.length - 1, k, arr);

        for (int i = 0; i < points.length; i++) {
            if (arr[i][0] <= kth)
                ret[--k] = points[arr[i][1]];
        }

        return ret;
    }

    public int quickSelect(int a, int b, int k, int[][] arr) {
        int i = a, t = a, j = b;
        int pivot = arr[(a + b) / 2][0];
        while (t <= j) {
            if (arr[t][0] < pivot) {
                swap(i, t, arr);
                i++;
                t++;
            } else if (arr[t][0] == pivot) {
                t++;
            } else {
                swap(t, j, arr);
                j--;
            }
        }

        if (i - a >= k) 
            return quickSelect(a, i - 1, k, arr);
        else if (j - a + 1 >= k)
            return pivot;
        else
            return quickSelect(j + 1, b, k - (j - a + 1), arr);
    }

    public void swap(int i, int j, int[][] arr) {
        int[] tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
    }
}
```

---

quick select可做到O(N)
請見[wisdompeak YouTube](https://www.youtube.com/watch?v=xi4QVECpmxQ)

###### tags: `Leetcode` `Others` `Quick Select`