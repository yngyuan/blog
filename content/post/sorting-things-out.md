---
title: "Sorting Things Out"
date: 2020-06-10T09:59:54-05:00
lastmod: 2020-06-10T09:59:54-05:00
draft: false
keywords: []
description: ""
tags: ["leetcode","sorting"]
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

In a ever-changing world, things could get pretty messy pretty fast. So it is important for us to be able to sort things out. Now let't take a look of the various sorting algorithms. There are too many of them, so here we discuss the most iconic ones. 

# Elementary Sorts

With but a few exceptions, our sort code refers to the data only through two opera-tions: the method less() that compares items and the method exch() that exchanges them.  Here is a template for sort class.

```java
public class Example{
  
	public static void sort(Comparable[] a)
{ /* See Algorithms 2.1, 2.2, 2.3, 2.4, 2.5, or 2.7. */ }
  
	private static boolean less(Comparable v, Comparable w) { 
    return v.compareTo(w) < 0; 
  }
	
  private static void exch(Comparable[] a, int i, int j){  
    Comparable t = a[i]; a[i] = a[j]; a[j] = t;  
  }
  
	private static void show(Comparable[] a) { 
    // Print the array, on a single line. for (int i = 0; i < a.length; i++)
			StdOut.print(a[i] + " "); StdOut.println();
  }
  
	public static boolean isSorted(Comparable[] a)
		{ // Test whether the array entries are in order.
				for (int i = 1; i < a.length; i++)
					if (less(a[i], a[i-1])) return false;
        return true;
     }
		
  public static void main(String[] args)
			{ // Read strings from standard input, sort them, and print.
					String[] a = In.readStrings(); sort(a);
					assert isSorted(a);
					show(a);
      } 
}
```

Take the simple task of sort an array for example.



**Example Quetion**

Given an array of integers `nums`, sort the array in ascending order.

**Example 1:**

```
Input: nums = [5,2,3,1]
Output: [1,2,3,5]
```

**Example 2:**

```
Input: nums = [5,1,1,2,0,0]
Output: [0,0,1,1,2,5]
```

 

**Constraints:**

- `1 <= nums.length <= 50000`
- `-50000 <= nums[i] <= 50000`

## Selection Sort

First, find the smallest item in the array and exchange it with the first entry (itself if the first entry is already the smallest). Then, find the next smallest item and exchange it with the sec- ond entry. Continue in this way until the entire array is sorted.

- Running time is insensitive to input.
- Data movement is minimal.
- O(N^2)

```java
class Solution {
    public int[] sortArray(int[] nums) {
        int len = nums.length;
        if(len<=1) return nums;
        int[] res = selectionSort(nums);
        return res;
    } 
    
  // exch() and less() as defined earlier
    private int[] selectionSort(int[] nums) {
        for(int i=0; i<nums.length; i++){
            int min = i;
            for(int j=i+1; j<nums.length; j++){
                if(less(nums[j], nums[min])){
                    min = j;
                }
            }
            exch(nums, i, min);
        }
        return nums;
    }
}
```



## Insertion Sort

For each i from 0 to N-1, exchange a[i] with the entries that are smaller in a[0] through a[i-1]. As the index i travels from left to right, the entries to its left are in sorted order in the array, so the array is fully sorted when i reaches the right end.

- Unlike that of selection sort, the running time of insertion sort depends on the ini- tial order of the items in the input. 
- excellent for partially sorted arrays.
- O(N^2)

```java
class Solution {
    public int[] sortArray(int[] nums) {
        int len = nums.length;
        if(len<=1) return nums;
        int[] res = insertionSort(nums);
        return res;
    }
    
    private int[] insertionSort(int[] nums) {
        for(int i=0; i<nums.length; i++){
            for(int j=i; j>0; j--){
                if(less(nums[j], nums[j-1])){
										exch(nums, j, j-1);
                }
            }
        }
        return nums;
    }
}
```



## Shellsort

Shellsort is based on insertion sort. It gains speed by allowing exchanges of array entries that are far apart, to produce partially sorted arrays that can be efficiently sorted, eventually by insertion sort.

One way to implement shellsort would be, for each *h*, to use insertion sort independently on each of the *h* subsequences. Because the subsequences are independent, we can use an even simpler approach: when *h*-sorting the array, we insert each item among the previous items in its *h*-subsequence by exchanging it with those that have larger keys (moving them each one position to the right in the subsequence). 

