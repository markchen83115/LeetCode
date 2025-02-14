# Leetcode - 1352. Product of the Last K Numbers (M+)

[Leetcode](https://leetcode.com/problems/product-of-the-last-k-numbers/)

Design an algorithm that accepts a stream of integers and retrieves the product of the last `k` integers of the stream.

Implement the `ProductOfNumbers` class:

-   `ProductOfNumbers()` Initializes the object with an empty stream.
-   `void add(int num)` Appends the integer `num` to the stream.
-   `int getProduct(int k)` Returns the product of the last `k` numbers in the current list. You can assume that always the current list has at least `k` numbers.

The test cases are generated so that, at any time, the product of any contiguous sequence of numbers will fit into a single 32-bit integer without overflowing.

**Example:**

> **Input**
> ["ProductOfNumbers","add","add","add","add","add","getProduct","getProduct","getProduct","add","getProduct"]
> [[],[3],[0],[2],[5],[4],[2],[3],[4],[8],[2]]
> 
> **Output**
> [null,null,null,null,null,null,20,40,0,null,32]
> 
> **Explanation**
> ProductOfNumbers productOfNumbers = new ProductOfNumbers();
> productOfNumbers.add(3);        // [3]
> productOfNumbers.add(0);        // [3,0]
> productOfNumbers.add(2);        // [3,0,2]
> productOfNumbers.add(5);        // [3,0,2,5]
> productOfNumbers.add(4);        // [3,0,2,5,4]
> productOfNumbers.getProduct(2); // return 20. The product of the last 2 numbers is 5 * 4 = 20
> productOfNumbers.getProduct(3); // return 40. The product of the last 3 numbers is 2 * 5 * 4 = 40
> productOfNumbers.getProduct(4); // return 0. The product of the last 4 numbers is 0 * 2 * 5 * 4 = 0
> productOfNumbers.add(8);        // [3,0,2,5,4,8]
> productOfNumbers.getProduct(2); // return 32. The product of the last 2 numbers is 4 * 8 = 32 

**Constraints:**

-   `0 <= num <= 100`
-   `1 <= k <= 4 * 104`
-   At most `4 * 104` calls will be made to `add` and `getProduct`.
-   The product of the stream at any point in time will fit in a **32-bit** integer.

**Follow-up: **Can you implement **both** `GetProduct` and `Add` to work in `O(1)` time complexity instead of `O(k)` time complexity?

---
```java
// Java 16ms(Beats 63.31%), Time O(1), Space O(N)
class ProductOfNumbers {
    List<Integer> list;
    public ProductOfNumbers() {
        list = new ArrayList<>();
        list.add(1);
    }
    
    public void add(int num) {
        if (num > 0)
        {
            list.add(list.get(list.size() - 1) * num);
        }
        else
        {
            list = new ArrayList<>();
            list.add(1);
        }
    }
    
    public int getProduct(int k) {
        int n = list.size();
        if (k > n - 1)
            return 0;
        
        return list.get(n - 1) / list.get(n - 1 - k);
    }
}

/**
 * Your ProductOfNumbers object will be instantiated and called as such:
 * ProductOfNumbers obj = new ProductOfNumbers();
 * obj.add(num);
 * int param_2 = obj.getProduct(k);
 */
```
---

本題看第一眼就知道解法是構造前綴乘積的數組，令pre[i]表示從nums[1]連續乘到nums[i]的積。假設目前已經有n個元素，那麼最後k個元素的乘積就是pre[n]/pre[n-k]。注意本題的約束條件裡保證了前綴乘積數組不會溢出。

但本題就這麼簡單嗎？其實本題需要考察的是當nums[i]=0的情況。我們發現一旦加入了0，那麼會讓當前乃至之後的pre永遠都是0，於是在getProduct時的計算公式pre[n]/pre[n-k]的表達式就會有除數為0的風險。那麼如何解決這個問題呢？

首先，如果從n往前數的k個數包括了0，那麼最終答案就是回傳0. 其次，如果這k個數不包括0的話，那麼如何保證pre[n]/pre[n-k]一定合法呢？我們可以認為把最近的0之前的數字都忽略掉，將整個pre數組從最近的0之後開始重新計數：也就是當nums[i]==1時，令pre[i]=1。


###### tags: `Leetcode` `Design`