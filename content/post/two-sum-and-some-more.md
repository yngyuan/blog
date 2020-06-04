---
title: "Two Sum and Some More"
date: 2020-06-03T10:38:01-05:00
lastmod: 2020-06-03T10:38:01-05:00
draft: false
keywords: ["two sum", "leetcode"]
description: ""
tags: ["leetcode",""]
categories: ["tech", "algorithms"]
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

Two Sum is a classic interview question. There is a total of 6,437,613 submissions on leetcode. However, Days are gone when people could get into FANG companies solving Two Sum. Now we have to do some more. 

Today let's do all of Two Sum's family. I will be using java.



## Two Sum

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have **exactly** one solution, and you may not use the *same* element twice.

**Example:**

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```



#### Analysis

1. Brute Force

   It is quite intuitive that we could just do two loops on the given array, and try for each element if they add up to the target. Notice we cannot use the same element twice.

   

   Time Complexity: O(N^2), where N is the length of the array nums.

   Space Complexity: O(1), the only extra space we used is the return array.

2. Two-pass Hash Map

   Hash Map is always my favorite. Who doesn't like to sacrifice a little bit Space for Time? Loop the array once to make a hash map of the value to its index. Then loop again to find if the value of target-current value is in the map.



​		Time Complexity: O(N), 2 for loops on the array.

​		Space Complexity: O(N), used a hash map.

3. One-pass Hash Map

   Now think again, we don't really need 2 for loops. We could build the map as we go and just might get lucky and find the target sooner.



​		Time Complexity: O(N),  for-loop on the array. Each look up on hash map is only O(1).

​		Space Complexity: O(N), N is at most the length of the array.

4. Sort and Search?

   If we are returning a boolean value as to whether there could be a sum equal to target, we could sort the array and do a binary search. However, since we will be returning indexes, sorting would break the original order. Not a good idea.

#### Solution

1. Brute Force

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] res = new int[2];
        for(int i=0; i<nums.length; ++i){
            for(int j=0; j<nums.length; ++j){
                if(i!=j && nums[i]+nums[j]==target){
                    res[0] = i;
                    res[1] = j;
                }
            }
        }
        return res;
    }
}
```



2. Two-pass Hash Map

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] res = new int[2];
        HashMap<Integer, Integer> map = new HashMap<>();
        for(int i=0; i<nums.length; ++i){
            map.put(nums[i], i);
        }
        for(int j=0; j<nums.length; ++j){
            if(map.containsKey(target-nums[j]) && map.get(target-nums[j])!=j){
                res[0] = map.get(target-nums[j]);
                res[1] = j;
            }
        }
        return res;
    }
}
```



3. One-pass Hash Map

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] res = new int[2];
        HashMap<Integer, Integer> map = new HashMap<>();
        for(int j=0; j<nums.length; ++j){
            if(map.containsKey(target-nums[j]) && map.get(target-nums[j])!=j){
                res[0] = map.get(target-nums[j]);
                res[1] = j;
            }
            map.put(nums[j], j);
        }

        return res;
    }
}
```



## Two Sum Less Than K

Given an array `A` of integers and integer `K`, return the maximum `S` such that there exists `i < j` with `A[i] + A[j] = S` and `S < K`. If no `i, j` exist satisfying this equation, return -1.

 

**Example 1:**

```
Input: A = [34,23,1,24,75,33,54,8], K = 60
Output: 58
Explanation: 
We can use 34 and 24 to sum 58 which is less than 60.
```

**Example 2:**

```
Input: A = [10,20,30], K = 15
Output: -1
Explanation: 
In this case it's not possible to get a pair sum less that 15.
```

**Note:**

1. `1 <= A.length <= 100`
2. `1 <= A[i] <= 1000`
3. `1 <= K <= 2000`



#### Analysis

1. Brute Force

   We can use two for-loops to iterate every possible pair, see if the sum is greater than the current max sum and meanwhile smaller than the given K.



​		Time Complexity: O(N^2)

​		Space Complexity: O(1)

2. Sort and two-pointer

   We can sort the array first. Then loop the array, use two pointer from both ends to find a value, such that the sum is greater than current max sum and less than K.



​		Time Complexity: The average time complexity for Arrays.sort(int[]) is O(Nlog(N)), worst case O(N^2), where N is the length of the array.

​		Space Complexity: O(1)



#### Solution

1. Brute Force

