# Leetcode - 380. Insert Delete GetRandom O(1) (M+)

[Leetcode](https://leetcode.com/problems/insert-delete-getrandom-o1/description/)

Implement the `RandomizedSet` class:

-   `RandomizedSet()` Initializes the `RandomizedSet` object.
-   `bool insert(int val)` Inserts an item `val` into the set if not present. Returns `true` if the item was not present, `false` otherwise.
-   `bool remove(int val)` Removes an item `val` from the set if present. Returns `true` if the item was present, `false` otherwise.
-   `int getRandom()` Returns a random element from the current set of elements (it's guaranteed that at least one element exists when this method is called). Each element must have the **same probability** of being returned.

You must implement the functions of the class such that each function works in **average** `O(1)` time complexity.

**Example 1:**
```
Input
["RandomizedSet", "insert", "remove", "insert", "getRandom", "remove", "insert", "getRandom"]
[[], [1], [2], [2], [], [1], [2], []]
Output
[null, true, false, true, 2, true, false, 2]

Explanation
RandomizedSet randomizedSet = new RandomizedSet();
randomizedSet.insert(1); // Inserts 1 to the set. Returns true as 1 was inserted successfully.
randomizedSet.remove(2); // Returns false as 2 does not exist in the set.
randomizedSet.insert(2); // Inserts 2 to the set, returns true. Set now contains [1,2].
randomizedSet.getRandom(); // getRandom() should return either 1 or 2 randomly.
randomizedSet.remove(1); // Removes 1 from the set, returns true. Set now contains [2].
randomizedSet.insert(2); // 2 was already in the set, so return false.
randomizedSet.getRandom(); // Since 2 is the only number in the set, getRandom() will always return 2.
```
**Constraints:**

-   `-231 <= val <= 231 - 1`
-   At most `2 * ``105` calls will be made to `insert`, `remove`, and `getRandom`.
-   There will be **at least one** element in the data structure when `getRandom` is called.

---
```java
// Java
class RandomizedSet {
    HashMap<Integer, Integer> map;
    ArrayList<Integer> list;
    Random random = new Random();
    public RandomizedSet() {
        map = new HashMap<>();
        list = new ArrayList<>();
    }
    
    public boolean insert(int val) {
        if (map.containsKey(val))
            return false;
        int index = list.size();
        list.add(val);
        map.put(val, index);
        return true;
    }
    
    public boolean remove(int val) {
        if (!map.containsKey(val))
            return false;
        int lastIndex = list.size() - 1;
        int valIndex = map.get(val);
        // put last index to val's index, and update hashmap
        if (valIndex < lastIndex) {
            list.set(valIndex, list.get(lastIndex));
            map.put(list.get(lastIndex), valIndex);
        }
        list.remove(lastIndex);
        map.remove(val);
        return true;
    }
    
    public int getRandom() {
        return list.get(random.nextInt(list.size()));
    }
}

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet obj = new RandomizedSet();
 * boolean param_1 = obj.insert(val);
 * boolean param_2 = obj.remove(val);
 * int param_3 = obj.getRandom();
 */
```
---

學習ArrayList.set(index, value)

```
public E set(int index, E element)

參數
-   index -- 替換索引的元素。
-   element -- 要被存儲在指定位置的元素。

返回值
此方法返回在指定位置之前元素。
```


###### tags: `Leetcode` `Design`
