# Coin Change 2

[Leetcode 518](https://leetcode.com/problems/coin-change-2/)
这道dp题对我来说比较难理解，首先看题：

![](../lc518.png)

### 1. DFS + Backtracking:

Because we want to find all the possible combinations of the given amount, it is easy for me to think of the backtracking methods:

'''java
<!-- Backtracking Solution:(TLE) -->
     public int change(int amount, int[] coins) {
         return dfs(amount, coins, 0);
     }

     private int dfs(int amount, int[] coins, int p) {
         if (amount < 0) return 0;
         if (amount == 0) return 1;
         int res = 0;
         for (int i = p; i < coins.length; i++) {
             int coin = coins[i];
             int subres = dfs(amount-coin, coins, i);
             res += subres;
         }
         return res;
     }
'''
