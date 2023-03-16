# Leetcode - 189. Rotate Array (M)
Given an array, rotate the array to the right by `k` steps, where `k` is non-negative.

**Example 1:**
```
Input: nums = [1,2,3,4,5,6,7], k = 3  
Output: [5,6,7,1,2,3,4]  
Explanation:  
rotate 1 steps to the right: [7,1,2,3,4,5,6]  
rotate 2 steps to the right: [6,7,1,2,3,4,5]  
rotate 3 steps to the right: [5,6,7,1,2,3,4]
```
**Example 2:**
```
Input: nums = [-1,-100,3,99], k = 2  
Output: [3,99,-1,-100]  
Explanation:   
rotate 1 steps to the right: [99,-1,-100,3]  
rotate 2 steps to the right: [3,99,-1,-100]
```
**Constraints:**

-   `1 <= nums.length <= 105`
-   `-231 <= nums[i] <= 231 - 1`
-   `0 <= k <= 105`

**Follow up:**

-   Try to come up with as many solutions as you can. There are at least **three** different ways to solve this problem.
-   Could you do it in-place with `O(1)` extra space?

---

```java
// Java  
// 3 reverse thinking, O(n) time cost, O(1) space cost  
class Solution {  
    public void rotate(int[] nums, int k) {  
        int n = nums.length;  
        k = k % n;  
        swap(nums, 0, n - 1 - k);  
        swap(nums, n - k, n - 1);  
        swap(nums, 0, n - 1);  
    }  
    public void swap(int[] array, int x, int y) {  
        while (x < y) {  
            int tmp = array[x];  
            array[x] = array[y];  
            array[y] = tmp;  
            x++;  
            y--;  
        }  
    }  
    //利用位元運算符swap值  
    /*public void reverse(int[] nums, int n, int m){  
        while(n < m){  
            nums[n] ^= nums[m];  
            nums[m] ^= nums[n];  
            nums[n] ^= nums[m];  
            n++;  
            m--;  
        }  
    }  
    */  
}
```

```java
// Java  
// Interesting way, O(n) time cost, O(1) space cost  
public class Solution {  
    public void rotate(int[] nums, int k) {  
        if(nums.length <= 1){  
            return;  
        }  
        //step each time to move  
        int step = k % nums.length;  
        //find GCD between nums length and step  
        int gcd = findGcd(nums.length, step);  
        int position, count;  
          
        //gcd path to finish movie  
        for(int i = 0; i < gcd; i++){  
            //beginning position of each path  
            position = i;  
            //count is the number we need swap each path  
            count = nums.length / gcd - 1;  
            for(int j = 0; j < count; j++){  
                position = (position + step) % nums.length;  
                //swap index value in index i and position  
                nums[i] ^= nums[position];  
                nums[position] ^= nums[i];  
                nums[i] ^= nums[position];  
            }  
        }  
    }  
      
    public int findGcd(int a, int b){  
        return (a == 0 || b == 0) ? a + b : findGcd(b, a % b);  
    }  
      
}
```


```java
// Java  
// Normal way, O(n) time cost, O(k % nums.length) space cost  
public class Solution {  
    public void rotate(int[] nums, int k) {  
        if(nums.length <= 1){  
            return;  
        }  
        //step each time to move  
        int step = k % nums.length;  
        int[] tmp = new int[step];  
        for(int i = 0; i < step; i++){  
            tmp[i] = nums[nums.length - step + i];  
        }  
        for(int i = nums.length - step - 1; i >= 0; i--){  
            nums[i + step] = nums[i];  
        }  
        for(int i = 0; i < step; i++){  
            nums[i] = tmp[i];  
        }  
    }  
}
```

---

Ref: [Link1](https://leetcode.com/problems/rotate-array/solutions/1730142/java-c-python-a-very-very-well-detailed-explanation/), [Link2](https://leetcode.com/problems/rotate-array/solutions/54289/my-three-way-to-solve-this-problem-the-first-way-is-interesting-java/)

![](https://miro.medium.com/max/1400/0*XDiRF_43TCfavLqv.png)

![](https://miro.medium.com/max/1400/0*a4mvnURbPJQ0Y8e2.png)

[  
](https://medium.com/tag/leetcode?source=post_page-----e2d1d3cfec8d---------------leetcode-----------------)




###### tags: `Leetcode` `Others`
