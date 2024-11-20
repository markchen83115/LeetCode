# Leetcode - 040. Combination Sum II (M+)

[Leetcode](https://leetcode.com/problems/combination-sum-ii/)

Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:** The solution set must not contain duplicate combinations.

**Example 1:**

**Input:** candidates = [10,1,2,7,6,1,5], target = 8
**Output:** 
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]

**Example 2:**

**Input:** candidates = [2,5,2,1,2], target = 5
**Output:** 
[
[1,2,2],
[5]
]

**Constraints:**

-   `1 <= candidates.length <= 100`
-   `1 <= candidates[i] <= 50`
-   `1 <= target <= 30`

---
```java
// Java 2ms(Beats 99.80%), Time O(2^N), Space O(N)
class Solution {
    List<List<Integer>> ret = new ArrayList<>();
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {   
        Arrays.sort(candidates);
        backtracking(candidates, target, new ArrayList<Integer>(), 0);
        return ret;
    }

    void backtracking(int[] candidates, int target, List<Integer> list, int k)
    {   
        if (target < 0)
            return;
        if (target == 0)
        {
            ret.add(new ArrayList(list));
            return;
        }
        for (int i = k; i < candidates.length; i++)
        {
            if (i > k && candidates[i] == candidates[i - 1])
                continue;   // skip duplicate
            if (candidates[i] > target) break;
            list.add(candidates[i]);
            backtracking(candidates, target - candidates[i], list, i + 1);
            list.remove(list.size() - 1);
        }

    }
}
```
---
參考[LeetCode/DFS/040.Combination-Sum-II at master · wisdompeak/LeetCode · GitHub](https://github.com/wisdompeak/LeetCode/tree/master/DFS/040.Combination-Sum-II#040combination-sum-ii)

與LC 039非常類似。但有一個非常重要的情況需要考慮。假設target=5，candidates={3,1,1}，那麼依照傳統的DFS方法，會搜尋到兩個組合{3,1}，這兩個組合中的1其實對應著的是candidates裡面不同的元素。如何有效率地去除這種重複的情況呢？

我們先將candidates排序。然後規定：凡是相同的元素，我們只能按先後次序連續取，而不能跳著取。例如上面的例子，我們如果需要取一個1，那麼就取第一個1.如果需要取兩個1，那麼就取前兩個1.以此類推。

那麼如何在程式碼中有效率地實現這個過濾呢？非常簡單。

```c
for (int i=idx; i<candidates.size(); i++)
{
 if (i>idx && candidates[i]==candidates[i-1])
 continue;
}
```

這段程式碼的脈絡是，在這一輪中，我們需要在`candidates[idx:end]`中間選取一個數字加入comb。如果我們選了candidates[i]，那麼表示candidates[i-1]就沒有被選中。此時如果發現`candidates[i]==candidates[i-1]`，則意味著相同的元素我們「跳著」選取了，這是上面指定的規則所不允許的，就可以終止。

當然有一個特例，如果i選中的就是candidates[idx]，那是可以豁免的。這是因為上一輪我們選的就是candidates[idx-1]。這樣即使`candidates[i]==candidates[i-1]`，其實剛好說明我們就是順著連續選取地這個元素。

這個處理技巧在DFS的題目中會經常遇到，需要能夠熟練。



###### tags: `Leetcode` `DFS`