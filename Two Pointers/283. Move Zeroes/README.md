# Leetcode - 283. Move Zeroes (M)

[Leetcode](https://leetcode.com/problems/move-zeroes/)

Given an integer array `nums`, move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

**Note** that you must do this in-place without making a copy of the array.

**Example 1:**
```
Input: nums = [0,1,0,3,12]  
Output: [1,3,12,0,0]
```
**Example 2:**
```
Input: nums = [0]  
Output: [0]
```
**Constraints:**

-   `1 <= nums.length <= 104`
-   `-231 <= nums[i] <= 231 - 1`

**Follow up:** Could you minimize the total number of operations done?

---

```java
// Java  
class Solution {  
    public void moveZeroes(int[] nums) {  
        int snowBallSize = 0;  
        for (int i = 0; i < nums.length; i++) {  
            if (nums[i] == 0) {  
                snowBallSize++;  
            } else if (snowBallSize > 0) {  
                nums[i - snowBallSize] = nums[i];  
                nums[i] = 0;  
            }  
        }  
    }  
}
```

[ref link](https://leetcode.com/problems/move-zeroes/solutions/172432/the-easiest-but-unusual-snowball-java-solution-beats-100-o-n-clear-explanation/)

The idea is that we go through the array and gather all zeros on our road.

![](https://miro.medium.com/max/1280/0*-xLkUMnlmTcocxpo.png)

Let’s take our example:  
the first step — we meet 0.  
Okay, just remember that now the size of our snowball is 1. Go further.

![](https://miro.medium.com/max/994/0*jpDCL5fBcI7mYevc.png)

Next step — we encounter 1. Swap the most left 0 of our snowball with element 1.

![](https://miro.medium.com/max/1120/0*uALOjU8sUAeqQGCV.png)

Next step — we encounter 0 again!

![](https://miro.medium.com/max/1120/0*KoKRsAdEFA461Q53.png)

Our ball gets bigger, now its size = 2.

![](https://miro.medium.com/max/1120/0*LSUDQEwZmKKnmoJ3.png)

Next step — 3. Swap again with the most left zero.

![](https://miro.medium.com/max/1120/0*YEPh8gBLPH37bgaA.png)

Looks like our zeros roll all the time

![](https://miro.medium.com/max/1120/0*ZjkIiU4cGE6dXnj0.png)

Next step — 12. Swap again:

![](https://miro.medium.com/max/1120/0*V_K1-JD22ZMDxKN-.png)

We reached finish! Congratulations!

![](https://miro.medium.com/max/1120/0*-A3TLYgUo4HMa8WG.png)

[  
](https://medium.com/tag/leetcode?source=post_page-----b8d4c8e1208c---------------leetcode-----------------)



###### tags: `Leetcode` `Two Pointers`