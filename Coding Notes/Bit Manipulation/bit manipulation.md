# Bit Manipulation

<br>

## @ Basic Bits

#### 1. Power of Two

Hint: If a number is power of 2 then it has only 1 set bit in its binary representation

```cpp
bool isPowerOfTwo(int n) {
    if(n<0) return false;

    int cnt = 0;  
    while(n){
        if(n&1)
            cnt++;
        n>>=1;
    }

    return cnt==1;
}
```

<br>

#### 2. Hamming Distance
The Hamming distance between two integers is the number of positions at which the corresponding bits are different. Given two integers x and y, return the Hamming distance between them.

```cpp
int hammingDistance(int x, int y) {
    int cnt = 0;

    while(x>0 or y>0){
        int bit_x = x&1;
        int bit_y = y&1;

        cnt += bit_x xor bit_y;

        x>>=1;
        y>>=1;
    }

    return cnt;
}
```

<br>

#### 3. Binary Number with Alternating Bits
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

<br>

#### 4. Counting Bits
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

#### 5. Number Complement
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

#### 1. Single Number
Given a non-empty array of integers nums, every element appears twice except for one. Find that single one.

```cpp

Time: O(N)
Space: O(1) 

int singleNumber(vector<int>& nums) {
    int ans = 0;

    for(int num : nums)
        ans = ans xor num;

    return ans;
}
```