```java
class Solution {
    public int twoSumLessThanK(int[] A, int K) {
        int res = -1;
        for(int i=0; i<A.length; ++i){
            for(int j=0; j<A.length; ++j){
                if(i!=j){
                    int sum = A[i]+A[j];
                    if(sum>res && sum<K){
                        res = sum;
                    }
                }
            }
        }
        return res;
    }
}
```



2. Sort and two-pointer 

```java
class Solution {
    public int twoSumLessThanK(int[] A, int K) 
    {   
        int max = -1;
        if(A==null || A.length<2)        
            return max;
        
        Arrays.sort(A);
        int l=0, r=A.length-1;
        while(l<r)
        {
            int sum=A[l]+A[r];
            if(sum<K){
                max=Math.max(max, sum);
                l+=1;
            } else{
                r-=1;
            }
        }
        return max;
    }
}
```



## Two Sum II - Input array is sorted

Given an array of integers that is already **sorted in ascending order**, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2.

**Note:**

- Your returned answers (both index1 and index2) are not zero-based.
- You may assume that each input would have *exactly* one solution and you may not use the *same* element twice.

**Example:**

```
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.
```



#### Analysis

1. Two pointers

   We can apply a similar approach like the last one. Use two pointers, one from the lower end and one from the other end.

   Because the poiner itself is an index, we don't need Hash Map to store the index.



​		Time Complexity: O(N) for worst case. 

​		Space Complexity: O(1)



#### Solution

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int[] res = new int[2];
        int l=0; int r=numbers.length-1;
        while(l<r){
            int sum = numbers[l]+numbers[r];
            if(sum>target){
                r-=1;
            }
            else if(sum<target){
                l+=1;
            }
            else{
                if(l!=r){
                    res[0] = l+1;
                    res[1] = r+1;
                    break;
                }
            }
        }
        return res;
    }
}
```



## Two Sum III - Data structure design

Design and implement a TwoSum class. It should support the following operations: `add` and `find`.

`add` - Add the number to an internal data structure.
`find` - Find if there exists any pair of numbers which sum is equal to the value.

**Example 1:**

```
add(1); add(3); add(5);
find(4) -> true
find(7) -> false
```

**Example 2:**

```
add(3); add(1); add(2);
find(3) -> true
find(6) -> false
```



#### Analysis

This question is a little different from the previous ones as this is a data structure design problem. And also becuase no clear preferences are given, we have to think about trade-offs of different implementation.

1. Sorted List

   This follows the idea of previous questions. We use an ArrayList to store the numbers. And when use find method, sort the numbers and apply the two-pointer approach. Also note that we should use ArrayList instead of a LinkedList, which is much slower when it comes to get a value at random position.

   

   Time Complexity:

   ​		Add: O(1)

   ​		Find: O(Nlog(N))

   Space Complexity: O(N)

2. Hash Map

   Use a hash map to store the number and its count. This count could come to use for case lie 8 + 8 = 16. 



​		Time Complexity:

​				Add: O(1)

​				Find: O(N)

​		Space Complexity: O(N)



3. Consider more add than find, use two Hash Set. Store all the possible sums in sum set. Store all the number in num set.

   

   Time Complexity: (TLE)

   ​		Add: O(N)

   ​		Find: O(1)

   Space Complexity: O(N^2) for worst case.

   

4. Maintain a list with distinct elements

   It is much faster to iterate a list than the key set of a map. Use a hash map to store the counts. Also, keep a min and max value of the numbers can help when there are some corner cases.

   Time Complexity: 

   ​		Add: O(1)

   ​		Find: O(N)

   Space Complexity: O(N)				

#### Solution

1. Sorted List

```java
import java.util.Collections;

class TwoSum {
  private ArrayList<Integer> nums;
  private boolean is_sorted;

  /** Initialize your data structure here. */
  public TwoSum() {
    this.nums = new ArrayList<Integer>();
    this.is_sorted = false;
  }

  /** Add the number to an internal data structure.. */
  public void add(int number) {
    this.nums.add(number);
    this.is_sorted = false;
  }

  /** Find if there exists any pair of numbers which sum is equal to the value. */
  public boolean find(int value) {
    if (!this.is_sorted) {
      Collections.sort(this.nums);
    }
    int l = 0, r = this.nums.size() - 1;
    while (l < r) {
      int sum = this.nums.get(l) + this.nums.get(r);
      if (sum < value)
        l += 1;
      else if (sum > value)
        r -= 1;
      else
        return true;
    }
    return false;
  }
}
```



2. Hash Map

```java
class TwoSum {
    private HashMap<Integer, Integer> num_counts;

    /** Initialize your data structure here. */
    public TwoSum() {
        this.num_counts = new HashMap<>();
    }
    
