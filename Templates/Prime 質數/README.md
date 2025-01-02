# Leetcode - Prime 質數

參考[LeetCode/Template/Math/Primes.cpp at master · wisdompeak/LeetCode · GitHub](https://github.com/wisdompeak/LeetCode/blob/master/Template/Math/Primes.cpp)

```java
// Find all primes <= n.
List<Integer> Eratosthenes(int n)
{
    int[] q = new int[n + 1];
    List<Integer> primes = new ArrayList<>();
    for (int i = 2; i <= Math.sqrt(n); i++)
    {
        if (q[i] == 1) continue;
        int j = i * 2;
        while (j <= n)
        {
            q[j] = 1;
            j += i;
        }
    }

    for (int i = 2; i <= n; i++)
    {
        if (q[i] == 0)
            primes.add(i);                
    }
    
    return primes;
}
```


###### tags: `Leetcode` `Templates`