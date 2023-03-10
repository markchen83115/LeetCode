# Leetcode - 239. Sliding Window Maximum (H-)

[Leetcode](https://leetcode.com/problems/sliding-window-maximum/)

You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return _the max sliding window_.

**Example 1:**
```
Input: nums = [1,3,-1,-3,5,3,6,7], k = 3  
Output: [3,3,5,5,6,7]  
Explanation:   
Window position                Max  
---------------               -----  
[1  3  -1] -3  5  3  6  7       3  
 1 [3  -1  -3] 5  3  6  7       3  
 1  3 [-1  -3  5] 3  6  7       5  
 1  3  -1 [-3  5  3] 6  7       5  
 1  3  -1  -3 [5  3  6] 7       6  
 1  3  -1  -3  5 [3  6  7]      7
```
**Example 2:**
```
Input: nums = [1], k = 1  
Output: [1]
```
**Constraints:**

-   `1 <= nums.length <= 105`
-   `-104 <= nums[i] <= 104`
-   `1 <= k <= nums.length`

---

```java
// Java  
class Solution {  
    public int[] maxSlidingWindow(int[] nums, int k) {  
        Deque<Integer> deque = new ArrayDeque<>();  
        int[] ret = new int[nums.length - k + 1];  
        int p = 0;  
        for (int i = 0; i < nums.length; i++) {  
            //maintain a mono-decreasing queue  
            while (!deque.isEmpty() && nums[deque.peekLast()] <= nums[i]) {  
                deque.pollLast();  
            }  
            deque.offerLast(i);  
  
            //check if queue head is out dated  
            while (!deque.isEmpty() && i - deque.peekFirst()>= k) {  
                deque.pollFirst();  
            }  
  
            //tha maximum of sliding window is head  
            if (i >= k - 1) {  
                ret[p++] = (nums[deque.peekFirst()]);  
            }  
        }  
        return ret;  
    }  
  
  
    //1. maintain a mono-decreasing queue  
    //2. check if queue head is out dated  
    //3. tha maximum of sliding window is head  
    //4. dequeue store index, not value  
}
```

```csharp
// C#  
public class Solution {  
    public int[] MaxSlidingWindow(int[] nums, int k) {  
        LinkedList<int> queue = new LinkedList<int>();  
        List<int> ret = new List<int>();  
        for (int i = 0; i < nums.Length; i++) {  
            while (queue.Count() > 0 && nums[queue.Last.Value] <= nums[i]) {  
                queue.RemoveLast();  
            }  
            queue.AddLast(i);  
  
            while (queue.Count() > 0 && i - queue.First.Value >= k) {  
                queue.RemoveFirst();  
            }  
  
            if (i >= k - 1) {  
                ret.Add(nums[queue.First.Value]);  
            }  
        }  
        return ret.ToArray();  
    }  
    //deque store index of nums  
    //deque is mono-decreasing  
    //check if deque head is out of window  
}
```
	
---
	
[wisedompeek/youtube??????](https://www.youtube.com/watch?v=b1rqOQ5p6EA)
```
Window position                Max  
---------------               -----  
 1  3 [-1  -3  *5] 3  6  7       5  
 1  3  -1 [-3  *5  3] 6  7       5  
 1  3  -1  -3 [*5  3  6] 7       6
```
???`*5`??????,  
???`*5`??????sliding windows???,  
?????????`-1`???`-3`???????????????????????????, ?????????????????????

??????????????????queue??????????????????????????????,  
????????????queue???`[*7, 5]`, ??????`5`?????????????????????`*7`???????????????sliding window???,  
??????`*7`???index??????`??? i-k`?????????(`5`???index = i)

1.  ???deque???????????????+?????????queue
2.  queue?????????index, ?????????????????????sliding window???
3.  ???queue head?????????sliding window, ?????????
4.  ????????????queue head





###### tags: `Leetcode` `Deque`