- much faster than insertion sort and selection sort.

```java
class Solution {
    public int[] sortArray(int[] nums) {
        int len = nums.length;
        if(len<=1) return nums;
        int[] res = shellSort(nums);
        return res;
    }
    
    private int[] shellSort(int[] nums) {
        int N = nums.length;
        int h = 1;
        while(h<N/3) h = 3*h + 1;
        
        while(h>=1){
            // h-sort the array
            for(int i=h; i<N; i++){
                for(int j=i; j>=h && less(nums[j], nums[j-h]); j-=h){
										exch(nums, j, j-h);
                }
            }
            h/=3;
        }
        return nums;
    }
}
```



## Shuffling

 **Shuffle Sort**

- Generate a random real number for each array entry.
- Sort the array.
- Shuffle sort produces a uniformly random permutation of the input array, provided no duplicate values.
- Need to pay cost of sort.



**Knuth Shuffle**

- In iteration i, pick integer r between 0 and i uniformly at random.
- Swap a[i] and a[r].
- Produces a uniformly random permutation of the input array in linear time.



```java
public static void shuffle(Object[] a){
		int N = a.length;
		for(int i=0; i<N; i++){
			int r = StdRandom.uniform(i+1);
			exch(a, i, r);
		}
}

private void shuffle(int[] array){
        Random rand = new Random();
		
		for (int i = 0; i < array.length; i++) {
			int randomIndexToSwap = rand.nextInt(array.length);
			int temp = array[randomIndexToSwap];
			array[randomIndexToSwap] = array[i];
			array[i] = temp;
		}
 }
```



# Mergesort

The algorithms that we consider in this section are based on a simple operation known as *merging*: combining two ordered arrays to make one larger ordered array. This operation immediately leads to a simple recursive sort method known as *mergesort*: to sort an array, divide it into two halves, sort the two halves (recursively), and then merge the results. 

- Java sort for objects
- Divide and Conquer
- O(Nlog(N))
- Not optimal with respect to space usage

### Top-down mergesort

```java
  public class Merge
  {
     private static Comparable[] aux;      // auxiliary array for merges
     public static void sort(Comparable[] a)
     {
				aux = new Comparable[a.length]; // Allocate space just once.
				sort(a, 0, a.length - 1); }

    private static void sort(Comparable[] a, int lo, int hi) 
    { // Sort a[lo..hi].
				if (hi <= lo) return;
				int mid = lo + (hi - lo)/2;
				sort(a, lo, mid); // Sort left half.
				sort(a, mid+1, hi); // Sort right half.
				merge(a, lo, mid, hi); // Merge results.
		}
    
    private static void merge(Comparable[] a, int lo, int mid, int hi) { 
      // Merge a[lo..mid] with a[mid+1..hi].
     int i = lo, j = mid+1;
			for (int k = lo; k <= hi; k++) // Copy a[lo..hi] to aux[lo..hi]. 
        aux[k] = a[k];
			for (int k = lo; k <= hi; k++) // Merge back to a[lo..hi]. 
        if (i > mid) a[k] = aux[j++];
				else if (j > hi ) a[k] = aux[i++];
				else if (less(aux[j], aux[i])) a[k] = aux[j++];
        else                           a[k] = aux[i++];
		}
}
```

- Speed up
  - Use insertion sort for small subarrays. Switching to insertion sort for small subarrays (length 15 or less, say) will improve the running time of a typical mergesort implementation by 10 to 15 percent.
  - Test Whether the array is already in order.  Skip the call to merge() if a[mid] is less than or equal to a[mid+1]. 
  - Eliminate the copy to the auxiliary array. To do so, we use two invocations of the sort method: one takes its input from the given array and puts the sorted output in the auxiliary array; the other takes its input from the auxiliary array and puts the sorted output in the given array. 

### Bottom-up mergesort

```java
public class MergeBU{
  		private static Comparable[] aux;      // auxiliary array for merges

			public static void sort(Comparable[] a) 
      { // Do lg N passes of pairwise merges.
				int N = a.length;
				aux = new Comparable[N];
				for (int sz = 1; sz < N; sz = sz+sz) // sz: subarray size 
        	for (int lo = 0; lo < N-sz; lo += sz+sz) // lo: subarray index
						merge(a, lo, lo+sz-1, Math.min(lo+sz+sz-1, N-1));
			}
}
```



