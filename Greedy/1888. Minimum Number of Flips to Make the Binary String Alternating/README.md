# Leetcode - 1888. Minimum Number of Flips to Make the Binary String Alternating (M+)

[Leetcode](https://leetcode.com/problems/minimum-number-of-flips-to-make-the-binary-string-alternating/)

You are given a binary string `s`. You are allowed to perform two types of operations on the string in any sequence:

-   **Type-1: Remove** the character at the start of the string `s` and **append** it to the end of the string.
-   **Type-2: Pick** any character in `s` and **flip** its value, i.e., if its value is `'0'` it becomes `'1'` and vice-versa.

Return _the **minimum** number of **type-2** operations you need to perform_ _such that _`s` _becomes **alternating**._

The string is called **alternating** if no two adjacent characters are equal.

-   For example, the strings `"010"` and `"1010"` are alternating, while the string `"0100"` is not.

**Example 1:**

> **Input:** s = "111000"
> **Output:** 2
> **Explanation**: Use the first operation two times to make s = "100011".
> Then, use the second operation on the third and sixth elements to make s = "101010".

**Example 2:**

> **Input:** s = "010"
> **Output:** 0
> **Explanation**: The string is already alternating.

**Example 3:**

> **Input:** s = "1110"
> **Output:** 1
> **Explanation**: Use the second operation on the second element to make s = "1010".

**Constraints:**

-   `1 <= s.length <= 105`
-   `s[i]` is either `'0'` or `'1'`.

---
```java
// Java three-pass 13ms(Beats 98.82%), Time O(N), Space O(N)
class Solution {
    public int minFlips(String s) {
        int n = s.length();
        int ret = n;
        int[] left10 = new int[n];
        int[] right01 = new int[n];
        int count = 0;
        char k = '1';
        for (int i = 0; i < n; i++)
        {
            if (s.charAt(i) != k)
                count++;
            left10[i] = count;
            k = k == '1' ? '0' : '1';
        }

        count = 0;
        k = '0';
        for (int i = n - 1; i >= 0; i--)
        {
            if (s.charAt(i) != k)
                count++;
            right01[i] = count;
            k = k == '1' ? '0' : '1';
        }

        ret = Math.min(left10[n - 1], right01[0]);
        for (int i = 0; i < n - 1; i++)
        {
            int sum = left10[i] + right01[i + 1];
            ret = Math.min(ret, Math.min(sum, n - sum));
        }

        return ret;        
    }
}
```
```java
// Java sliding window 21ms(Beats 79.53%), Time O(N), Space O(1)
class Solution {
    public int minFlips(String s) {
        int n = s.length();
        int ret = n;
        int count = 0;
        for (int i = 0; i < n * 2; i++)
        {
            int j = i % n;
            if (s.charAt(j) - '0' == (i % 2 == 0 ? 1 : 0))
                count++;
            
            if (i >= n)
                if (s.charAt(i - n) - '0' == ((i - n) % 2 == 0 ? 1 : 0))
                    count--;

            if (i >= n - 1)
                ret = Math.min(ret, Math.min(count, n - count));
        }

        return ret;        
    }
}
// 奇數
// 11010 11010
// 01010 10...

// 偶數
// 1101 1101
// 0101 01..
```
---

### Three-pass

參考[【每日一题】1888. Minimum Number of Flips to Make the Binary String Alternating, 6/9/2021 - YouTube](https://youtu.be/shqRII8gvCo)

首先，flip和rotate兩個操作彼此之間不會有任何影響。例如你有a次flip，b次rotate，那麼打亂順序去實現這a+b次操作，結果都完全一樣。所以我們不妨考慮先做flip，再做rotate。言下之意，先做flip，然後將序列前端的某一段翻轉之後拼在序列後端。

如果flip操作完之後已經是交替序列了，那就不需要rotate。那麼rotate能帶來什麼好處呢？唯一的好處就是類似這種情況：
```
0101 1010101
```
前後半段都是交替序列，但整體並不是交替序列，因為中間有兩個連續的1.此時用flip再做調整顯然是低效的。我們發現，如果我們將前面那段轉移到後面去，就解決了這個問題。當然，能翻轉的先決條件是，前端的頭是0，後段的尾是1. 類似的，如果前段的頭是1，後段的尾是0的話，也可以做這樣的rotate，省下flip的操作。

這樣來看，我們只要將前後兩個序列各自弄成交替序列就行了。

那麼要如何確定這個分界點呢？自然想到會列舉這個位置。加入分界點在i和i+1的地方，這時候我們考慮的是：前段從左到右的0101序列，後段從右往左的1010序列，然後可以在這個地方斷開rotate。或前段從左到右的1010序列，後段從右往左的0101列，然後也可以在這點斷開rotate。我們容易知道，可以用one pass計算從左到右0101序列延伸到每個位置時的flip操作數，同理也可以提前預處理從左往右1010序列延伸到每個位置時的flip操作數、從右往做0101序列延伸到每個位置時的flip操作數、從右往1010序列延伸到每個位置時的flip操作數。

最後的答案就是枚舉端點位置i，考察`left0[i]+right1[i+1]`和`left1[i]+right0[i+1]`的較小值。

### Sliding Window

因為可以rotate, 因此將s擴展為兩倍方便遍歷

題目要求最後為`01010101`或`1010101`
而只要計算其中一種的`flip次數`, 另一種即為`n - flip次數`


###### tags: `Leetcode` `Greedy` `Three-pass`