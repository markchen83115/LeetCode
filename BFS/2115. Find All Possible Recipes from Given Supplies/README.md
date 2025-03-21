# Leetcode - 2115. Find All Possible Recipes from Given Supplies (M)

[Leetcode](https://leetcode.com/problems/find-all-possible-recipes-from-given-supplies/)

You have information about `n` different recipes. You are given a string array `recipes` and a 2D string array `ingredients`. The `ith` recipe has the name `recipes[i]`, and you can **create** it if you have **all** the needed ingredients from `ingredients[i]`. A recipe can also be an ingredient for **other **recipes, i.e., `ingredients[i]` may contain a string that is in `recipes`.

You are also given a string array `supplies` containing all the ingredients that you initially have, and you have an infinite supply of all of them.

Return _a list of all the recipes that you can create. _You may return the answer in **any order**.

Note that two recipes may contain each other in their ingredients.

**Example 1:**

> **Input:** recipes = ["bread"], ingredients = [["yeast","flour"]], supplies = ["yeast","flour","corn"]
> **Output:** ["bread"]
> **Explanation:**
> We can create "bread" since we have the ingredients "yeast" and "flour".

**Example 2:**

> **Input:** recipes = ["bread","sandwich"], ingredients = [["yeast","flour"],["bread","meat"]], supplies = ["yeast","flour","meat"]
> **Output:** ["bread","sandwich"]
> **Explanation:**
> We can create "bread" since we have the ingredients "yeast" and "flour".
> We can create "sandwich" since we have the ingredient "meat" and can create the ingredient "bread".

**Example 3:**

> **Input:** recipes = ["bread","sandwich","burger"], ingredients = [["yeast","flour"],["bread","meat"],["sandwich","meat","bread"]], supplies = ["yeast","flour","meat"]
> **Output:** ["bread","sandwich","burger"]
> **Explanation:**
> We can create "bread" since we have the ingredients "yeast" and "flour".
> We can create "sandwich" since we have the ingredient "meat" and can create the ingredient "bread".
> We can create "burger" since we have the ingredient "meat" and can create the ingredients "bread" and "sandwich".

**Constraints:**

-   `n == recipes.length == ingredients.length`
-   `1 <= n <= 100`
-   `1 <= ingredients[i].length, supplies.length <= 100`
-   `1 <= recipes[i].length, ingredients[i][j].length, supplies[k].length <= 10`
-   `recipes[i], ingredients[i][j]`, and `supplies[k]` consist only of lowercase English letters.
-   All the values of `recipes` and `supplies` combined are unique.
-   Each `ingredients[i]` does not contain any duplicate values.

---
```java
// Java 65ms(Beats 95.09%), Time O(N), Space O(N)
class Solution {
    public List<String> findAllRecipes(String[] recipes, List<List<String>> ingredients, String[] supplies) {
        HashMap<String, List<String>> next = new HashMap<>();
        HashMap<String, Integer> inDegree = new HashMap<>();
        HashSet<String> recipeSet = new HashSet<>();

        int n = recipes.length;
        for (int i = 0; i < n; i++)
        {
            String r = recipes[i];
            List<String> list = ingredients.get(i);
            recipeSet.add(r);

            for (String ingre : list)
            {
                next.computeIfAbsent(ingre, v -> new ArrayList<>()).add(r);
                inDegree.merge(r, 1, Integer::sum);
            }
        }

        Queue<String> queue = new LinkedList<>();
        for (String s : supplies)
            queue.offer(s);
        
        List<String> ret = new ArrayList<>();
        while (!queue.isEmpty())
        {
            String cur = queue.poll();
            if (recipeSet.contains(cur))
                ret.add(cur);
            if (!next.containsKey(cur))
                continue;
            for (String nxt : next.get(cur))
            {
                inDegree.merge(nxt, -1, Integer::sum);
                if (inDegree.get(nxt) == 0)
                    queue.offer(nxt);
            }
        }

        return ret;
    }
}
```
---

此題目非常類似那些「先修課程」的題目：必須已經造訪過某幾個節點（ingredient）之後才能存取一個新節點（recipe）
所以用拓樸排序是非常自然的想法

具體做法：
BFS隊列的初始節點就是那些supplies
每次從隊列裡彈出一個物品，查看它可能「解鎖」哪些新物品，確認這個新物品的所有條件都已經滿足（`inDegree`為零）之後就把其加入隊列。


###### tags: `Leetcode` `BFS` `拓撲排序`