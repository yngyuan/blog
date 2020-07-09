---
title: "Two Sum and Some More"
date: 2020-06-03T10:38:01-05:00
lastmod: 2020-06-03T10:38:01-05:00
draft: false
keywords: ["two sum", "leetcode"]
description: ""
tags: ["leetcode","data structure"]
categories: ["algorithms"]
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

   - Time Complexity: O(N^2), where N is the length of the array nums.

   - Space Complexity: O(1), the only extra space we used is the return array.

2. Two-pass Hash Map

   Hash Map is always my favorite. Who doesn't like to sacrifice a little bit Space for Time? Loop the array once to make a hash map of the value to its index. Then loop again to find if the value of target-current value is in the map.

   - Time Complexity: O(N), 2 for loops on the array.
   - Space Complexity: O(N), used a hash map.

3. One-pass Hash Map

   Now think again, we don't really need 2 for loops. We could build the map as we go and just might get lucky and find the target sooner.

   - Time Complexity: O(N),  for-loop on the array. Each look up on hash map is only O(1).
   - Space Complexity: O(N), N is at most the length of the array.

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

   - Time Complexity: O(N^2)
   - Space Complexity: O(1)

2. Sort and two-pointer

   We can sort the array first. Then loop the array, use two pointer from both ends to find a value, such that the sum is greater than current max sum and less than K.

   - Time Complexity: The average time complexity for Arrays.sort(int[]) is O(Nlog(N)), worst case O(N^2), where N is the length of the array.
   - Space Complexity: O(1)



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

   - Time Complexity: O(N) for worst case. 
   - Space Complexity: O(1)



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

   - Time Complexity:

   ​		Add: O(1)

   ​		Find: O(Nlog(N))

   - Space Complexity: O(N)

2. Hash Map

   Use a hash map to store the number and its count. This count could come to use for case lie 8 + 8 = 16. 

   - Time Complexity: Add: O(1), Find: O(N)
   - Space Complexity: O(N)

3. Consider more add than find, use two Hash Set. Store all the possible sums in sum set. Store all the number in num set.

   - Time Complexity: (TLE): Add: O(N),  Find: O(1)
   - Space Complexity: O(N^2) for worst case.

   

4. Maintain a list with distinct elements

   It is much faster to iterate a list than the key set of a map. Use a hash map to store the counts. Also, keep a min and max value of the numbers can help when there are some corner cases.

   - Time Complexity:  Add: O(1), Find: O(N)
   - Space Complexity: O(N)				

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

   In-Order Traverse the tree and and then we have a sorted list. We can do as the previous problems.

   - Time Complexity: O(N), Where N is the number of nodes in the BST.
   - Space Complexity: O(N), Where N is the number of nodes in the BST.

2. Using a Hash Set

   Traverse the tree and maintain a Set of all the encountered numbers. For every value p, check if k-p exeist in the set already.

   - Time Complexity: O(N), worst case we iterate all nodes in the tree.
   - Space Complexity: O(N), worst case we store all node value in Set.



3. BFS and Hash Set

   We can traverse the tree in a BFS manner and also maintain a Hash Set like previous one. We do the traverse iteritatively using a queue.

   1. Remove an element, p, from the front of the queue.
   2. Check if the element k-p already exists in the set. If so, return True.
   3. Otherwise, add this element, p to the set. Further, add the right and the left child nodes of the current node to the back of the queue.
   4. Continue steps 1. to 3. till the queue becomes empty.
   5. Return false if the queue becomes empty.

Time and Space Complexity is the same as the previous one.



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



2. Using Hash Set

```java
class Solution {
    public boolean findTarget(TreeNode root, int k){
            Set<Integer> set = new HashSet<>();
            return find(root, k, set);  
    }
    
    public boolean find(TreeNode root, int k, Set<Integer> set){
        if(root==null) return false;
        if(set.contains(k-root.val)){
            return true;
        }
        set.add(root.val);
        return find(root.left, k, set) || find(root.right, k, set);
    }

}
```



3. BFS and HashSet

