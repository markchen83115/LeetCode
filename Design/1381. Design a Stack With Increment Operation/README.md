# Leetcode - 1381. Design a Stack With Increment Operation (H-)

[Leetcode](https://leetcode.com/problems/design-a-stack-with-increment-operation/)

Design a stack that supports increment operations on its elements.

Implement the `CustomStack` class:

-   `CustomStack(int maxSize)` Initializes the object with `maxSize` which is the maximum number of elements in the stack.
-   `void push(int x)` Adds `x` to the top of the stack if the stack has not reached the `maxSize`.
-   `int pop()` Pops and returns the top of the stack or `-1` if the stack is empty.
-   `void inc(int k, int val)` Increments the bottom `k` elements of the stack by `val`. If there are less than `k` elements in the stack, increment all the elements in the stack.

**Example 1:**
```
Input
["CustomStack","push","push","pop","push","push","push","increment","increment","pop","pop","pop","pop"]
[[3],[1],[2],[],[2],[3],[4],[5,100],[2,100],[],[],[],[]]
Output
[null,null,null,2,null,null,null,null,null,103,202,201,-1]
Explanation
CustomStack stk = new CustomStack(3); // Stack is Empty []
stk.push(1);                          // stack becomes [1]
stk.push(2);                          // stack becomes [1, 2]
stk.pop();                            // return 2 --> Return top of the stack 2, stack becomes [1]
stk.push(2);                          // stack becomes [1, 2]
stk.push(3);                          // stack becomes [1, 2, 3]
stk.push(4);                          // stack still [1, 2, 3], Do not add another elements as size is 4
stk.increment(5, 100);                // stack becomes [101, 102, 103]
stk.increment(2, 100);                // stack becomes [201, 202, 103]
stk.pop();                            // return 103 --> Return top of the stack 103, stack becomes [201, 202]
stk.pop();                            // return 202 --> Return top of the stack 202, stack becomes [201]
stk.pop();                            // return 201 --> Return top of the stack 201, stack becomes []
stk.pop();                            // return -1 --> Stack is empty return -1.
```
**Constraints:**

-   `1 <= maxSize, x, k <= 1000`
-   `0 <= val <= 100`
-   At most `1000` calls will be made to each method of `increment`, `push` and `pop` each separately.

---
```java
// Java [my way] 5ms(Beats 93.47%), Time O(N), Space O(N)
class CustomStack {
    Stack<Integer> stack;
    int[] offset;
    int maxSize;
    public CustomStack(int maxSize) {
        stack = new Stack<>();
        offset = new int[maxSize + 1];
        this.maxSize = maxSize;
    }
    
    public void push(int x) {
        // System.out.println("push "+x);
        if (stack.size() < maxSize)
        {
            stack.push(x);
        }
    }
    
    public int pop() {
        // System.out.println("pop");
        if (stack.isEmpty())
            return -1;

        int increments = offset[stack.size()];
        offset[stack.size()] = 0;
        int x = stack.pop() + increments;

        if (!stack.isEmpty())
            offset[stack.size()] += increments;

        return x;
    }
    
    public void increment(int k, int val) {
        // System.out.println("increment "+k+" : "+val);
        if (stack.isEmpty())
            return;

        k = Math.min(k, stack.size());
        offset[k] += val;
    }
}
//  0      1   2   3  4 5 6 7
//        +150   +100 
//                +50
```

```java
// Java [beautiful way] 5ms(Beats 93.47%), Time O(N), Space O(N)
class CustomStack {
    int n;
    int[] inc;
    Stack<Integer> stack;
    public CustomStack(int maxSize) {
        n = maxSize;
        inc = new int[n];
        stack = new Stack<>();
    }

    public void push(int x) {
        if (stack.size() < n)
            stack.push(x);
    }

    public int pop() {
        int i = stack.size() - 1;
        if (i < 0)
            return -1;
        if (i > 0)
            inc[i - 1] += inc[i];
        int res = stack.pop() + inc[i];
        inc[i] = 0;
        return res;
    }

    public void increment(int k, int val) {
        int i = Math.min(k, stack.size()) - 1;
        if (i >= 0)
            inc[i] += val;
    }
}
```
---

此題可暴力求解
但更優解為O(N), 利用差分數組求解
當遇到`increment(k, val)`時, 在`k`標註`+val`
因此當`pop()`時, 則可知道後續`pop()`需要再加上`val`


###### tags: `Leetcode` `Design`