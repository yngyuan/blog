---

title: "Best Time to Buy and Sell Stock"
date: 2020-07-13T17:20:39-05:00
lastmod: 2020-07-13T17:20:39-05:00
draft: false
keywords: ["dynamic programming", "algorithms"]
description: ""
tags: ["algorithms", "dynamic programming"]
categories: ["leetcode"]
author: ""

# You can also close(false) or open(true) something for this content.
# P.S. comment can only be closed
comment: false
toc: true
autoCollapseToc: false
# You can also define another contentCopyright. e.g. contentCopyright: "This is another copyright."
contentCopyright: false
reward: false
mathjax: false
---

<!--more-->



## Intro

The best time to buy and sell stock is a popular problem in interviews. The problem can gengerally be described as:

- Given an array prices[], where prices[i] is the price of a given stock on day i. len = prices.length.
- You are only permitted to complete at most K transactions
- You cannot sell a stock before you buy one. (Obviously)

If you think about it, problem 1 is the case when K = 1, Problem 2,3,4 is the case when K = Infinity. Problem 5 is the case when K = 2. And problem 6 is the most general case described above.

## General Approach

### Exhaustive

We could find all possible outcomes in an exhaustice way. We have 3 states to record. 

- i, on which day, i is from [0, len-1]
- K, max transaction time till now
- 1/0, have a stock at hand or not.

In total, we have prices.length \* K \* 2 possible outcomes which we could save in a 3d array dp\[len]\[K]\[2]. For example, dp\[3]\[2]\[1] would mean on day **3**, at most **K** transactions till now, and currently I **have** a stock at hand.

Our final answer would be dp\[len-1]\[K]\[0]. Obviously it is bigger than dp\[len-1]\[K]\[1] because we sold that 1 at hand.

### State Transition

```java
// Possible state if I do not have a stock at hand today.
// 1. I didn't have stock yesterday, and I rest today.
// 2. I had a stock yesterday, and I sold it for prices[i] today.

dp[i][k][0] = Math.max(dp[i-1][k][0], dp[i-1][k][1] + prices[i]);
								\\ max(  rest, do nothing  ,  sell the stock)

// Possible state if I do have a stock at hand today
// 1. I had the stock yesterday, and I rest today.
// 2. I didn't have a stock yesterday, and I buy one today at the price of prices[i].
dp[i][k][1] = Math.max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i]);
							  \\ max(   rest, do nothing  ,  buy the stock)
```

Now think about the base case, which is pretty straight forward.

```java
// base case：
dp[-1][k][0] = 0;
dp[i][0][0] = 0;
dp[-1][k][1] = Integer.MIN_VALUE;
dp[i][0][1] = Integer.MIN_VALUE;

// State Transition Function：
dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i]);
dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i]);
```

## 121 Best Time to Buy and Sell Stock

Say you have an array for which the *i*th element is the price of a given stock on day *i*.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

**Example 1:**

```
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.
```

### Analysis

This is the case when K=1. So we could simplify the general approach because K is not really affecting the state.

```
dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i]);
dp[i][1] = max(dp[i-1][1], -prices[i]);
```

Furthermore, Current state is only affected by the adjacent state. We could just use a variable to store the previous state. This way we could make the space complexity O(1).

### Solution

1. Brute Force, Time Complexity: O(N^2), Space Complexity: O(1)

```java
class Solution {
    public int maxProfit(int[] prices) {
        int len = prices.length;
        int ans = 0;     
        for(int i=0; i<len; i++){
            for(int j=i; j<len; j++){
                int profit = prices[j]-prices[i];
                if(profit>ans){
                    ans = profit;
                }
            }
        }     
        return ans;
    }
}
```

2. DP, Time Complexity: O(N), Space Complexity: O(N)

```java
class Solution {
    public int maxProfit(int[] prices) {
        int len = prices.length;
        if(len<1) return 0;
        // on ith day, current state 0/1
        int[][] dp = new int[len][2];
        
        for(int i=0; i<len; i++){
            if(i-1==-1){
                dp[i][0] = 0;
                dp[i][1] = -prices[i];
                continue;
            }
            dp[i][0] = Math.max(dp[i-1][1]+prices[i], dp[i-1][0]);
            dp[i][1] = Math.max(-prices[i], dp[i-1][1]);
        }
        
        return dp[len-1][0];
    }
}
```



3. DP optimized, Time Complexity: O(N), Space Complexity: O(1).

```java
class Solution {
    public int maxProfit(int[] prices) {
        int len = prices.length;
        if(len<1) return 0;
        int dp_i_0 = 0;
        int dp_i_1 = Integer.MIN_VALUE;
        for(int i=0; i<len; i++){
            dp_i_0 = Math.max(dp_i_0, dp_i_1 + prices[i]);
            dp_i_1 = Math.max(dp_i_1, -prices[i]);
        }
        return dp_i_0;
    }
}
```