```java
class Solution {
    public boolean findTarget(TreeNode root, int k){
        Set<Integer> set = new HashSet<>();
        Queue<TreeNode> queue = new LinkedList();
        queue.add(root);
        while(!queue.isEmpty()) {
            if(queue.peek() != null){
                TreeNode node = queue.remove();
                if(set.contains(k-node.val)){
                    return true;
                }
                set.add(node.val);
                queue.add(node.left);
                queue.add(node.right);
            } else{
                queue.remove();
            }
        }
        return false;
    }
}
```



## Two Sum BSTs

Given two binary search trees, return `True` if and only if there is a node in the first tree and a node in the second tree whose values sum up to a given integer `target`.

```
Input: root1 = [2,1,4], root2 = [1,0,3], target = 5
Output: true
Explanation: 2 and 3 sum up to 5.
```



#### Analysis

1. Recursive In-Order Traversal and a Set

   Looking familiar? That's right. Just in-order traverse the tree and we have two sorted list. Then we use a set to track all the values in one tree, check if the set contains target-value in another tree.

   - Time Complexity: O(N+M),worst case is the total number of nodes across two trees
   - Space Complexity: O(N+M+N+M+N), two list and a set and recursive stacks.

2. Recursive In-Order Traversal but without that unnecessary list

   If you think about it, we don't really need two lists. We can build the Hash Set when doing traversal. 

   - Time Complexity: O(N+M)
   - Space Complexity: O(N+M+N)

3. Iterative In-Order Traversal.

   The drawback of the recursive approach is that one has to traverse the entire second tree, even if it's not really needed.

   - Do iterative inorder traversal of the first tree to build hashset of complements (target - val).
   - Do iterative inorder traversal of the second tree to check if at least one element of the second tree is in hashset. Stop the traversal and return True once you find such an element.

   Time complexity: O(N+M) ,N, Mare the numbers of nodes in the first and the second tree respectively.

   Space complexity: O(N+MAX(N,M)) , N to keep the hashset and up to  MAX(N,M) for the stack.

#### Solution

1. In-Order Traversal and a Set

```java
class Solution {
    public boolean twoSumBSTs(TreeNode root1, TreeNode root2, int target) {
        List<Integer> list1 = new ArrayList<>();
        List<Integer> list2 = new ArrayList<>();
        traverse(root1, list1);
        traverse(root2, list2);
        Set<Integer> set = new HashSet<>();
        for(int val: list1){
            set.add(val);
        }
        for(int val: list2){
            if(set.contains(target-val)){
                return true;
            }
        }
        return false;
    }
    
    public void traverse(TreeNode root, List<Integer> list){
        if(root==null) return;
        traverse(root.left, list);
        list.add(root.val);
        traverse(root.right, list);
    }
}
```



2.  In-Order Traversal but without that unnecessary list

```java
class Solution {
  public Set<Integer> inHashset(TreeNode r, int target, Set<Integer> s) {
    if (r == null) return s;
    inHashset(r.left, target, s);
    s.add(target - r.val);
    inHashset(r.right, target, s);
    return s;
  }

  public boolean inCheck(TreeNode r, Set<Integer> s) {
    if (r == null) return false;
    return inCheck(r.left, s) || s.contains(r.val) || inCheck(r.right, s);
  }

  public boolean twoSumBSTs(TreeNode root1, TreeNode root2, int target) {
    Set<Integer> s = inHashset(root1, target, new HashSet());
    return inCheck(root2, s);
  }
}
```



3. Iterative In-Order Traversal

```java
class Solution {
  public boolean twoSumBSTs(TreeNode root1, TreeNode root2, int target) {
    ArrayDeque<TreeNode> stack = new ArrayDeque();
    Set<Integer> s = new HashSet();
    // traverse the first tree 
    // and store node complements (target - val) in hashset
    while (!stack.isEmpty() || root1 != null) {
      while (root1 != null) {
        stack.push(root1);
        root1 = root1.left;
      }
      root1 = stack.pop();
      s.add(target - root1.val);
      root1 = root1.right;
    }

    // traverse the second tree 
    // and check if one of the values exists in hashset
    while (!stack.isEmpty() || root2 != null) {
      while (root2 != null) {
        stack.push(root2);
        root2 = root2.left;
      }
      root2 = stack.pop();
      if (s.contains(root2.val)) {
        return true;
      }
      root2 = root2.right;
    }
    
    return false;
  }
}
```

