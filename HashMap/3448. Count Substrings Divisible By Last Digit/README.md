# Leetcode - 3448. Count Substrings Divisible By Last Digit (H-)

[Leetcode](https://leetcode.com/problems/count-substrings-divisible-by-last-digit/)

You are given a string `s` consisting of digits.

Return the **number** of substrings of `s` **divisible** by their **non-zero** last digit.

**Note**: A substring may contain leading zeros.

**Example 1:**

**Input:** s = "12936"

**Output:** 11

**Explanation:**

Substrings `"29"`, `"129"`, `"293"` and `"2936"` are not divisible by their last digit. There are 15 substrings in total, so the answer is `15 - 4 = 11`.

**Example 2:**

**Input:** s = "5701283"

**Output:** 18

**Explanation:**

Substrings `"01"`, `"12"`, `"701"`, `"012"`, `"128"`, `"5701"`, `"7012"`, `"0128"`, `"57012"`, `"70128"`, `"570128"`, and `"701283"` are all divisible by their last digit. Additionally, all substrings that are just 1 non-zero digit are divisible by themselves. Since there are 6 such digits, the answer is `12 + 6 = 18`.

**Example 3:**

**Input:** s = "1010101010"

**Output:** 25

**Explanation:**

Only substrings that end with digit `'1'` are divisible by their last digit. There are 25 such substrings.

**Constraints:**

-   `1 <= s.length <= 105`
-   `s` consists of digits only.

---
```java
// Java 125ms(Beats 100.00%), Time O(N * 10 * 10), Space O(10)
class Solution {
    public long countSubstrings(String s) {
        long ret = 0;
        int n = s.length();
        int[] nums = new int[n];
        for (int i = 0; i < n; i++)
            nums[i] = s.charAt(i) - '0';
        
        for (int d = 1; d <= 9; d++)
            ret += helper(nums, d);

        return ret;
    }

    long helper(int[] nums, int d)
    {
        long ret = 0;
        int n = nums.length;
        int[] count = new int[d];
        int k = 0;
        count[0] = 1;
        for (int i = 0; i < n; i++)
        {
            int[] count_new = new int[d];
            for (int r = 0; r < d; r++)
                count_new[r * 10 % d] += count[r];
            
            count = count_new;
            k = (k * 10 + nums[i]) % d;

            if (nums[i] == d)
                ret += count[k];
            
            count[k]++; 
        }

        return ret;
    }
}
```
---

參考[【每日一题】LeetCode 3448. Count Substrings Divisible By Last Digit - YouTube](https://youtu.be/7VDPEki9qX4)

這題很容易想到能否用「前綴+Hash」的套路來做。

假設`yyyxxxi`被d除的餘數是r，那麼我們能否找出某一段後綴是能被d整除的呢？我們不禁會想，如果前綴`yyy`被d除的餘數恰好也是r，那麼是否意味著`xxxi`就是能被d整除的呢？其實並不是。 `xxxi`能被d整除的充要條件其實是`yyy0000`和`yyyxxxi`對於d同餘，而不是`yyy`和`yyyxxxi`對於d同餘。

於是我們發現，要判定以i為結尾的substring是否能被d整除，我們在Hash裡就不能查找有多少「原始的前綴」的餘數，而是應該查找有多少「升級後的前綴」的餘數。比方說，當我們考察`abcdi`時，我們在Hash裡記錄的其實應該是`a0000`,`ab000`,`abc00`,`abcd0`對於d餘數。這樣方便我們和`abcdi`對於d的餘數進行對照。如果有同餘的，表示目前有對應一段後綴能被d整除。

`abcd0`對於d餘數從哪裡來，其實就是`abcd`對於d餘數r，再做變化r*10%r.

`abc00`對於d餘數從哪裡來，其實就是`abc0`對於d餘數r，再做變化得到`r*10%r`. 那麼`abc0`對於d餘數又從哪裡來，其實就是`abc`對於d餘數r，再做變化r*10%r.

由此類推，我們只需要在每一輪，將Hash裡存放的餘數頻次進行變化即可。比如說目前count裡存放的是`a000`,`ab00`,`abc0`,`abcd`的餘數頻次，那麼

```java
for (int r = 0; r < k; r++)
  count_new[r * 10 % r] += count[r];
```

count_new裡存放的就是`a0000`,`ab000`,`abc00`,`abcd0`的餘數頻次.

由此本題迎刃而解。我們循環10遍，對於d=0,1,2,..,9分別進行計算，逐位考察存在多少以當前字符結尾的substring能被d整除。採用上述的「同餘前綴」的思路來計數。


###### tags: `Leetcode` `Hash Map` `Hash+Prefix`