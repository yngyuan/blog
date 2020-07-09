---
title: "Just a Little Bit"
date: 2020-07-01T23:19:55-05:00
lastmod: 2020-07-01T23:19:55-05:00
draft: false
keywords: ["bitwise operation", "leetcode"]
description: ""
tags: ["bitwise", "java"]
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

# bitwise Operation

The Java programming language provides operators that perform bitwise and bit shift operations on integral types. 

- The unary bitwise complement operator "`~`" inverts a bit pattern; it can be applied to any of the integral types, making every "0" a "1" and every "1" a "0". For example, a `byte` contains 8 bits; applying this operator to a value whose bit pattern is "00000000" would change its pattern to "11111111".

- The signed left shift operator "`<<`" shifts a bit pattern to the left, and the signed right shift operator "`>>`" shifts a bit pattern to the right. The bit pattern is given by the left-hand operand, and the number of positions to shift by the right-hand operand. 
- The unsigned right shift operator "`>>>`" shifts a zero into the leftmost position, while the leftmost position after `">>"` depends on sign extension.

- The bitwise `&` operator performs a bitwise AND operation.

- The bitwise `^` operator performs a bitwise exclusive OR operation.

- The bitwise `|` operator performs a bitwise inclusive OR operation.

## Example

Assume we have two Integers A and B. 

- `A = 60`, which is 0011 1100 in binary, we mark a = 0011 1100.

- `B = 13`, which is 0000 1101 in binary, we mark b = 0000 1101.

Now 

- `A&B` will give 12, which is 0000 1100
- `A|B` will give 61, which is 0011 1101

- `A^B` will give 49, which is 0011 0001

- `~A`  will give -61 which is 1100 0011 due to a signed binary number.

- `A<<2` will give 240 which is 1111 0000.

- `A>>2` will give 15 which is 1111

