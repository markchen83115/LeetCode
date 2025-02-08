# Leetcode - 2349. Design a Number Container System (M-)

[Leetcode](https://leetcode.com/problems/design-a-number-container-system/)

Design a number container system that can do the following:

-   **Insert **or **Replace** a number at the given index in the system.
-   **Return **the smallest index for the given number in the system.

Implement the `NumberContainers` class:

-   `NumberContainers()` Initializes the number container system.
-   `void change(int index, int number)` Fills the container at `index` with the `number`. If there is already a number at that `index`, replace it.
-   `int find(int number)` Returns the smallest index for the given `number`, or `-1` if there is no index that is filled by `number` in the system.

**Example 1:**

> **Input**
> ["NumberContainers", "find", "change", "change", "change", "change", "find", "change", "find"]
> [[], [10], [2, 10], [1, 10], [3, 10], [5, 10], [10], [1, 20], [10]]
> **Output**
> [null, -1, null, null, null, null, 1, null, 2]
> 
> **Explanation**
> NumberContainers nc = new NumberContainers();
> nc.find(10); // There is no index that is filled with number 10. Therefore, we return -1.
> nc.change(2, 10); // Your container at index 2 will be filled with number 10.
> nc.change(1, 10); // Your container at index 1 will be filled with number 10.
> nc.change(3, 10); // Your container at index 3 will be filled with number 10.
> nc.change(5, 10); // Your container at index 5 will be filled with number 10.
> nc.find(10); // Number 10 is at the indices 1, 2, 3, and 5. Since the smallest index that is filled with 10 is 1, we return 1.
> nc.change(1, 20); // Your container at index 1 will be filled with number 20. Note that index 1 was filled with 10 and then replaced with 20. 
> nc.find(10); // Number 10 is at the indices 2, 3, and 5. The smallest index that is filled with 10 is 2. Therefore, we return 2.

**Constraints:**

-   `1 <= index, number <= 109`
-   At most `105` calls will be made **in total** to `change` and `find`.

---
```java
// Java 55ms(Beats 96.58%), Time O(NlogN), Space O(N)
class NumberContainers {
    HashMap<Integer, PriorityQueue<Integer>> numToIndex;
    HashMap<Integer, Integer> indexToNum;
    public NumberContainers() {
        numToIndex = new HashMap<>();
        indexToNum = new HashMap<>();
    }
    
    public void change(int index, int number) {
        indexToNum.put(index, number);
        numToIndex.computeIfAbsent(number, v -> new PriorityQueue<>()).offer(index);
    }
    
    public int find(int number) {
        if (!numToIndex.containsKey(number))
            return -1;

        PriorityQueue<Integer> pq = numToIndex.get(number);
        while (!pq.isEmpty())
        {
            int index = pq.peek();
            if (indexToNum.get(index) == number)
                return index;
            pq.poll();
        }
        return -1;
    }
}
```
---

要找到`number`的最小`index`, 利用`PriorityQueue`來實作, 並且透過`懶刪除`來找到答案
若當前的最小`index`存放的不是`number`, 代表該`index`已被改為`其他number`
故從`PQ`剔除, 並檢查下一個, 直到該`index`中存放的是`number`


###### tags: `Leetcode` `HashMap`