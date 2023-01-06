# Leetcode - 48. Rotate Image (M+)

[Leetcode](https://leetcode.com/problems/rotate-image/)

You are given an `n x n` 2D `matrix` representing an image, rotate the image by **90** degrees (clockwise).

You have to rotate the image [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

**Example 1:**

![](https://miro.medium.com/max/1284/0*ov9WHQZgN3B2uwjP.jpg)
```
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]  
Output: [[7,4,1],[8,5,2],[9,6,3]]
```
**Example 2:**

![](https://miro.medium.com/max/1400/0*mKNMbZN48HxLQxfT.jpg)
```
Input: matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]  
Output: [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
```
**Constraints:**

-   `n == matrix.length == matrix[i].length`
-   `1 <= n <= 20`
-   `-1000 <= matrix[i][j] <= 1000`

---

```java
// Java  
class Solution {  
    public void rotate(int[][] matrix) {  
        /*  
        1 2 3  
        4 5 6  
        7 8 9  
          
        //transpose  
        1 4 7  
        2 5 8  
        3 6 9  
  
        //swap column  
        7 4 1  
        8 5 2  
        9 6 3  
          
        */  
        int temp = 0;  
        for (int i = 0; i < matrix.length; i++) {  
            for (int j = i; j < matrix[0].length; j++) {  
                //swap(matrix[i][j], matrix[j][i]);  
                temp = matrix[i][j];  
                matrix[i][j] = matrix[j][i];  
                matrix[j][i] = temp;  
            }  
        }  
          
        for (int i = 0; i < matrix.length; i++) {  
            for (int j = 0; j < matrix[0].length/2; j++) {  
                //swap(matrix[i][j], matrix[i][matrix[0].length-1-j]);  
                temp = matrix[i][j];  
                matrix[i][j] = matrix[i][matrix[0].length-1-j];  
                matrix[i][matrix[0].length-1-j] = temp;  
            }  
        }  
    }  
}
```


> **clockwise (順時鐘):**
```
1 2 3  
4 5 6  
7 8 9
```
1.  transpose matrix
```
1 4 7  
2 5 8  
3 6 9
```
2. swap column
```
7 4 1  
8 5 2  
9 6 3
```
> **anticlockwise (逆時鐘):**
```
1 2 3  
4 5 6  
7 8 9
```
1.  transpose matrix
```
1 4 7  
2 5 8  
3 6 9
```
2. swap row
```
3 6 9  
2 5 8  
1 4 7
```

###### tags: `Leetcode` `Matrix`