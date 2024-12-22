# Leetcode - 3394. Check if Grid can be Cut into Sections (M)

[Leetcode](https://leetcode.com/problems/check-if-grid-can-be-cut-into-sections/)

You are given an integer `n` representing the dimensions of an `n x n` grid, with the origin at the bottom-left corner of the grid. You are also given a 2D array of coordinates `rectangles`, where `rectangles[i]` is in the form `[startx, starty, endx, endy]`, representing a rectangle on the grid. Each rectangle is defined as follows:

-   `(startx, starty)`: The bottom-left corner of the rectangle.
-   `(endx, endy)`: The top-right corner of the rectangle.

**Note **that the rectangles do not overlap. Your task is to determine if it is possible to make **either two horizontal or two vertical cuts** on the grid such that:

-   Each of the three resulting sections formed by the cuts contains **at least** one rectangle.
-   Every rectangle belongs to **exactly** one section.

Return `true` if such cuts can be made; otherwise, return `false`.

**Example 1:**

> **Input:** n = 5, rectangles = [[1,0,5,2],[0,2,2,4],[3,2,5,3],[0,4,4,5]]
> 
> **Output:** true
> 
> **Explanation:**
> 
> ![](https://assets.leetcode.com/uploads/2024/10/23/tt1drawio.png)
> 
> The grid is shown in the diagram. We can make horizontal cuts at `y = 2` and `y = 4`. Hence, output is true.

**Example 2:**

> **Input:** n = 4, rectangles = [[0,0,1,1],[2,0,3,4],[0,2,2,3],[3,0,4,3]]
> 
> **Output:** true
> 
> **Explanation:**
> 
> ![](https://assets.leetcode.com/uploads/2024/10/23/tc2drawio.png)
> 
> We can make vertical cuts at `x = 2` and `x = 3`. Hence, output is true.

**Example 3:**

> **Input:** n = 4, rectangles = [[0,2,2,4],[1,0,3,2],[2,2,3,4],[3,0,4,2],[3,2,4,4]]
> 
> **Output:** false
> 
> **Explanation:**
> 
> We cannot make two horizontal or two vertical cuts that satisfy the conditions. Hence, output is false.

**Constraints:**

-   `3 <= n <= 109`
-   `3 <= rectangles.length <= 105`
-   `0 <= rectangles[i][0] < rectangles[i][2] <= n`
-   `0 <= rectangles[i][1] < rectangles[i][3] <= n`
-   No two rectangles overlap.

---
```java
// Java 297ms(Beats 100.00%), Time O(NlogN), Space O(N)
class Solution {
    public boolean checkValidCuts(int n, int[][] rectangles) {
        HashMap<Integer, Integer> addX = new HashMap<>();
        HashMap<Integer, Integer> minuseX = new HashMap<>();
        TreeSet<Integer> setX = new TreeSet<>();
        HashMap<Integer, Integer> addY = new HashMap<>();
        HashMap<Integer, Integer> minuseY = new HashMap<>();
        TreeSet<Integer> setY = new TreeSet<>();
        for (int[] r : rectangles) {
            setX.add(r[0]);
            setX.add(r[2]);
            setY.add(r[1]);
            setY.add(r[3]);
            addX.merge(r[0], 1, Integer::sum);
            minuseX.merge(r[2], 1, Integer::sum);
            addY.merge(r[1], 1, Integer::sum);
            minuseY.merge(r[3], 1, Integer::sum);
        }

        return isOK(addX, minuseX, setX) || isOK(addY, minuseY, setY);
    }

    boolean isOK(HashMap<Integer, Integer> add, HashMap<Integer, Integer> minuse, TreeSet<Integer> set)
    {
        int sum = 0;
        int count = 3;
        for (int k : set) {
            sum -= minuse.getOrDefault(k, 0);
            if (sum == 0 && minuse.getOrDefault(k, 0) > 0)
                count--;
            if (count == 0)
                return true;
            sum += add.getOrDefault(k, 0);
        }
        return false;
    }
}
```
---

將X與Y分別計算, 
以X為例, 在長方形開始時+1, 長方形離開時-1,
因此透過差分數組找出當`sum == 0`的時候, 
並且必須為`長方型離開後變為0`, 因為題目要求每個分割區塊至少有一塊長方形

注意, 在計算差分數組時, 
先計算長方形離開, 再檢查是否當前已沒有長方形, 再加入新長方形


###### tags: `Leetcode` `Others` `掃描線/差分陣列`