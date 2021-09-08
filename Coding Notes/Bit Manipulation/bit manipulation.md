# Bit Manipulation

All Bitwise operators:
1. & (bitwise AND) takes two numbers as operands and does AND on every bit of two numbers. The result of AND is 1 only if both bits are 1. 
2. | (bitwise OR) takes two numbers as operands and does OR on every bit of two numbers. The result of OR is 1 if any of the two bits is 1. 
3. ^ (bitwise XOR) takes two numbers as operands and does XOR on every bit of two numbers. The result of XOR is 1 if the two bits are different. 
4. << (left shift) takes two numbers, left shifts the bits of the first operand, the second operand decides the number of places to shift. 
5. >> (right shift) takes two numbers, right shifts the bits of the first operand, the second operand decides the number of places to shift. 
6. ~ (bitwise NOT) takes one number and inverts all bits of it.

Note:
1. The left shift and right shift operators should not be used for negative numbers.

**Significance of 2's complement**

It is used to represent negative binary number.

Is

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

#### 2. Power of Four

```cpp
bool isPowerOfFour(int n) {
    return n>0 and (n&(n-1))==0 and (n-1)%3==0;
}
```

<br>

#### 3. Hamming Distance
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

#### 4. Binary Number with Alternating Bits
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

#### 5. Counting Bits
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

#### 6. Number Complement
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

<br>

#### 2. Missing Number
Given an array nums containing n distinct numbers in the range [0, n], return the only number in the range that is missing from the array.

Input: nums = [9,6,4,2,3,5,7,0,1]

Output: 8

Hint: Apply XOR operation to both the index and value of the array. In a complete array with no missing numbers, the index and value should be perfectly corresponding (nums[index] = index), so in a missing array, what left finally is the missing number.

```cpp
int missingNumber(vector<int>& nums) {
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

#### 3. XOR Queries of a Subarray
Given the array arr of positive integers and the array queries where queries[i] = [Li, Ri], for each query i compute the XOR of elements from Li to Ri (that is, arr[Li] xor arr[Li+1] xor ... xor arr[Ri] ). Return an array containing the result for the given queries.

Input: arr = [1,3,4,8], queries = [[0,1],[1,2],[0,3],[3,3]]

Output: [2,7,14,8] 

Hint:
1. Make use of property: x ^ x = 0
2. Use prefix xor (like prefix sum).

```cpp
vector<int> xorQueries(vector<int>& arr, vector<vector<int>>& queries) {
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


## @ Problems on Bitmask

#### 1. Maximum Product of Word Lengths
Given a string array words, return the maximum value of length(word[i]) * length(word[j]) where the two words do not share common letters. If no such two words exist, return 0.

Input: words = ["abcw","baz","foo","bar","xtfn","abcdef"]

Output: 16

```cpp
bool check(vector<bool> & v1, vector<bool> & v2){
    for(int i=0; i<26; i++){
        if(v1[i] && v2[i]) return false;
    }
    return true;
}    

int maxProduct(vector<string>& words) {
    vector<vector<bool>> v;         // --> It contains bitmask for every word
    unsigned long ans = 0;

    for(auto w : words){
        vector<bool> temp (26, false);
        for(auto ch : w)
            temp[ch-'a'] = true;
        
        v.push_back(temp);
    }

    for(int i=0; i<words.size(); i++){
        for(int j=i+1; j<words.size(); j++){
            if(check(v[i], v[j]))
                ans = max(ans, words[i].size()*words[j].size());
        }
    }

    return ans;
}
```
