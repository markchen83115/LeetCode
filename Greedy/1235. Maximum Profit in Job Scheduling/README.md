# Leetcode - 1235. Maximum Profit in Job Scheduling (H-)

[Leetcode](https://leetcode.com/problems/maximum-profit-in-job-scheduling/)

We have `n` jobs, where every job is scheduled to be done from `startTime[i]` to `endTime[i]`, obtaining a profit of `profit[i]`.

You’re given the `startTime`, `endTime` and `profit` arrays, return the maximum profit you can take such that there are no two jobs in the subset with overlapping time range.

If you choose a job that ends at time `X` you will be able to start another job that starts at time `X`.

**Example 1:**

![](https://miro.medium.com/max/760/0*Vyd0HuO732bX4Z6I.png)
```
Input: startTime = [1,2,3,3], endTime = [3,4,5,6], profit = [50,10,40,70]  
Output: 120  
Explanation: The subset chosen is the first and fourth job.   
Time range [1-3]+[3-6] , we get profit of 120 = 50 + 70.
```
**Example 2:**

![](https://miro.medium.com/max/1400/0*L9N4t_ngeUKXT3Mn.png)
```
Input: startTime = [1,2,3,4,6], endTime = [3,5,10,6,9], profit = [20,20,100,70,60]  
Output: 150  
Explanation: The subset chosen is the first, fourth and fifth job.   
Profit obtained 150 = 20 + 70 + 60.
```
**Example 3:**

![](https://miro.medium.com/max/1052/0*aIDrZHs-Ol1mZk1v.png)
```
Input: startTime = [1,1,1], endTime = [2,3,4], profit = [5,6,4]  
Output: 6
```
**Constraints:**

-   `1 <= startTime.length == endTime.length == profit.length <= 5 * 104`
-   `1 <= startTime[i] < endTime[i] <= 109`
-   `1 <= profit[i] <= 104`

---

```java
// Java (TreeMap)  
class Solution {  
    public int jobScheduling(int[] startTime, int[] endTime, int[] profit) {  
        int n = startTime.length;  
        int[][] jobs = new int[n][3];  
        for (int i = 0; i < n; i++) {  
            jobs[i] = new int[] {startTime[i], endTime[i], profit[i]};  
        }  
  
        Arrays.sort(jobs, (a, b) -> a[1] - b[1]);    // sort by endTime asc  
  
        // Greedy Algo.  
        // dp[i] = j => At time i, the biggest profit is j  
        TreeMap<Integer, Integer> dp = new TreeMap<Integer, Integer>();   
        dp.put(-1, 0);  
        for (int[] job : jobs) {  
            // floor: <= k, the biggest number  
            int curr = dp.floorEntry(job[0]).getValue() + job[2];  
            dp.put(job[1], Math.max(dp.lastEntry().getValue(), curr));  
        }  
  
        return dp.lastEntry().getValue();  
    }  
}
```


```java
// Java (Binary Search)  
class Solution {
    public int jobScheduling(int[] startTime, int[] endTime, int[] profit) {
        int n = startTime.length;
        int[][] jobs = new int[n][3];
        for (int i = 0; i < n; i++) {
            jobs[i] = new int[] {startTime[i], endTime[i], profit[i]};
        }

        Arrays.sort(jobs, (a, b) -> a[1] - b[1]);    // sort by endTime asc

        // Greedy Algo.
        // dp[i] = j => At time i, the biggest profit is j
        List<List<Integer>> dp = new ArrayList<>();
        dp.add(Arrays.asList(-1, 0));

        for (int[] job : jobs) {
            // find largestSmaller
            int left = 0, right = dp.size();
            // 0 1 3 5
            while (left < right) {
                int mid = left + (right - left) / 2;
                if (job[0] < dp.get(mid).get(0)) {     // use startTime
                    right = mid;
                } else {
                    left = mid + 1;
                    
                }
            }
            // left = smallestLarger, so need to -1
            if (left > 0)
                left--;
            
            int curr = dp.get(left).get(1) + job[2];
            if (curr > dp.get(dp.size() - 1).get(1)) {
                dp.add(Arrays.asList(job[1], curr));
            }
        }

        return dp.get(dp.size() - 1).get(1);
    }
}
```

---

> **Greedy Algo.**

[wisedompeak/YT解說](https://www.youtube.com/watch?v=0C7re8lam7M)

看到區間類型的問題, 可以先對endTime來排序


###### tags: `Leetcode` `Greedy` `Intervals`