## 122 Best Time to Buy and Sell Stock II

Say you have an array `prices` for which the *i*th element is the price of a given stock on day *i*.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

**Note:** You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

**Example 1:**

```
Input: [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
             Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
```

### Analysis

This is the case when K is infinite. So we can view this as: for a state K and K-1 doesn't make any difference. So a 2D dp would do.

Current state is only affected by the adjacent state. We could just use a variable to store the previous state. This way we could make the space complexity O(1).

### Solution

1. DP, Time Complexity: O(N), Space Complexity: O(N).

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int[][] dp = new int[n][2];
        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        for(int i=1; i<n; i++){
            dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i]);
            dp[i][1] = Math.max(dp[i-1][1], dp[i-1][0] - prices[i]);
        }
        return dp[n-1][0];
    }
}
```

2. DP optimized, Time Complexity O(N), Space Complexity: O(1).

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int dp_i_0 = 0;
        int dp_i_1 = Integer.MIN_VALUE;
        for(int i=0; i<n; i++){
            int temp = dp_i_0;
            dp_i_0 = Math.max(dp_i_0, dp_i_1 + prices[i]);
            dp_i_1 = Math.max(dp_i_1, temp - prices[i]);
        }
        return dp_i_0;
    }
}
```

3. One pass, O(N), O(1)

```java
class Solution {
    public int maxProfit(int[] prices) {
        int maxprofit = 0;
        for (int i = 1; i < prices.length; i++) {
            if (prices[i] > prices[i - 1])
                maxprofit += prices[i] - prices[i - 1];
        }
        return maxprofit;
    }
}
```



## 309 Best Time to Buy and Sell Stock with Cooldown

Say you have an array for which the *i*th element is the price of a given stock on day *i*.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times) with the following restrictions:

- You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
- After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)

**Example:**

```
Input: [1,2,3,0,2]
Output: 3 
Explanation: transactions = [buy, sell, cooldown, buy, sell]
```

### Analysis

This is a variation of the previous question. We need to change the state transition change. The current state i when you decide buy/sell is transitioned from i-2 instead of i-1.

```
dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i]);
dp[i][1] = max(dp[i-1][1], dp[i-2][0] - prices[i]);
```

### Solution

1. DP, O(N), O(N)

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        if(n<=1) return 0;
        int[][] dp = new int[n][2];
        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        dp[1][0] = Math.max(prices[1]-prices[0], 0);
        dp[1][1] = Math.max(-prices[0], -prices[1]);
        for(int i=2; i<n; i++){
            dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i]);
            dp[i][1] = Math.max(dp[i-1][1], dp[i-2][0] - prices[i]);
        }
        return dp[n-1][0];
    }
}
```

2. DP optimized, O(N), O(1)

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        if(n<=1) return 0;
        int dp_i_0 = 0;
        int dp_i_1 = Integer.MIN_VALUE;
        // dp_i-2_0
        int dp_pre_0 = 0;
        for(int i=0; i<n; i++){
            int temp = dp_i_0;
            dp_i_0 = Math.max(dp_i_0, dp_i_1 + prices[i]);
            dp_i_1 = Math.max(dp_i_1, dp_pre_0 - prices[i]);
            dp_pre_0 = temp;
        }
        return dp_i_0;
    }
}
```



## 714 Best Time to Buy and Sell Stock with Transaction Fee

Your are given an array of integers `prices`, for which the `i`-th element is the price of a given stock on day `i`; and a non-negative integer `fee` representing a transaction fee.

You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction. You may not buy more than 1 share of a stock at a time (ie. you must sell the stock share before you buy again.)

Return the maximum profit you can make.

**Example 1:**

```
Input: prices = [1, 3, 2, 8, 4, 9], fee = 2
Output: 8
Explanation: The maximum profit can be achieved by:
Buying at prices[0] = 1Selling at prices[3] = 8Buying at prices[4] = 4Selling at prices[5] = 9The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
```

**Note:**

`0 < prices.length <= 50000`.

`0 < prices[i] < 50000`.

`0 <= fee < 50000`.

### Analysis

This is also a variation of the 2nd problem. We can just substract fee when selling the stock. Make a slight change to the state transaction function.

```
dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i]);
dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i] - fee);
```

### Solution

1. DP, O(N), O(N)

```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int n = prices.length;
        int[][] dp = new int[n][2];
        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        for(int i=1; i<n; i++){
            dp[i][0] = Math.max(dp[i-1][0], 
                                dp[i-1][1] + prices[i] - fee);
            dp[i][1] = Math.max(dp[i-1][1],
                                dp[i-1][0] - prices[i]);
        }
        return dp[n-1][0];
    }
}
```

2. DP optimized, O(N), O(1)