# Quicksort

It works by *partitioning* an array into two subarrays, then sorting the subarrays indepen- dently. Quicksort is complementary to mergesort: for mergesort, we break the array into two subarrays to be sorted and then combine the ordered subarrays to make the whole ordered array; for quicksort, we rearrange the array such that, when the two subarrays are sorted, the whole array is ordered. 

- Java sort for primitive types

- In-place
- O(N(log(N)))

```java
  public class Quick
  {
     public static void sort(Comparable[] a)
     {
				StdRandom.shuffle(a); // Eliminate dependence on input.
				sort(a, 0, a.length - 1); 
     }
    
     private static void sort(Comparable[] a, int lo, int hi)
     {
				if (hi <= lo) return;
				int j = partition(a, lo, hi); // Partition (see page 291). 
       	sort(a, lo, j-1); // Sort left part a[lo .. j-1]. 
       	sort(a, j+1, hi); // Sort right part a[j+1 .. hi].
	} 
    
    private static int partition(Comparable[] a, int lo, int hi) 
    { // Partition into a[lo..i-1], a[i], a[i+1..hi].
				int i = lo, j = hi+1; // left and right scan indices 
      	Comparable v = a[lo]; // partitioning item
				while (true)
				{ // Scan right, scan left, check for scan complete, and exchange.
						while (less(a[++i], v)) if (i == hi) break; 
          	while (less(v, a[--j])) if (j == lo) break; 
          	if (i >= j) break;
						exch(a, i, j);
 				}
				exch(a, lo, j); // Put v = a[j] into position
				return j; // // with a[lo..j-1] <= a[j] <= a[j+1..hi].
   }
}


```



#### Usage - Selection

Selection problems can also be solved with the quick sort approach.

Find the **k**th largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

**Example 1:**

```
Input: [3,2,1,5,6,4] and k = 2
Output: 5
```

**Example 2:**

```
Input: [3,2,3,1,2,4,5,5,6] and k = 4
Output: 4
```

**QuickSelect**

- Choose a random pivot.
- Use a partition algorithm to place the pivot into its perfect position `pos` in the sorted array, move smaller elements to the left of pivot, and larger or equal ones - to the right.
- Compare `pos` and `N - k` to choose the side of array to proceed recursively.
- Here there is no need to deal with both parts since now one knows in which part to search for `N - k`th smallest element, and that reduces average time complexity to O(N)

```java
import java.util.Random;
class Solution {
  int [] nums;

  public void swap(int a, int b) {
    int tmp = this.nums[a];
    this.nums[a] = this.nums[b];
    this.nums[b] = tmp;
  }


  public int partition(int left, int right, int pivot_index) {
    int pivot = this.nums[pivot_index];
    // 1. move pivot to end
    swap(pivot_index, right);
    int store_index = left;

    // 2. move all smaller elements to the left
    for (int i = left; i <= right; i++) {
      if (this.nums[i] < pivot) {
        swap(store_index, i);
        store_index++;
      }
    }

    // 3. move pivot to its final place
    swap(store_index, right);

    return store_index;
  }

  public int quickselect(int left, int right, int k_smallest) {
    /*
    Returns the k-th smallest element of list within left..right.
    */

    if (left == right) // If the list contains only one element,
      return this.nums[left];  // return that element

    // select a random pivot_index
    Random random_num = new Random();
    int pivot_index = left + random_num.nextInt(right - left); 
    
    pivot_index = partition(left, right, pivot_index);

    // the pivot is on (N - k)th smallest position
    if (k_smallest == pivot_index)
      return this.nums[k_smallest];
    // go left side
    else if (k_smallest < pivot_index)
      return quickselect(left, pivot_index - 1, k_smallest);
    // go right side
    return quickselect(pivot_index + 1, right, k_smallest);
  }

  public int findKthLargest(int[] nums, int k) {
    this.nums = nums;
    int size = nums.length;
    // kth largest is (N - k)th smallest
    return quickselect(0, size - 1, size - k);
  }
}
```



#### Usage - duplicate

3-way partitioning. Partition array into 3 parts so that

- Entries between lt and gt equal to partition item v.
- No larger entries to left of lt
- No smaller entries to right of gt.



