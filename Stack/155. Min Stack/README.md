# Leetcode - 155. Min Stack (M)

[Leetcode](https://leetcode.com/problems/min-stack/description/)

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:

-   `MinStack()` initializes the stack object.
-   `void push(int val)` pushes the element `val` onto the stack.
-   `void pop()` removes the element on the top of the stack.
-   `int top()` gets the top element of the stack.
-   `int getMin()` retrieves the minimum element in the stack.

You must implement a solution with `O(1)` time complexity for each function.

**Example 1:**
```
Input:
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output:
[null,null,null,null,-3,null,0,-2]

Explanation:
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2
```
**Constraints:**

-   `-231 <= val <= 231 - 1`
-   Methods `pop`, `top` and `getMin` operations will always be called on **non-empty** stacks.
-   At most `3 * 104` calls will be made to `push`, `pop`, `top`, and `getMin`.

---
```java
// Java
class MinStack {
    Node head;
    public MinStack() {

    }
    
    public void push(int val) {
        if (head == null) {
            head = new Node(val, val, null);
        } else {
            head = new Node(val, Math.min(head.min, val), head);
        }
    }
    
    public void pop() {
        head = head.next;
    }
    
    public int top() {
        return head.val;
    }
    
    public int getMin() {
        return head.min;
    }

    class Node {
        int val;
        int min;
        Node next;

        Node(int val, int min, Node next) {
            this.val = val;
            this.min = min;
            this.next = next;
        }
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(val);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```

```java
// Java
class MinStack {
    Deque<Long> stack;
    long min;
    public MinStack() {
        stack = new ArrayDeque();
    }
    
    public void push(int val) {
        if (stack.isEmpty()) {
            stack.push(0L);
            min = val;
        } else {
            long diff = val - min;
            stack.push(diff);
            if (diff < 0) {
                min = val;
            }
        }
    }
    
    public void pop() {
        long diff = stack.pop();
        if (diff < 0) {
            min = min - diff;
        }
    }
    
    public int top() {
        long diff = stack.peek();
        if (diff >= 0) {
            return (int) (min + diff);
        } else {
            return (int) min;
        }
    }
    
    public int getMin() {
        return (int) min;
    }
}
```
---

> 解法1:

要做到min()為O(1), 將每個元素當下的min都記錄起來

> 解法2:

有一種更節省空間的方法

進入Stack時, 不放入X而是放入差值, `差值 = X - Min`
離開Stack時, 檢查是否`差值 < 0`, `< 0`代表當時放進的為min, 
因此需要更新min為`min - 差值`

###### tags: `Leetcode` `Stack`