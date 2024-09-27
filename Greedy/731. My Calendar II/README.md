# Leetcode - 731. My Calendar II (M+)

[Leetcode](https://leetcode.com/problems/my-calendar-ii/)

You are implementing a program to use as your calendar. We can add a new event if adding the event will not cause a **triple booking**.

A **triple booking** happens when three events have some non-empty intersection (i.e., some moment is common to all the three events.).

The event can be represented as a pair of integers `start` and `end` that represents a booking on the half-open interval `[start, end)`, the range of real numbers `x` such that `start <= x < end`.

Implement the `MyCalendarTwo` class:

-   `MyCalendarTwo()` Initializes the calendar object.
-   `boolean book(int start, int end)` Returns `true` if the event can be added to the calendar successfully without causing a **triple booking**. Otherwise, return `false` and do not add the event to the calendar.

**Example 1:**
```
Input
["MyCalendarTwo", "book", "book", "book", "book", "book", "book"]
[[], [10, 20], [50, 60], [10, 40], [5, 15], [5, 10], [25, 55]]
Output
[null, true, true, true, false, true, true]

Explanation
MyCalendarTwo myCalendarTwo = new MyCalendarTwo();
myCalendarTwo.book(10, 20); // return True, The event can be booked. 
myCalendarTwo.book(50, 60); // return True, The event can be booked. 
myCalendarTwo.book(10, 40); // return True, The event can be double booked. 
myCalendarTwo.book(5, 15);  // return False, The event cannot be booked, because it would result in a triple booking.
myCalendarTwo.book(5, 10); // return True, The event can be booked, as it does not use time 10 which is already double booked.
myCalendarTwo.book(25, 55); // return True, The event can be booked, as the time in [25, 40) will be double booked with the third event, the time [40, 50) will be single booked, and the time [50, 55) will be double booked with the second event.
```
**Constraints:**

-   `0 <= start < end <= 109`
-   At most `1000` calls will be made to `book`.

---
```java
// Java 69ms(Beats 65.17%), Time O(NlogN), Space O(N)
class MyCalendarTwo {
    List<int[]> books;
    public MyCalendarTwo() {
        books = new ArrayList<>();
    }
    
    public boolean book(int start, int end) {
        List<int[]> overlaps = new ArrayList<>();
        for (int[] book : books)
        {
            if (book[1] <= start || book[0] >= end)
                continue;
            overlaps.add(book);
        }
        Collections.sort(overlaps, (a, b) -> a[0] - b[0]);
        
        for (int i = 1; i < overlaps.size(); i++)
        {
            if (overlaps.get(i - 1)[1] > overlaps.get(i)[0])
                return false;
        }

        books.add(new int[] {start, end});
        return true;
    }
}
```
---

不需要考慮有序的hash
只需要找出所有與`[start, end)`重合的區間, 並檢查這些區間是否有重疊
是的話, 代表一定有**triple booking**


###### tags: `Leetcode` `Greedy` `Sort`