# Leetcode - 1792. Maximum Average Pass Ratio (M+)

[Leetcode](https://leetcode.com/problems/maximum-average-pass-ratio/)

There is a school that has classes of students and each class will be having a final exam. You are given a 2D integer array `classes`, where `classes[i] = [passi, totali]`. You know beforehand that in the `ith` class, there are `totali` total students, but only `passi` number of students will pass the exam.

You are also given an integer `extraStudents`. There are another `extraStudents` brilliant students that are **guaranteed** to pass the exam of any class they are assigned to. You want to assign each of the `extraStudents` students to a class in a way that **maximizes** the **average** pass ratio across **all** the classes.

The **pass ratio** of a class is equal to the number of students of the class that will pass the exam divided by the total number of students of the class. The **average pass ratio** is the sum of pass ratios of all the classes divided by the number of the classes.

Return _the **maximum** possible average pass ratio after assigning the _`extraStudents`_ students. _Answers within `10-5` of the actual answer will be accepted.

**Example 1:**

> **Input:** classes = [[1,2],[3,5],[2,2]], `extraStudents` = 2
> **Output:** 0.78333
> **Explanation:** You can assign the two extra students to the first class. The average pass ratio will be equal to (3/4 + 3/5 + 2/2) / 3 = 0.78333.

**Example 2:**

> **Input:** classes = [[2,4],[3,9],[4,5],[2,10]], `extraStudents` = 4
> **Output:** 0.53485

**Constraints:**

-   `1 <= classes.length <= 105`
-   `classes[i].length == 2`
-   `1 <= passi <= totali <= 105`
-   `1 <= extraStudents <= 105`

---
```java
// Java 356ms(Beats 70.89%), Time O(NlogN), Space O(N)
class Solution {
    public double maxAverageRatio(int[][] classes, int extraStudents) {
        double ret = 0;
        PriorityQueue<double[]> pq = new PriorityQueue<>((a, b) -> Double.compare(b[0], a[0]));
        for (int[] c : classes)
        {
            double pass = (double) c[0];
            double total = (double) c[1];
            pq.offer(new double[] { (pass+1) / (total+1) - pass/total , pass, total});
        }
        for (int i = 0; i < extraStudents; i++)
        {
            double[] curr = pq.poll();
            double pass = curr[1] + 1;
            double total = curr[2] + 1;
            pq.offer(new double[] { (pass+1) / (total+1) - pass/total , pass, total});
        }

        while (!pq.isEmpty())
        {
            double[] curr = pq.poll();
            ret += curr[1] / curr[2];
        }
        
        return ret / classes.length;
    }
}
```
---

透過貪心的想法, 把extraStudents一個一個加到對於班級`提升pass ratio最大`的班級
將PriorityQueue按照`(pass+1) / (total+1) - pass / total`排序
故可將學生一個一個加到提昇最大的班級內


###### tags: `Leetcode` `Priority Queue`