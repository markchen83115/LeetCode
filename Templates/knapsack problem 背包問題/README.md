# Leetcode - knapsack problem 背包問題

### 01背包: 每個物品只能用一次

```java
for i in obj:			// 遍歷物品
	for c in capcity:	// 遍歷容量		
        dp[c] = max(dp[c], dp[c-cost[i]]+val[i];
```


### 完全背包: 每個物品可以用無限次

```java
for c in capcity:		// 遍歷容量	
    for i in obj:		// 遍歷物品
        dp[c] = max(dp[c], dp[c-cost[i]]+val[i];
```

###### tags: `Leetcode` `Templates`