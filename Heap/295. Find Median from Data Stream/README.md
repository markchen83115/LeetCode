# Leetcode - 295. Find Median from Data Stream (M)

[Leetcode](https://leetcode.com/problems/find-median-from-data-stream/description/)

The **median** is the middle value in an ordered integer list. If the size of the list is even, there is no middle value, and the median is the mean of the two middle values.

-   For example, for `arr = [2,3,4]`, the median is `3`.
-   For example, for `arr = [2,3]`, the median is `(2 + 3) / 2 = 2.5`.

Implement the MedianFinder class:

-   `MedianFinder()` initializes the `MedianFinder` object.
-   `void addNum(int num)` adds the integer `num` from the data stream to the data structure.
-   `double findMedian()` returns the median of all elements so far. Answers within `10-5` of the actual answer will be accepted.

**Example 1:**
```
Input
["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
[[], [1], [2], [], [3], []]
Output
[null, null, null, 1.5, null, 2.0]

Explanation
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.addNum(2);    // arr = [1, 2]
medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
medianFinder.addNum(3);    // arr[1, 2, 3]
medianFinder.findMedian(); // return 2.0
```
**Constraints:**

-   `-105 <= num <= 105`
-   There will be at least one element in the data structure before calling `findMedian`.
-   At most `5 * 104` calls will be made to `addNum` and `findMedian`.

**Follow up:**

-   If all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution?
-   If `99%` of all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution?

---
```java
// Java 147ms (Beats 40.23%), Time O(logN) add, O(1) find, Space O(N)
class MedianFinder {
    PriorityQueue<Integer> large;
    PriorityQueue<Integer> small;
    boolean even = true;

    public MedianFinder() {
        large = new PriorityQueue<>((a, b) -> b - a);
        small = new PriorityQueue<>((a, b) -> a - b);
        even = true;
    }
    
    // large -> small
    public void addNum(int num) {
        if (even) {
            large.offer(num);
            small.offer(large.poll());
        } else {
            small.offer(num);
            large.offer(small.poll());
        }
        even = !even;
    }
    
    public double findMedian() {
        if (even) {
            return (large.peek() + small.peek()) / 2.0;
        } else {
            return small.peek();
        }
    }
}
```
---

參考 [Java/Python two heap solution, O(log n) add, O(1) find](https://leetcode.com/problems/find-median-from-data-stream/solutions/74047/java-python-two-heap-solution-o-log-n-add-o-1-find/?orderBy=most_votes)


參考[【每日一題】 295. Find Median from Data Stream, 06/25/2019 - YouTube](https://youtu.be/dy8AdRodlkQ)
#### 解法1:

[ ](https://github.com/wisdompeak/LeetCode/tree/master/Sorted_Container/295.Find-Median-from-Data-Stream#%E8%A7%A3%E6%B3%951)

設計兩個可以即時排序的multiset，其中一個Large存放較大的那一半數據，另一個Small存放較小的那一半數據。

每次讀入新數num，和Large的首元素比較大小，決定要加入Large還是Small。加入之後，需要調整Large和Small的個數，使得總是保持Large比Small多一個或兩者數目相等。

產出的Median是Large的首元素，或是Large首元素和Small尾元素的平均值。

#### 解法2:

[ ](https://github.com/wisdompeak/LeetCode/tree/master/Sorted_Container/295.Find-Median-from-Data-Stream#%E8%A7%A3%E6%B3%952)

使用multiset容器和相應的迭代器的操作,非常簡單明了.不斷更新處於正中間或者中間偏左的那個迭代器位置即可.

注意,multiset裡有相同的元素加入時,新元素的迭代器位置在舊元素迭代器位置的後面.


###### tags: `Leetcode` `Sorted Container` `Dual Multiset`