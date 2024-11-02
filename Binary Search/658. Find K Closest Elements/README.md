# Leetcode - 658. Find K Closest Elements (H)

[Leetcode](https://leetcode.com/problems/find-k-closest-elements/description/)

Given a **sorted** integer array `arr`, two integers `k` and `x`, return the `k` closest integers to `x` in the array. The result should also be sorted in ascending order.

An integer `a` is closer to `x` than an integer `b` if:

-   `|a - x| < |b - x|`, or
-   `|a - x| == |b - x|` and `a < b`

**Example 1:**
```
Input: arr = [1,2,3,4,5], k = 4, x = 3
Output: [1,2,3,4]
```
**Example 2:**
```
Input: arr = [1,2,3,4,5], k = 4, x = -1
Output: [1,2,3,4]
```
**Constraints:**

-   `1 <= k <= arr.length`
-   `1 <= arr.length <= 104`
-   `arr` is sorted in **ascending** order.
-   `-104 <= arr[i], x <= 104`

---
```java
// Java 4ms(Beats 97.93%), Time O(log(N-K)+K), Space O(N)
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        int low = 0, high = arr.length - k;
        List<Integer> ret = new LinkedList<>();
        while (low < high) {
            int mid = low + (high - low) / 2;

            if (x - arr[mid] > arr[mid + k] - x) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }

        for (int i = 0; i < k; i++)
            ret.add(arr[low + i]);
        
        return ret;
    }


    /*
     |---------------|
            x
    mid            mid+k 
    */
}
```
```java
// Java 40ms(Beats 15.65%), Time O(NlogK), Space O(K)
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        PriorityQueue<Integer> pq = new PriorityQueue<>(
            (a, b) -> {
                if (Math.abs(a - x) == Math.abs(b - x))
                    return b - a;
                return Math.abs(b - x) - Math.abs(a - x);
            }
        );

        for (int a : arr)
        {
            pq.offer(a);
            if (pq.size() > k)
                pq.poll();
        }

        List<Integer> ret = new ArrayList<>();
        for (int a : pq)
            ret.add(a);
        Collections.sort(ret);

        return ret;

    }
}
```
---

[wisdompeak/YouTube](https://www.youtube.com/watch?v=gfwLpRYbCx0)
```
圖例:
      |---------------|
            x
    mid            mid+k 
```
注: Binary Search的mid為區間的頭

因為是sorted array, 利用尋找`k+1`個元素的區間
因此範圍是`mid ~ mid+k`, 而非`mid ~ mid+k-1`

比較`Arr[mid]`與`Arr[mid+k]`跟`x`的距離
* `Arr[mid]`若比`Arr[mid+k]`距離x更遠, 
代表`Arr[mid]`與其更前面的元素都不可能是答案
(因為範圍內有`k+1`個元素, 而第一個比最後一個 距離`x`更遠)
因此`low = mid + 1`

* `Arr[mid+k]`若比`Arr[mid]`距離x更遠, 
代表`Arr[mid+k]`與其更後面的元素都不可能是答案
(因為範圍內有`k+1`個元素, 而最後一個比第一個 距離`x`更遠)
(題目規定: 若距離相同取小直)
因此`high = mid`


###### tags: `Leetcode` `Binary Search`