```
private static void sort(Comparable[] a,  int lo, int hi)
{
	if(hi<=lo) return;
	int. lt =0,  gt = hi;
	Compareable v = a[lo];
	int i=lo;
	while(i<=gt){
		int cmp = a[i].compareTo(v);
		if(cmp<0) exch(a, lt++, i++);
		else if(cmp>0) exch(a, i, gt--);
		else i++;
	}
	sort(a, lo, lt-1);
	sort(a, gt+1, hi);
}
```



# Priority Queue

A Priority Queue supports two operations: *remove the maximum*/minimum and *insert*. we consider a classic priority-queue implementation based on the *binary heap* data structure, where items are kept in an array, subject to certain ordering constraints that allow for efficient (logarithmic-time) implementations of *remove the maximum* and *insert*. 

### Heap

 binary tree is *heap-ordered* if the key in each node is larger than or equal to the keys in that node’s two children (if any).



A *binary heap* is a collection of keys arranged in a complete heap-or- dered binary tree, represented in level order in an array (not using the first entry).

##### Heap Prority Queue

```java
public class MaxPQ<Key extends Comparable<Key>>
  {
			private Key[] pq; // heap-ordered complete binary tree 
			private int N = 0; // in pq[1..N] with pq[0] unused
     	
     	public MaxPQ(int maxN)
     {  pq = (Key[]) new Comparable[maxN+1];  }
     
 			public boolean isEmpty()
			{  return N == 0;  }

			public int size()
			{  return N;  }

			public void insert(Key v)
			{
				pq[++N] = v;
				swim(N); }
			
			public Key delMax()
			{
				Key max = pq[1]; // Retrieve max key from top.
				exch(1, N--);   // Exchange with last item.
				pq[N+1] = null;   // Avoid loitering.
				sink(1);  // Restore heap property.
				return max; }

     	private boolean less(int i, int j)
     	private void exch(int i, int j)
     	
      private void swim(int k)
  		{
     		while (k > 1 && less(k/2, k))
     		{
        exch(k/2, k);
				k = k/2; 
        }
			}
        
        
     	private void sink(int k)
  		{
     		while (2*k <= N)
     	{
        int j = 2*k;
        if (j < N && less(j, j+1)) j++;
        if (!less(k, j)) break;
        exch(k, j);
        k = j;
			} 
    }
        
}
```



### Heap Sort

We insert all the items to be sorted into a minimum-oriented priority queue, then repeatedly use *remove the minimum* to remove them all in order. Heapsort breaks into two phases: *heap construction*, where we reorganize the original array into a heap, and the *sortdown*, where we pull the items out of the heap in decreas- ing order to build the sorted result.

##### Heap Sort

```java
public static void sort(Comparable[] a)
{
		int N = a.length;
		for (int k = N/2; k >= 1; k--)
        sink(a, k, N);
    while (N > 1)
     {
				exch(a, 1, N--);
        sink(a, 1, N);
     }
}
```



Time Complexity: Nlog(N) guarantee

Space: O(1) in place



##### Example Question

Given an array of integers `nums`, sort the array in ascending order.

**Example 1:**

```
Input: nums = [5,2,3,1]
Output: [1,2,3,5]
```

**Example 2:**

```
Input: nums = [5,1,1,2,0,0]
Output: [0,0,1,1,2,5]
```

 

**Constraints:**

- `1 <= nums.length <= 50000`
- `-50000 <= nums[i] <= 50000`

```java
class Solution {
    
    public int[] sortArray(int[] nums) {

        // rearrange the given array to build the heap
        int n=nums.length;
        for(int i=n/2-1;i>=0;--i)
            heapify(nums, n, i);

        // extract the min from heap
        for(int i=n-1;i>=0;--i) {

            // move root to the last
            swap(nums, 0,i);
            heapify(nums, i, 0);
        }

        return nums;
    }


    private void heapify(int[] nums, int n, int i) {
        int parent = i; // root
        int left = 2*i + 1; // left child
        int right = 2*i + 2; // right child

        if(left<n && nums[left]>nums[parent]) {
            parent=left;
        }

        if(right<n && nums[right]>nums[parent]) {
            parent=right;
        }

        if(parent != i) {
            // heapify
            swap(nums, parent, i);
            heapify(nums, n, parent);
        }

    }

    private void swap(int[] nums, int i , int j) {
        int temp = nums[i];
        nums[i]=nums[j];
        nums[j] = temp;
    }
}
```



# Summary



![sorting](/image/sorting.png)

