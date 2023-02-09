# Leetcode - 15. 3Sum (M)

Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

**Example 1:**

**Input:** nums = \[-1,0,1,2,-1,-4\]
**Output:** \[\[-1,-1,2\],\[-1,0,1\]\]

**Example 2:**

**Input:** nums = \[\]
**Output:** \[\]

**Example 3:**

**Input:** nums = \[0\]
**Output:** \[\]

**Constraints:**

-   `0 <= nums.length <= 3000`
-   `-105 <= nums[i] <= 105`

---

```java
class Solution {  
    public List<List<Integer>> threeSum(int[] nums) {  
        Arrays.sort(nums);List<List<Integer>> outputArr = new LinkedList();  
        int lowBond = 0, highBond = 0, sum = 0;  
        //拆成固定一個 + 2 SUM  
        for (int i=0; i<nums.length-2; i++) {  
            if (i == 0 || (i > 0 && nums[i] != nums[i-1])) {  
                lowBond = i + 1;  
                highBond = nums.length - 1;  
                sum = 0 - nums[i];  
                while (lowBond < highBond) {  
                    if (nums[lowBond] + nums[highBond] == sum) {  
                        outputArr.add(Arrays.asList(nums[i], nums[lowBond], nums[highBond]));  
                        while (lowBond < highBond && nums[lowBond] == nums[lowBond+1]) lowBond++;  
                        while (lowBond < highBond && nums[highBond] == nums[highBond-1]) highBond--;  
                        lowBond++;  
                        highBond--;  
                    } else if (nums[lowBond] + nums[highBond] > sum) {  
                        highBond--;  
                    } else {  
                        lowBond++;  
                    }  
                }  
            }  
        }  
        return outputArr;  
    }  
}
```

---

Arrary先sort(),
將3 SUM拆解成固定一個 + 2 SUM
2 SUM 用lower bond跟higher bond去加總
要unique answer, `[-4,-1,-1,0,1,2]` 避開重複使用-1去找2 SUM

###### tags: `Leetcode` `Two Pointers`