We used ArrayDeuqe implementation of stack, here are some notes

- Use an ArrayList if you need to access elements by index and you only need to insert/delete at the end.
- Use an ArrayDeque as a stack, queue, or deque.
- Use a LinkedList if you need to insert/delete while iterating through the list, or if you need insertion at the ends to be worst-case O(1).

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

1. Sort and Two Pointers

   If you think about it, when we fix one of the three numbers, it became 2 Sum again. The tricky part is actually how to avoid duplicates. To make sure the result contains unique triplets, we need to skip duplicate values. It is easy to do because repeating values are next to each other in a sorted array.

   - Time Complexity: O(N^2)) for worst case. Sorting take O(Nlog(N).
   - Space Complexity: O(N)

2. HashSet

   Handling duplicates here is trickier compared to the two pointers approach. We can put a combination of three values into a hash set to efficiently check whether we've found that triplet already. Values in a combination should be ordered (e.g. ascending). Otherwise, we can have results with the same values in the different positions.

   - Time Complexity: O(N^2)
   - Space Complexity: O(N^2), we may have to store N^2 elements for deduplication

#### Solution

1. Sort and Two Pointers

```java
class Solution{
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();
        for (int i = 0; i < nums.length && nums[i] <= 0; ++i)
            if (i == 0 || nums[i - 1] != nums[i]) // skip duplicate
                twoSumII(nums, i, res);
        return res;
    }
    public void twoSumII(int[] nums, int i, List<List<Integer>> res) {
        int lo = i + 1, hi = nums.length - 1;
        while (lo < hi) {
            int sum = nums[i] + nums[lo] + nums[hi];
         		if (sum < 0 || (lo > i + 1 && nums[lo] == nums[lo - 1]))
                lo+=1;
            else if (sum > 0 || (hi < nums.length - 1 && nums[hi] == nums[hi + 1]))
                hi-=1;
            else
                res.add(Arrays.asList(nums[i], nums[lo++], nums[hi--]));
        }
    }
}
```



2. HashSet

```java
class Solution{
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        Set<Pair> found = new HashSet<>();
        Set<Integer> dups = new HashSet<>();
        Map<Integer, Integer> seen = new HashMap<>();
        for (int i = 0; i < nums.length; ++i){
            if (dups.add(nums[i])) { // true if this set did not already contain the specified element
                    for (int j = i + 1; j < nums.length; ++j) {
                        int complement = -nums[i] - nums[j];
                        if (seen.containsKey(complement) && seen.get(complement) == i) {
                            int v1 = Math.min(nums[i], Math.min(complement, nums[j]));
                            int v2 = Math.max(nums[i], Math.max(complement, nums[j]));
                            if (found.add(new Pair(v1, v2)))
                                res.add(Arrays.asList(nums[i], complement, nums[j]));
                        }
                        seen.put(nums[j], i);
                    } 
            }
        }
        return res;
    }
}
```



## 3Sum Smaller

Given an array of *n* integers *nums* and a *target*, find the number of index triplets `i, j, k` with `0 <= i < j < k < n` that satisfy the condition `nums[i] + nums[j] + nums[k] < target`.

**Example:**

```
Input: nums = [-2,0,1,3], and target = 2
Output: 2 
Explanation: Because there are two triplets which sums are less than 2:
             [-2,0,1]
             [-2,0,3]
```

**Follow up:** Could you solve it in *O*(*n*2) runtime?



#### Analysis

1. Brute Force

  Surely we can find all possible combinations of the triplets. However that would take Time Complexity of O(N^3). And we do not want that.

2. Sort and Two Pointer

   First sort the array, and iterate the array. For each number, we can find target minus the value for a 2 SUM Smaller problem. Use Two Pointer Approach to do it.

   - Time Complexity: O(N^2) for worst case.
   - Space Complexity: O(1)

3. Binary Search

   It shoule be an instinct to think about binary search whenever a sorted array is present. We take a similar approach as last one, for each number in the sorted array, we use binary search to find the largest index j such that nums[i]+nums[j]<target.

   - Time complexity: O(N^2log(N)), binary search takes O(log(N)), Then we do it N times for 2 SumSmaller problem, then again do it in a for loop for 3SumSmaller.
   - Space Complexity: O(1)

#### Solution

2. Sort and Two Pointer

```java
class Solution {
public int threeSumSmaller(int[] nums, int target) {
    Arrays.sort(nums); 
    int sum = 0;
    for (int i = 0; i < nums.length - 2; i++) {
        sum += twoSumSmaller(nums, i + 1, target - nums[i]);
    }
    return sum;
}

private int twoSumSmaller(int[] nums, int startIndex, int target) {
    int sum = 0;
    int lo = startIndex; // start at i+1, so we don't duplicate.
    int hi = nums.length - 1;
    while (lo < hi) {
        if (nums[lo] + nums[hi] < target) {
            sum += hi - lo; // if nums[lo]+nums[hi]<target, same applis to num[lo]+num[hi-1] because it is sorted.
            left++;
        } else {
            right--;
        }
    }
    return sum;
}
}
```



3. Binary Search

```java
class Solution {
    
    public int threeSumSmaller(int[] nums, int target) {
        Arrays.sort(nums);
        int sum = 0;
        for (int i = 0; i <nums.length -2; i++){ // i up to 3 to last
            sum += twoSumSmaller(nums, i+1, target-nums[i]); 
        }
        return sum;
    }
    
    private int twoSumSmaller(int[] nums, int startIndex, int target) {
        int sum = 0;
        for(int i= startIndex; i<nums.length-1; i++){ //i up to second to last
            int j = binarySearch(nums, i, target-nums[i]);
            sum += j-i;
        }
        return sum;
    }
    
    private int binarySearch(int[] nums, int startIndex, int target){
        int lo = startIndex;
        int hi = nums.length-1;
        while(lo<hi){
            int mid = (lo + hi + 1)/2; //upper middle element for terminating condition
            if(nums[mid] < target){
                lo = mid;
            } else{
                hi = mid-1;
            }
        }
        return lo;
    }
}
```



## 3Sum Closest

Given an array `nums` of *n* integers and an integer `target`, find three integers in `nums` such that the sum is closest to `target`. Return the sum of the three integers. You may assume that each input would have exactly one solution.

 **Example 1:**

```
Input: nums = [-1,2,1,-4], target = 1
Output: 2
Explanation: The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

**Constraints:**

- `3 <= nums.length <= 10^3`
- `-10^3 <= nums[i] <= 10^3`
- `-10^4 <= target <= 10^4`



#### Analysis

1. Sort and Two-Pointer

   After previous questions, we may have found a pattern. First Sort the array. Then for each value in the array do a 2Sum Closest on the target-value using Two-Pointer approach. We will maintain a min diff along the way.

   - Time Complexity: O(N^2+O(Nlog(N))), which equivalent to O(N^2) 
   - Space Complexity: O(log(N) or N) depending on the implementation of sorting algorithm.

2. Binary Search

   Yet again, we use binary search on a sorted array. It should be second nature. In the two pointers approach, we fix one number and use two pointers to enumerate pairs. Here, we fix two numbers, and use a binary search to find the third complement number. This is less efficient than the two pointers approach, however, it could be more intuitive to come up with.

   - Time Complexity: O(N^2log(N))
   - Space Complexity: O(log(N) or N)

#### Solution

1. Sort and Two-Pointer

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        int diff = Integer.MAX_VALUE, len = nums.length;
        Arrays.sort(nums);
        for (int i = 0; i < len && diff != 0; ++i) {
            int lo = i + 1, hi = len - 1;
            while (lo < hi) {
                int sum = nums[i] + nums[lo] + nums[hi];
                if (Math.abs(target - sum) < Math.abs(diff))
                    diff = target - sum;
                if (sum < target) // don't worry about signs, just the relative value
                    lo+=1;
                else
                    hi-=1;
            }
        }
        return target - diff;
    }
}
```



## 3Sum With Multiplicity

Given an integer array `A`, and an integer `target`, return the number of tuples `i, j, k` such that `i < j < k` and `A[i] + A[j] + A[k] == target`.

**As the answer can be very large, return it modulo `10^9 + 7`**.

**Example 1:**

```
Input: A = [1,1,2,2,3,3,4,4,5,5], target = 8
Output: 20
Explanation: 
Enumerating by the values (A[i], A[j], A[k]):
(1, 2, 5) occurs 8 times;
(1, 3, 4) occurs 8 times;
(2, 2, 4) occurs 2 times;
(2, 3, 3) occurs 2 times.
```

  

#### Analysis

1. HashMap

   Intuitively, you will try to solve it based on the solution of 3Sum. We only need a count of all the suitable  combinations of the triplet. Build a map for counting different sums of two numbers. The rest of things are straightfoward.

   - Time Complexity: O(N^2)
   - Space Complexity: O(N)

#### Solution

1. HashMap

```java
class Solution {
    public int threeSumMulti(int[] A, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        
        int res = 0;
        int mod = 1000000007;
        for (int i = 0; i < A.length; i++) {
            res = (res + map.getOrDefault(target - A[i], 0)) % mod;
            
            for (int j = 0; j < i; j++) {
                int temp = A[i] + A[j];
                map.put(temp, map.getOrDefault(temp, 0) + 1);
            }
        }
        return res;
    }
}
```



## 4Sum

Given an array `nums` of *n* integers and an integer `target`, are there elements *a*, *b*, *c*, and *d* in `nums` such that *a* + *b* + *c* + *d* = `target`? Find all unique quadruplets in the array which gives the sum of `target`.

**Note:**

The solution set must not contain duplicate quadruplets.

**Example:**

```
Given array nums = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```



#### Analysis

1. from 3Sum

   We can adapt the same approach as 3Sum.

   - Time Complexity: O(N^3)
   - Space Complexity: O(1)

2. KSum general Two Pointer

   Again? You may wonder. However, from now on we will think of this problem in a more general way. We will think this as a case of KSum and come up with more general solution. 

   - Time Complexity: O(N^(k-1)), 
   - Space Complexity: Worst case could be O(N)

3. HashSet

   

   - Time Complexity: O(N^(k-1)), 
   - Space Complexity: Worst case could be O(N)

#### Solution

1. 3Sum

```java
public class Solution {
public List<List<Integer>> fourSum(int[] nums, int target) {
    List<List<Integer>> list = new ArrayList<>();
    if(nums == null || nums.length < 4) return list;
    Arrays.sort(nums);
    int len = nums.length;
    
    // anchor the first element
    for(int i=0; i<=len-4; i++) {
        if(i != 0 && nums[i] == nums[i-1]) continue;                                // avoid duplicate
        if(nums[i] + nums[i+1] + nums[i+2] + nums[i+3] > target) break;             // speed up
        if(nums[i] + nums[len-3] + nums[len-2] + nums[len-1] < target) continue;    // speed up
        
        // 3-sum
        for(int j=i+1; j<=len-3; j++) {
            if(j != i+1 && nums[j] == nums[j-1]) continue;                          // avoid duplicate
            if(nums[i] + nums[j] + nums[i+1] + nums[j+2] > target) break;           // speed up
            if(nums[i] + nums[j] + nums[len-2] + nums[len-1] < target) continue;    // speed up
            
            // two pointer
            int left = j+1, right = len-1, tmpTarget = target - nums[i] - nums[j];
            while(left < right) {
                if(nums[left] + nums[right] == tmpTarget) {
                    list.add(Arrays.asList(nums[i], nums[j], nums[left], nums[right]));
                    while(left < right && nums[left] == nums[left+1]) left++;       // avoid duplicate
                    while(left < right && nums[right] == nums[right-1]) right--;    // avoid duplicate
                    left++;
                    right--;
                } else if(nums[left] + nums[right] < tmpTarget) {
                    left++;
                } else {
                    right--;
                }
            }
        }
    }
    return list;
}
}
```



2. General Two Pointer

```java
public List<List<Integer>> fourSum(int[] nums, int target) {
    Arrays.sort(nums);
    return kSum(nums, target, 0, 4);
}
public List<List<Integer>> kSum(int[] nums, int target, int start, int k) {
    List<List<Integer>> res = new ArrayList<>();
    if (start == nums.length || nums[start] * k > target || target > nums[nums.length - 1] * k)
        return res;
    if (k == 2)
        return twoSum(nums, target, start);
    for (int i = start; i < nums.length; ++i)
        if (i == start || nums[i - 1] != nums[i])
            for (var set : kSum(nums, target - nums[i], i + 1, k - 1)) {
                res.add(new ArrayList<>(Arrays.asList(nums[i])));
                res.get(res.size() - 1).addAll(set);
            }
    return res;
}
public List<List<Integer>> twoSum(int[] nums, int target, int start) {
    List<List<Integer>> res = new ArrayList<>();
    int lo = start, hi = nums.length - 1;
    while (lo < hi) {
        int sum = nums[lo] + nums[hi];
        if (sum < target || (lo > start && nums[lo] == nums[lo - 1]))
            ++lo;
        else if (sum > target || (hi < nums.length - 1 && nums[hi] == nums[hi + 1]))
            --hi;
        else
            res.add(Arrays.asList(nums[lo++], nums[hi--]));
    }
    return res;
}
```



3. Hash Set

```java
public List<List<Integer>> fourSum(int[] nums, int target) {
    Arrays.sort(nums);
    return kSum(nums, target, 0, 4);
}
public List<List<Integer>> kSum(int[] nums, int target, int start, int k) {
    List<List<Integer>> res = new ArrayList<>();
    if (start == nums.length || nums[start] * k > target || target > nums[nums.length - 1] * k)
        return res;
    if (k == 2)
        return twoSum(nums, target, start);
    for (int i = start; i < nums.length; ++i)
        if (i == start || nums[i - 1] != nums[i])
            for (var set : kSum(nums, target - nums[i], i + 1, k - 1)) {
                res.add(new ArrayList<>(Arrays.asList(nums[i])));
                res.get(res.size() - 1).addAll(set);
            }
    return res;
}
public List<List<Integer>> twoSum(int[] nums, int target, int start) {
    List<List<Integer>> res = new ArrayList<>();
    Set<Integer> s = new HashSet<>();
    for (int i = start; i < nums.length; ++i) {
        if (res.isEmpty() || res.get(res.size() - 1).get(1) != nums[i])
            if (s.contains(target - nums[i]))
                res.add(Arrays.asList(target - nums[i], nums[i]));
        s.add(nums[i]);
    }
    return res;
}
```



## 4Sum II

Given four lists A, B, C, D of integer values, compute how many tuples `(i, j, k, l)` there are such that `A[i] + B[j] + C[k] + D[l]` is zero.

To make problem a bit easier, all A, B, C, D have same length of N where 0 ≤ N ≤ 500. All integers are in the range of -228 to 228 - 1 and the result is guaranteed to be at most 231 - 1.

**Example:**

```
Input:
A = [ 1, 2]
B = [-2,-1]
C = [-1, 2]
D = [ 0, 2]

Output:
2

Explanation:
The two tuples are:
1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0
```



#### Analysis

1. Hash Map

   After all the above, it is easy for us to fall into the trap of trying 4 layer of for-loops on this problem, or even doing 4 separate loops on 4 arrays. However, 2 of the 2 layer for-loop will do the job. Use Hash Map to store all the possible sum of 2 arrays to the times they occur.

   - Time Complexity: O(N^2)

   - Space Complexity: O(N)

#### Solution

1. Hash Map

```java
class Solution {
    public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
        Map<Integer, Integer> map = new HashMap<Integer, Integer>(); 
        for(int a : A)
            for(int b : B){
                int s = a+b;
                map.put( s, map.getOrDefault(s, 0)+1 ); // all possible sum and occur times
            }
        
        int result=0;
        for(int c : C)
            for(int d : D){
                int s = c+d;
                result += map.getOrDefault(-s, 0); 
            }
        return result; 
    }
}
```



## Coda

Seems like we have finished the Two Sum family. There are a lot of them and we have done them all. Bravo! 







