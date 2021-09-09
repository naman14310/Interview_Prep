# Bit Manipulation
```
All Bitwise operators:

1. &    -   Bitwise AND
2. |    -   Bitwise OR
3. ^    -   Bitwise XOR
4. >>   -   Right Shift binary numbers (or divide by 2 in decimal)
5. <<   -   Left shift binary numbers (or multiply by 2 in decimal)
6. ~    -   1's complement (Invert all bits in binary representation)
```

Note: The left shift and right shift operators should not be used for negative numbers.

<br>

### Significance of 2's complement

It is used to represent negative binary number. For eg: -5 is 2's complement of 5 in binary world.

Steps:
1. Invert all bits (by ~ operator)
2. Add 1 to it

Eg: 2's complement of 5 will be

```
Binary representation of 5  =   00000000000000000000000000000101
1's complement of 5         =   11111111111111111111111111111010  (after inverting bits using ~)
2's complement of 5 (or -5) =   11111111111111111111111111111011  (after adding 1 to 1's complement)
```

<br>

### Addition/Subtraction of Binary Numbers

```
Addition:

1 + 0 = 1
0 + 0 = 0
1 + 1 = 10 (where 1 is carry)
1 + 1 + 1 = 11 (where 1 is carry)

Subtraction:

We do not have any direct subtraction operation in binary system. We just add negative numbers.

For eg: 12-5 = 12 + (-5) = 12 + 2's complement of (5)

Hence, 00000000000000000000000000001100
     + 11111111111111111111111111111011 
      ----------------------------------
       00000000000000000000000000000111  (it is equals to 12-5 = 7)       
```

<br>