    /** Add the number to an internal data structure.. */
    public void add(int number) {
        this.num_counts.put(number, num_counts.getOrDefault(number, 0) + 1);
    }
    
    /** Find if there exists any pair of numbers which sum is equal to the value. */
    public boolean find(int value) {
        for(Integer key : this.num_counts.keySet()) {
            int complement = value - key;
            if(complement != key) {
                if(this.num_counts.containsKey(complement))
                    return true;
            } else if(this.num_counts.get(key) > 1)
                return true;
        }
        return false;
    }
}
```

3. Two Sets

```java
public class TwoSum {
        Set<Integer> sum;
        Set<Integer> num;
        
        TwoSum(){
            sum = new HashSet<Integer>();
            num = new HashSet<Integer>();
        }
        // Add the number to an internal data structure.
    	public void add(int number) {
    	    if(num.contains(number)){
    	        sum.add(number * 2);
    	    }else{
    	        Iterator<Integer> iter = num.iterator();
    	        while(iter.hasNext()){
    	            sum.add(iter.next() + number);
    	        }
    	        num.add(number);
    	    }
    	}
    
        // Find if there exists any pair of numbers which sum is equal to the value.
    	public boolean find(int value) {
    	    return sum.contains(value);
    	}
    }
```



4. A list with distinct elements

```java
class TwoSum {
    Map<Integer, Integer> map;
    List<Integer> list;
    int min;
    int max;

    /** Initialize your data structure here. */
    public TwoSum() {
        map = new HashMap<>();
        list = new ArrayList<>();
        int min = Integer.MAX_VALUE;
        int max = Integer.MIN_VALUE;
    }
    
    /** Add the number to an internal data structure.. */
    public void add(int number) {
        map.put(number, map.getOrDefault(number, 0)+1);
        list.add(number);
        min= Math.min(min, number);
        max = Math.max(max, number);
    }
    
    /** Find if there exists any pair of numbers which sum is equal to the value. */
    public boolean find(int value) {
     if(value > 2*max || value < 2* min) return false;
        for(int i = 0; i < list.size()-1; i++){
            int num1 = list.get(i);
            int num2 = value -num1;
            if(num1 == num2 && map.get(num1) >=2) return true;
            else if(num1 != num2 && map.containsKey(num2)) return true;
        }
        return false;
    }
}
```



##  Two Sum IV - Input is a BST

Given a Binary Search Tree and a target number, return true if there exist two elements in the BST such that their sum is equal to the given target.

**Example 1:**

```
Input: 
    5
   / \
  3   6
 / \   \
2   4   7

Target = 9

Output: True
```



#### Analysis

1. In-Order Traversal

   In-Order Traversal and and then we have a sorted list. We can do as the previous problems.



​	Time Complexity: O(N), Where N is the number of nodes in the BST.

​	Space Complexity: O(N), Where N is the number of nodes in the BST.

2. Using a Hash Set

#### Solution

1. In-Order Traversal 

```java
class Solution {
    public boolean findTarget(TreeNode root, int k) {
        List<Integer> list = new ArrayList<>();
        inorder(root, list);
        int l = 0, r = list.size() - 1;
        while (l < r) {
            int sum = list.get(l) + list.get(r);
            if (sum == k)
                return true;
            if (sum < k)
                l++;
            else
                r--;
        }
        return false;        
        
    }
    
    public void inorder(TreeNode root, List<Integer> list) {
        if(root==null) return;
        inorder(root.left, list);
        list.add(root.val);
        inorder(root.right, list);
    }
}
```



## Two Sum BSTs

#### Analysis



#### Solution



## 3Sum

Given an array `nums` of *n* integers, are there elements *a*, *b*, *c* in `nums` such that *a* + *b* + *c* = 0? Find all unique triplets in the array which gives the sum of zero.

**Note:**

The solution set must not contain duplicate triplets.

**Example:**

```
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```



#### Analysis

1. Brute Force

   If you think about it, when we fix one of the three numbers, it became 2 Sum again. 

#### Solution



## 3Sum Smaller

#### Analysis



#### Solution





## 3Sum Closest

#### Analysis



#### Solution





## 3Sum With Multiplicity

#### Analysis



#### Solution





## 4Sum

#### Analysis



#### Solution





## 4Sum II

#### Analysis



#### Solution





## Subarray Sum Equals K

#### Analysis



#### Solution





## Continuous Subarray Sum

#### Analysis



#### Solution





## Subarray Sums Divisible by K

#### Analysis



#### Solution