```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int n = prices.length;
        int dp_i_0 = 0;
        int dp_i_1 = -prices[0];
        for(int i=0; i<n; i++){
            int temp = dp_i_0;
            dp_i_0 = Math.max(dp_i_0, dp_i_1+ prices[i]-fee);
            dp_i_1 = Math.max(dp_i_1, temp-prices[i] );
        }
        return dp_i_0;
    }
}
```



## 123 Best Time to Buy and Sell Stock III

Say you have an array for which the *i*th element is the price of a given stock on day *i*.

Design an algorithm to find the maximum profit. You may complete at most *two* transactions.

**Note:** You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

**Example 1:**

```
Input: [3,3,5,0,0,3,1,4]
Output: 6
Explanation: Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
             Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.
```

### Analysis

This problem is trickier because we cannot just use the previous state transition function. Because for cases when k =1 or k=infinity, k doesn't really make a difference. This time, K = 2. And we have to exhaust all states from k.

### Solution

1. DP, O(N\*K), O(N\*K)

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        if(n<2) return 0;
        int maxK = 2;
        int[][][] dp = new int[n][maxK+1][2];
        // base case
        dp[0][2][0] = 0;
        dp[0][2][1] = -prices[0];
        dp[0][1][0] = 0;
        dp[0][1][1] = -prices[0];
        for(int i=1; i<n; i++){
            for(int k=maxK; k>0; k--){
                dp[i][k][0] = Math.max(dp[i-1][k][0], 
                                       dp[i-1][k][1]+prices[i]);
                dp[i][k][1] = Math.max(dp[i-1][k][1], 
                                       dp[i-1][k-1][0]-prices[i]);
            }
        }
        
        return dp[n-1][maxK][0];
    }
}
```

2. DP optimized

```java
class Solution {
    public int maxProfit(int[] prices) {
        int dp_i10 = 0, dp_i11 = Integer.MIN_VALUE;
        int dp_i20 = 0, dp_i21 = Integer.MIN_VALUE;
        for (int price : prices) {
            dp_i20 = Math.max(dp_i20, dp_i21 + price);
            dp_i21 = Math.max(dp_i21, dp_i10 - price);
            dp_i10 = Math.max(dp_i10, dp_i11 + price);
            dp_i11 = Math.max(dp_i11, -price);
        }
        return dp_i20;
    }
}
```



## 124 Best Time to Buy and Sell Stock IV

Say you have an array for which the *i-*th element is the price of a given stock on day *i*.

Design an algorithm to find the maximum profit. You may complete at most **k** transactions.

**Note:**
You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

**Example 1:**

```
Input: [2,4,1], k = 2
Output: 2
Explanation: Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.
```

### Analysis



### Solution

Simply change the previous code maxK=2 to maxK = k.

1. DP memory exceeded, passed 209/211 test case.

```java
class Solution {
    public int maxProfit(int p, int[] prices) {
        int n = prices.length;
        if(n<2) return 0;
        int maxK = p;
        int[][][] dp = new int[n][maxK+1][2];
        // base case
        for(int j=maxK; j>0; j--){
            dp[0][j][0] = 0;
            dp[0][j][1] = -prices[0];
        }

        for(int i=1; i<n; i++){
            for(int k=maxK; k>0; k--){
                dp[i][k][0] = Math.max(dp[i-1][k][0], 
                                       dp[i-1][k][1]+prices[i]);
                dp[i][k][1] = Math.max(dp[i-1][k][1], 
                                       dp[i-1][k-1][0]-prices[i]);
            }
        }
        
        return dp[n-1][maxK][0];        
    }
}
```

2. DP optimized

Notice when the k is too big, bigger than n/2, it is the same as infinite. Because buy and sell takes at least 2 days.

```java
class Solution {
    public int maxProfit(int max_k, int[] prices) {
        int n = prices.length;
        if (max_k > n / 2) 
            return maxProfit_k_inf(prices);

        int[][][] dp = new int[n][max_k + 1][2];
        for(int k=max_k; k>0; k--){
            dp[0][k][0] = 0;
            dp[0][k][1] = -prices[0];
        }
        for (int i = 1; i < n; i++) 
            for (int k = max_k; k >= 1; k--) {
                dp[i][k][0] = Math.max(dp[i-1][k][0], dp[i-1][k][1] + prices[i]);
                dp[i][k][1] = Math.max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i]);     
        }
        return dp[n - 1][max_k][0];       
    }
    
    public int maxProfit_k_inf(int[] prices) {
        int n = prices.length;
        int dp_i_0 = 0;
        int dp_i_1 = Integer.MIN_VALUE;
        for(int i=0; i<n; i++){
            int temp = dp_i_0;
            dp_i_0 = Math.max(dp_i_0, dp_i_1 + prices[i]);
            dp_i_1 = Math.max(dp_i_1, temp - prices[i]);
        }
        return dp_i_0;
    }
}
```



## Coda

Great, we have made this far and finished the Buy and Sell Stock Family. 