- `A>>>2` will give 15 which is 0000 1111 (unsigned right shift operator ">>>" doesn't preserve sign of original number and fills the new place with zero)

  - ```
    -2
    Before shift : 1111111111111111111111111111111(0)
    -1
    After shift : (1)1111111111111111111111111111111
    ```

  - ```
     -117769990 
    Before unsigned right shift : 1111 1000 1111 1010 1111 1000 1111 101(0)
    2088598653
    After unsigned right shift :  (0)111 1100 0111 1101 0111 1100 0111 1101 
    ```

  

## Solving Problems

#### 136 Single Number

Given a **non-empty** array of integers, every element appears *twice* except for one. Find that single one.

**Note:**

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

**Example 1:**

```
Input: [2,2,1]
Output: 1
```



##### Approach 1 Sum

```java
class Solution {
    private int sum(int[] arr){
        int res = 0;
        for(int a: arr){
            res += a;
        }
        return res;
    }
    
    public int singleNumber(int[] nums) {
        HashSet<Integer> set = new HashSet<>();
        int setsum = 0;
        for(int n: nums){
            if(!set.contains(n)){
                set.add(n);
                setsum+=n;
            }
        }
        return 2*setsum-sum(nums);
    }
}
```



##### Approach 2 XOR, so fast so easy

```java
class Solution {
    public int singleNumber(int[] nums) {
        int n = nums[0];
        for(int i=1; i<nums.length; i++){
            n ^= nums[i];
        }
        return n;
    }
}
```



#### 1342 Number of Steps to Reduce a Number to Zero

Given a non-negative integer `num`, return the number of steps to reduce it to zero. If the current number is even, you have to divide it by 2, otherwise, you have to subtract 1 from it.

**Example 1:**

```
Input: num = 14
Output: 6
Explanation: 
Step 1) 14 is even; divide by 2 and obtain 7. 
Step 2) 7 is odd; subtract 1 and obtain 6.
Step 3) 6 is even; divide by 2 and obtain 3. 
Step 4) 3 is odd; subtract 1 and obtain 2. 
Step 5) 2 is even; divide by 2 and obtain 1. 
Step 6) 1 is odd; subtract 1 and obtain 0.
```

##### Approach 1 Counting bits

If you think about it, divide a number `num`by 2 is doing `num>>1` and substract 1 when the number is odd is making the last bit from 1 to 0.

 14 in binary: 1110

divide by 2

7 in binary: 0111

substract 1

6 in binary: 0110

**Solution**

To count the bits we'll convert our number into a binary string, for each character if it's a `"1"` we'll add two steps, else if it's `"0"` we'll add one step. In the end we need to subtract 1, because the last bit was over-counted.

```java
class Solution {
    public int numberOfSteps (int num) {
        int cnt = 0;
        String binaryStr = Integer.toBinaryString(num);
        if(num==0) return cnt;
        for(char bit: binaryStr.toCharArray()){
            if(bit=='0') cnt +=1;
            else cnt+=2;
        }
        return cnt-1;
    }
}
```

##### Approach 2 Counting bits with bitwise Operators

Strings are considerably larger than the integer they represent though. Another way of inspecting bits, to check if they're `1` or `0`, is to use the bitwise-and (`&`) operator.

So, to actually inspect a specific bit, we can use a number that has a `1` followed by enough `0`s to put the `1` at the position we want it (we commonly call this a "bitmask"). With this number, we bitwise-and (`&`) it with the input number. If the input number has a `1` at the same position, it'll output `1` at that position, and because all other numbers are `0` they will be `0` in the output as well.

```java
public int numberOfSteps(int num) {
    // We need to handle this as a special case, otherwise it'll return -1.
    if (num == 0) return 0;
    int steps = 0;
    for (int powerOfTwo = 1; powerOfTwo <= num; powerOfTwo = powerOfTwo * 2) {
        // Apply the bit mask to check if the bit at "powerOfTwo" is a 1.
        if ((powerOfTwo & num) != 0) {
            steps = steps + 2;
        } else {
            steps = steps + 1;
        }
    }
    // We need to subtract 1, because the last bit was over-counted.
    return steps - 1;
}
```

#### 231 Power of Two

Given an integer, write a function to determine if it is a power of two.

**Example 1:**

```
Input: 16
Output: true
Explanation: 2^4 = 16
```

##### Approach 1 bit mask

```java
    public boolean isPowerOfTwo(int n) {
        if(n<=0) return false;
        for(int i=0; i<32; i++){
            if((1<<i & n)==n){
                return true;
            }
        }
        return false;
    }
```



#### 242 Power of Four


Given an integer (signed 32 bits), write a function to check whether it is a power of 4.

**Example 1:**

```
Input: 16
Output: true
```



##### Approach 1

```java
  public boolean isPowerOfFour(int n) {
    if (n == 0) return false;
    while (n % 4 == 0) n /= 4;
    return n == 1;
  }
```

##### Approach 2 bit mask

```java
public boolean isPowerOfFour(int num) {
        if (num <= 0) return false;
        for (int i = 0; i < 32; i += 2) {
            if (((1 << i) & num) == num) return true;
        }
        return false;
    }
```



#### 190 Reverse bits

Reverse bits of a given 32 bits unsigned integer.

**Example 1:**

```
Input: 00000010100101000001111010011100
Output: 00111001011110000010100101000000
Explanation: The input binary string 00000010100101000001111010011100 represents the unsigned integer 43261596, so return 964176192 which its binary representation is 00111001011110000010100101000000.
```

##### Approach 1 Using bit mask

Use 0000001, 0000010...... as a bit mask. Do mask&n to get if the current bit is 1 or 0. Then append to a stringbuilder and use Integer.parseUnsignedInt().

##### Solution

```java
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        StringBuilder sb = new StringBuilder();
        int mask = 1;
        for(int i=0; i<32; i++){
            if((mask&n)!=0){
                sb.append(1);
            } else{
                sb.append(0);
            }
            mask = mask<<1;
        }

        return Integer.parseUnsignedInt(sb.toString(),2); 
    }
}
```

##### Follow up

If you think about it, is the StringBuilder necessary? It is slow and all...

##### Approach 2 Bit by Bit

> The key idea is that for a bit that is situated at the index `i`, after the reversion, its position should be `31-i` (note: the index starts from zero).

```java
public int reverseBits(int n) {
    int result = 0;
    for(int i=0; i<32; i++){
        result <<= 1;
        result += n&1;
        n >>= 1;
    }
    return result;
}
```

##### Approach 3 Optimize

We can divide an int into 4 bytes, and reverse each byte then combine into an int. For each byte, we can use cache to improve performance.

```java
public int reverseBits(int n) {
    byte[] bytes = new byte[4];
    for (int i = 0; i < 4; i++) // convert int into 4 bytes
        bytes[i] = (byte)((n >>> 8*i) & 0xFF);
    int result = 0;
    for (int i = 0; i < 4; i++) {
        result += reverseByte(bytes[i]); // reverse per byte
        if (i < 3)
            result <<= 8;
    }
    return result;
}

private int reverseByte(byte b) {
    Integer value = cache.get(b); // first look up from cache
    if (value != null)
        return value;
    value = 0;
    // reverse by bit
    for (int i = 0; i < 8; i++) {
        value += ((b >>> i) & 1);
        if (i < 7)
            value <<= 1;
    }
    cache.put(b, value);
    return value;
}
```

##### Built-in

```java
public int reverseBits(int n) {
        return Integer.reverse(n);
    }
```



#### 389 Find the difference

Given two strings ***s\*** and ***t\*** which consist of only lowercase letters.

String ***t\*** is generated by random shuffling string ***s\*** and then add one more letter at a random position.

Find the letter that was added in ***t\***.

**Example:**

```java
Input:
s = "abcd"
t = "abcde"

Output:
e

Explanation:
'e' is the letter that was added.
```



##### Approach 1 hashmap

```java
class Solution {
    public char findTheDifference(String s, String t) {
        int[] cnt = new int[26];
        for(char c: s.toCharArray()){
            cnt[c-'a']++;
        }
        
        for(char c: t.toCharArray()){
            if(cnt[c-'a']==0){
                return c;
            }
            else{
                cnt[c-'a']--;
            }
        }
        return '0';
    }
}
```



##### Approach 2 Bit Manipulation

The trick is simple. To use bitwise `XOR` operation on all the elements. `XOR` would help to eliminate the alike and only leave the odd duckling.

**Algorithm**

1. Initialize a variable `ch` which would hold the `XOR`ed results.
2. `XOR` all the characters with `ch` while iterating through string `s`.
3. `XOR` all the characters with `ch` while iterating through string `t`. (Alternatively, we could have also combined steps `2` and `3`).
4. Return `ch` as the answer.



```java
class Solution {
	public char findTheDifference(String s, String t) {

		// Initialize ch with 0, because 0 ^ X = X
		// 0 when XORed with any bit would not change the bits value.
		char ch = 0;

		// XOR all the characters of both s and t.
		for (int i = 0; i < s.length(); i += 1) {
			ch ^= s.charAt(i);
		}
		for (int i = 0; i < t.length(); i += 1) {
			ch ^= t.charAt(i);
		}

		// What is left after XORing everything is the difference.
		return ch;
	}
}
```



#### 1356 Sort Integers by The Number of 1 Bits

Given an integer array `arr`. You have to sort the integers in the array in ascending order by the number of **1's** in their binary representation and in case of two or more integers have the same number of **1's** you have to sort them in ascending order.

Return *the sorted array*.

**Example 1:**

```
Input: arr = [0,1,2,3,4,5,6,7,8]
Output: [0,1,2,4,8,3,5,6,7]
Explantion: [0] is the only integer with 0 bits.
[1,2,4,8] all have 1 bit.
[3,5,6] have 2 bits.
[7] has 3 bits.
The sorted array by bits is [0,1,2,4,8,3,5,6,7]
```



##### Approach 1 Comparator

```java
class Solution {
    public int[] sortByBits(int[] arr) {
        List<Integer> al= new ArrayList<>();
        for(int i:arr)
            al.add(i);
        Collections.sort(al, (a, b) -> 
                         Integer.bitCount(a) == Integer.bitCount(b) ? a - b :
                         Integer.bitCount(a) - Integer.bitCount(b));
        for(int i=0;i<arr.length;i++)
        arr[i]=al.get(i);
        return arr;
    }
}
```



#### 693 Binary Number with Alternating Bits

Given a positive integer, check whether it has alternating bits: namely, if two adjacent bits will always have different values.

**Example 1:**

```
Input: 5
Output: True
Explanation:
The binary representation of 5 is: 101
```

##### Approach 1 to string

```java
public boolean hasAlternatingBits(int n) {
        String bits = Integer.toBinaryString(n);
        for(int i=0; i<bits.length()-1; i++){
            if(bits.charAt(i)==bits.charAt(i+1)){
                return false;
            }
        }
        return true;
    }
```

##### Approach 2 bit wise

```java
public boolean hasAlternatingBits(int n){
    //      10101010101
    //  +    1010101010    ( number >> 1 )
    //  ---------------
    //  =   11111111111
    //  &  100000000000
    //  ---------------
    //  =             0    ( power of two )
    int tmp = ( n >> 1 ) + n;
    return (tmp & tmp + 1) == 0;
}
```



#### 762 Prime Number of Set Bits in Binary Representation

Given two integers `L` and `R`, find the count of numbers in the range `[L, R]` (inclusive) having a prime number of set bits in their binary representation.

(Recall that the number of set bits an integer has is the number of `1`s present when written in binary. For example, `21` written in binary is `10101` which has 3 set bits. Also, 1 is not a prime.)



**Example 1:**

```
Input: L = 6, R = 10
Output: 4
Explanation:
6 -> 110 (2 set bits, 2 is prime)
7 -> 111 (3 set bits, 3 is prime)
9 -> 1001 (2 set bits , 2 is prime)
10->1010 (2 set bits , 2 is prime)
```

##### Approach 1 Integer.bitCount()

```java
class Solution {
    public int countPrimeSetBits(int L, int R) {
        int cnt = 0;
        for(int i=L; i<=R; i++){
            if(isPrime(Integer.bitCount(i))){
                cnt++;
            }
        }
        return cnt;
    }
    
    private boolean isPrime(int n){
        if(n<=1) return false;
        for(int i=2; i<n; i++){
            if(n%i==0){
                return false;
            }
        }
        return true;
    }
    
}
```

Notice, For each number from `L` to `R`, let's find out how many set bits it has. If that number is `2, 3, 5, 7, 11, 13, 17`, or `19`, then we add one to our count. We only need primes up to 19.

```java
    private boolean isPrime(int x) {
        return (x == 2 || x == 3 || x == 5 || x == 7 ||
                x == 11 || x == 13 || x == 17 || x == 19);
    }
```

And if you think it is not a good idea to use bitCount directly, we can write one ourself.

##### Approach 2 bit mask count

```java
class Solution {
    public int countPrimeSetBits(int L, int R) {
        int cnt = 0;
        for(int i=L; i<=R; i++){
            if(isPrime(countBits(i))){
                cnt++;
            }
        }
        return cnt;
    }
    
    private boolean isPrime(int x) {
        return (x == 2 || x == 3 || x == 5 || x == 7 ||
                x == 11 || x == 13 || x == 17 || x == 19);
    }
    
    private int countBits(int x) {
        int mask = 1;
        int cnt = 0;
        while(mask<=x){
            if((mask&x)==mask){
                cnt++;
            }
            mask <<=1;
        }
        return cnt;
    }          
            
}
```

