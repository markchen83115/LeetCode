# Leetcode - 1861. Rotating the Box (M)

[Leetcode](https://leetcode.com/problems/rotating-the-box/)

You are given an `m x n` matrix of characters `box` representing a side-view of a box. Each cell of the box is one of the following:

-   A stone `'#'`
-   A stationary obstacle `'*'`
-   Empty `'.'`

The box is rotated **90 degrees clockwise**, causing some of the stones to fall due to gravity. Each stone falls down until it lands on an obstacle, another stone, or the bottom of the box. Gravity **does not** affect the obstacles' positions, and the inertia from the box's rotation **does not **affect the stones' horizontal positions.

It is **guaranteed** that each stone in `box` rests on an obstacle, another stone, or the bottom of the box.

Return _an _`n x m`_ matrix representing the box after the rotation described above_.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2021/04/08/rotatingtheboxleetcodewithstones.png)
> 
> **Input:** box = [["#",".","#"]]
> **Output:** 
> [["."],
>  ["#"],
>  ["#"]]

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2021/04/08/rotatingtheboxleetcode2withstones.png)
> 
> **Input:** box = 
> [["#",".","*","."],
>  ["#","#","*","."]]
> **Output:** 
> [["#","."],
>  ["#","#"],
>  ["*","*"],
>  [".","."]]

**Example 3:**

> ![](https://assets.leetcode.com/uploads/2021/04/08/rotatingtheboxleetcode3withstone.png)
> 
> **Input:** box = 
> [["#","#","*",".","*","."],
>  ["#","#","#","*",".","."],
>  ["#","#","#",".","#","."]]
> **Output:** 
> [[".","#","#"],
>  [".","#","#"],
>  ["#","#","*"],
>  ["#","*","."],
>  ["#",".","*"],
>  ["#",".","."]]

**Constraints:**

-   `m == box.length`
-   `n == box[i].length`
-   `1 <= m, n <= 500`
-   `box[i][j]` is either `'#'`, `'*'`, or `'.'`.

---
```java
// Java 7ms(Beats 79.67%), Time O(M*N), Space O(1)
class Solution {
    public char[][] rotateTheBox(char[][] box) {
        int m = box.length, n = box[0].length;
        char[][] ret = new char[n][m];
        // move to right
        for (int i = 0; i < m; i++)
        {
            int empty = n - 1;
            for (int j = n - 1; j >= 0; j--)
            {
                if (box[i][j] == '*')
                    empty = j - 1;
                if (box[i][j] == '#')
                {
                    box[i][j] = '.';
                    box[i][empty] = '#';
                    empty--;
                }
            }
        }

        // 90
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
                ret[j][m - 1 - i] = box[i][j];

        return ret;
    }
}
```
---

對於原本的box都向右移動, 而移動時若在[i][j]遇到石頭代表空位必須從[i][j-1]開始
最後再90度旋轉

###### tags: `Leetcode` `Others`