### Some Tricks
1. n & (n-1) will update least significant set bit (rightmost 1) to 0
2. If (n & (n+1) == 0) then all bits of numbers are set bits (i.e all 1's)

<br>


## @ Basic Bits


### 1. Check Odd-Even

```cpp
string checkOddEven(int n){
    if(n&1) return "odd";
    else return "even";
}
```

<br>

### 2. Get/Set/Clear ith bits

```cpp
int get_ith_bit(int n, int i){
    int mask = 1<<i;

    if(n&mask)
        return 1;
    else
        return 0; 
}


int set_ith_bit(int n, int i){
    int mask = 1<<i;
    n = n | mask;
    return n;
} 


int clear_ith_bit(int n, int i){
    int mask = ~(1<<i);
    n = n & mask;
    return n;
} 
```

<br>

### 3. Count Set Bits

**Log(n) approach**

```cpp
int countSetBits(int num){
    int cnt = 0;

    while(num>0){ 
        cnt += num&1;
        num>>=1;
    }

    return cnt;
}
```

**Optimized approach**

```cpp
int countSetBits(int num){
   int cnt = 0;

   while(num>0){ 
       cnt ++;
       num = num & (num-1);   // --> It will update righmost 1 to 0 in each iteration
   }

   return cnt;
}
```

<br>

### 4. Hamming Distance
The Hamming distance between two integers is the number of positions at which the corresponding bits are different. Given two integers a and b, return the Hamming distance between them.

Hint: Compute xor of both and cnt number of 1's.

```cpp
int number_of_changed_bits(int a, int b){
    int n = a xor b;
    return countSetBits(n);
}
```

<br>

### 5. Power of Two

Hint: If a number is power of 2 then it has only 1 set bit in its binary representation

**Log(n) approach**

```cpp
bool isPowerOfTwo(int n) {
    if(n<0) return false;

    int cnt = 0;  
    while(n){
        if(n&1) cnt++;
        n>>=1;
    }

    return cnt==1;
}
```

**O(1) approach**

```cpp
bool isPowerOfTwo(int n) {
   return n>0 and ((n & (n-1)) == 0);
}
```

<br>

### 6. Power of Four

```cpp
bool isPowerOfFour(int n) {
    return n>0 and (n&(n-1))==0 and (n-1)%3==0;
}
```

<br>

### 7. Binary Number with Alternating Bits
Given a positive integer, check whether it has alternating bits: namely, if two adjacent bits will always have different values.

```cpp
bool hasAlternatingBits(int n) {
    int prev = -1;

    while(n>0){
        int bit = n&1;
        if(bit==prev) return false;

        prev = bit;
        n>>=1;
    }

    return true;
}
```

**Direct Method - O(1)**

```cpp
bool hasAlternatingBits(int n) {
   long x = n xor (n>>1);       // --> will produce all set bits if they are in alternating fashion
   return (x & (x+1)) == 0;
}
```

<br>

### 8. Counting Bits
Given an integer n, return an array ans of length n + 1 such that for each i (0 <= i <= n), ans[i] is the number of 1's in the binary representation of i.

Input: n = 5

Output: [0,1,1,2,1,2]

**Approach 1 : Bit Counting**

```cpp
int countSetBits(int num){
    int cnt = 0;

    while(num>0){ 
        cnt += num&1;
        num>>=1;
    }

    return cnt;
}


vector<int> countBits(int n) {
    vector<int> res;

    for(int i=0; i<=n; i++)
        res.push_back(countSetBits(i));

    return res;
}
```

**Approach 2 : Using DP**

[Explaination](https://leetcode.com/problems/counting-bits/discuss/800456/C%2B%2B-or-Counting-Bits-or-O(N)-Explanation)

1. If the number is even, the number of 1s is equal to the number which is half of it. That is because number i is just left shift by 1 bit from number i / 2.
2. If the numbers are odd, The number of 1 bits is equal to the number (i - 1) plus 1.

```cpp
vector<int> countBits(int n) {
    vector<int> res;
    res.push_back(0);

    for(int i=1; i<=n; i++){

        /* If i is ODD */

        if(i&1) 
            res.push_back(res[i-1]+1);

        /* If i is EVEN */

        else 
            res.push_back(res[i/2]);
    }

    return res;
}
```

<br>

### 9. Number Complement
The complement of an integer is the integer you get when you flip all the 0's to 1's and all the 1's to 0's in its binary representation. Given an integer num, return its complement.

```cpp
int findComplement(int num) {
    int complement = 0;
    int shift = 0;

    while(num>0){
        int complement_bit = !(num&1);
        complement = complement | (complement_bit << shift);

        num>>=1;
        shift++;
    }

    return complement;
}
```

<br>


## @ Problems on XOR

Properties of xor:
1. n ^ 0 = n
2. n ^ n = 0
3. XOR of the XOR’s of all subsets is always 0 when n > 1 and Set[0] when n is 1. 

<br>

### 1. Swap two numbers

```cpp
void swap(int & a, int & b){
    a = a xor b;
    b = a xor b;
    a = a xor b;
}
```

<br>

### 2. Toggle ith bit of Binary Number

```cpp
void toggle(int n, int i){
     int mask = 1<<i;
     n = n xor mask;
     return n;
}
```

<br>

### 3. Compute XOR from 1 to n

```cpp
/* Direct XOR of all numbers from 1 to n */

int computeXOR(int n){
    if (n % 4 == 0)
        return n;
    if (n % 4 == 1)
        return 1;
    if (n % 4 == 2)
        return n + 1;
    else
        return 0;
}
```

<br>

### 4. Single Number
Given a non-empty array of integers nums, every element appears twice except for one. Find that single one.

```cpp

Time: O(N)
Space: O(1) 

int singleNumber(vector<int>& nums){
    int ans = 0;

    for(int num : nums)
        ans = ans xor num;

    return ans;
}
```

<br>

### 5. Missing Number
Given an array nums containing n distinct numbers in the range [0, n], return the only number in the range that is missing from the array.

Input: nums = [9,6,4,2,3,5,7,0,1]

Output: 8

Hint: Apply XOR operation to both the index and value of the array. In a complete array with no missing numbers, the index and value should be perfectly corresponding (nums[index] = index), so in a missing array, what left finally is the missing number.

```cpp
int missingNumber(vector<int>& nums){
    int n = nums.size();
    int ans = n;            // --> initialised with n so that it will act as an index for number n in [0,n]

    for(int i=0; i<n; i++){
        ans = ans xor i;
        ans = ans xor nums[i];
    }

    return ans;
}
```

Alternate approach : Math logic

<br>

### 6. XOR Queries of a Subarray
Given the array arr of positive integers and the array queries where queries[i] = [Li, Ri], for each query i compute the XOR of elements from Li to Ri (that is, arr[Li] xor arr[Li+1] xor ... xor arr[Ri] ). Return an array containing the result for the given queries.

Input: arr = [1,3,4,8], queries = [[0,1],[1,2],[0,3],[3,3]]

Output: [2,7,14,8] 

Hint:
1. Make use of property: x ^ x = 0
2. Use prefix xor (like prefix sum).

```cpp
vector<int> xorQueries(vector<int>& arr, vector<vector<int>>& queries){
    int n = arr.size();
    vector<int> res;
    vector<int> prefix_xor (n, 0);

    prefix_xor[0] = arr[0];

    for(int i=1; i<n; i++)
        prefix_xor[i] = prefix_xor[i-1]^arr[i];

    for(auto q : queries){
        int l = q[0], r = q[1];

        if(l==0) 
            res.push_back(prefix_xor[r]);
        else
            res.push_back(prefix_xor[r] ^ prefix_xor[l-1]);
    }

    return res;
}
```

<br>

### 7. Equal Sum and XOR
Given a positive integer n, find count of positive integers i such that 0 <= i <= n and n+i = n^i 

Hint: Answer = pow(2, count of zero bits)

```cpp
int countValues(int n){
    int unset_bits=0;
    
    while(n){
        if((n & 1) == 0) unset_bits++;
        n=n>>1;
    }
 
    return 1 << unset_bits;      // --> Return 2 ^ unset_bits
}
```

