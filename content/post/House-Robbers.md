---
title: "House Robbers"
date: 2020-07-19T20:35:45-05:00
lastmod: 2020-07-19T20:35:45-05:00
draft: false
keywords: ["dynamic programming"]
description: ""
tags: ["dynamic programming"]
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

You are a professional robber planning to rob houses along a street. Sounds awesome? Actually not. There are 3 problems of House Robber on leetcode, let's dive into that. These are pretty classic dp problems. 

To solve such problems, we need to make sure we find 2 things:

- State
- Choice

In this case, we go from house to house facing two choices: Rob or Not Rob. 

- If we rob this one, we surely cannot rob the next one. Next house to rob is next next one. 
- If we do not rob this one, we can make the decision about rob the next one.
- If we have passed the last house, we have nothing to rob. 

So to conclude, the index of the house is our STATE, and Rob or Not Rob is our CHOICE.

## 198 House Robber

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

 

**Example 1:**

```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.
```

**Example 2:**

```
Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
             Total amount you can rob = 2 + 9 + 1 = 12.
```

 

**Constraints:**

- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 400`



### Solution

1. Dp recursive, TLE Error

```java
class Solution {
    public int rob(int[] nums) {
        return dp(nums, 0);
    }
    
    private int dp(int[] nums, int start){
        if(start>=nums.length){
            return 0;
        }
        // rob or not rob
        int res = Math.max(dp(nums, start+1),   
                          nums[start]+dp(nums, start+2));
        return res;
    }
}
```

2. DP with memo to prevent extra work. O(N)

```java
class Solution {
    private int[] memo;
    public int rob(int[] nums) {
        memo = new int[nums.length];
        Arrays.fill(memo, -1);
        return dp(nums, 0);
    }
    
    private int dp(int[] nums, int start){
        if(start>=nums.length){
            return 0;
        }
        // prevent extra work
        if(memo[start]!=-1){
            return memo[start];
        }
        // rob or not rob
        int res = Math.max(dp(nums, start+1),   
                          nums[start]+dp(nums, start+2));
        memo[start] = res;
        return res;
    }
}
```



## 213 House Robber II

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

**Example 1:**

```
Input: [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2),
             because they are adjacent houses.
```

**Example 2:**

```
Input: [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.
```

### Solution

1. dp, O(N)

```
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if(n==1) return nums[0];
        // either not rob the 1st, or the last
        return Math.max(robRange(nums, 0, n-2),
                        robRange(nums, 1, n-1));
    }
    
    private int robRange(int[] nums, int start, int end){
        int n = nums.length;
        int dp_i_1 = 0;
        int dp_i_2 = 0;
        int dp_i = 0;
        for(int i=end; i>=start; i--){
            dp_i = Math.max(dp_i_1, nums[i]+dp_i_2);
            dp_i_2 = dp_i_1;
            dp_i_1 = dp_i;
        }
        return dp_i;
    }
}
```



## 337 House Robber III

The thief has found himself a new place for his thievery again. There is only one entrance to this area, called the "root." Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that "all houses in this place forms a binary tree". It will automatically contact the police if two directly-linked houses were broken into on the same night.

Determine the maximum amount of money the thief can rob tonight without alerting the police.

**Example 1:**

```
Input: [3,2,3,null,3,null,1]

     3
    / \
   2   3
    \   \ 
     3   1

Output: 7 
Explanation: Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
```

**Example 2:**

```
Input: [3,4,5,1,3,null,1]

     3
    / \
   4   5
  / \   \ 
 1   3   1

Output: 9
Explanation: Maximum amount of money the thief can rob = 4 + 5 = 9.
```

### Solution

1. Dp with memo, O(N), O(N)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    Map<TreeNode, Integer> memo = new HashMap<>();
    public int rob(TreeNode root) {
        if(root==null) return 0;
        if(memo.containsKey(root)){
            return memo.get(root);
        }
        // rob, and go to next next
        int do_it = root.val
            + (root.left==null ? 0: rob(root.left.left)+rob(root.left.right))
            + (root.right==null ? 0: rob(root.right.left)+rob(root.right.right));
        int not_do = rob(root.left) + rob(root.right);
        int res = Math.max(do_it, not_do);
        memo.put(root, res);
        return res;
    }
}
```

2. A look at the brilliant, O(N), O(1)

```java
class Solution {
int rob(TreeNode root) {
    int[] res = dp(root);
    return Math.max(res[0], res[1]);
}

/* 
return: an arr of size 2
arr[0] max res not robbing root
arr[1] max res robbing root
*/
int[] dp(TreeNode root) {
    if (root == null)
        return new int[]{0, 0};
    int[] left = dp(root.left);
    int[] right = dp(root.right);
    // rob this root
    int rob = root.val + left[0] + right[0];
    // not rob this root
    int not_rob = Math.max(left[0], left[1])
                + Math.max(right[0], right[1]);

    return new int[]{not_rob, rob};
}
}
```

