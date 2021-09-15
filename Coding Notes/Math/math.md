# Math

<br>

## @ Standard Algos

### 1. Greatest Common Divisor (Euclidean Algo)

```cpp
int gcd(int a, int b){
    if (b == 0)
        return a;
    return gcd(b, a % b);
}
```

<br>

### 2. Happy Number (Floyds Cycle Algo)
Write an algorithm to determine if a number n is happy. A happy number is a number defined by the following process:
1. Starting with any positive integer, replace the number by the sum of the squares of its digits.
2. Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
3. Those numbers for which this process ends in 1 are happy.
4. Return true if n is a happy number, and false if not.

```cpp
int squareSum(int n){
    int sum = 0;
    while(n){
        int digit = n%10;
        n = n/10;
        sum += digit*digit;
    }
    return sum;
}

bool isHappy(int n) {
    int slow = n , fast = n;
    do{
        slow = squareSum(slow);
        fast = squareSum(fast);
        fast = squareSum(fast);
    }
    while(slow!=fast);

    if(slow==1) return true;
    else return false;
}
```

<br>

### 3. Factorial Trailing Zeroes
Given an integer n, return the number of trailing zeroes in n!.

```cpp
int trailingZeroes(int n) {
    int ans = 0;
    while(n){
        ans += n/5;
        n/=5;
    }
    return ans;
}
```

<br>



## @ Prime Time

### 1. Count Primes (Sieve of Eratosthenes)
Count the number of prime numbers less than a non-negative number, n.

```cpp
int countPrimes(int n) {
    if(n<2) return 0;
    int root = sqrt(n);
    vector<bool> primes(n, false);
    int count = 0;

    for(int i=2; i<=root; i++){
        if(!primes[i]){
            for(int j=i+i; j<n; j+=i)
                primes[j] = true;
        }
    }

    for(int i=2; i<n; i++){
        if(!primes[i]) count++; 
    }
    return count;
}
```

<br>

### 2. Prime Sum
Given an even number (greater than 2), return two prime numbers whose sum will be equal to given number.

**Theorem: Goldbach's conjecture**

It states that every even whole number greater than 2 is the sum of two prime numbers. The conjecture has been shown to hold for all integers less than 4 × 10^18.

```cpp
vector<bool> primeSieve (int A){
    vector<bool> prime (A+1, true);
    prime[0] = false; prime[1] = false;
    int root = sqrt(A);

    for(int i=2; i<=root; i++){
        if(!prime[i]) continue;

        for(int j=i+i; j<prime.size(); j+=i)
            prime[j] = false;
    }

    return prime;
}

vector<int> Solution::primesum(int A) {
    vector<bool> prime  = primeSieve(A);

    for(int i=2; i<prime.size(); i++){
        if(prime[i] and prime[A-i])
            return {i, A-i};
    }

    return {-1, -1};
}
```

<br>



## @ Playing With Powers

### 1. Matrix Exponentiation
Given a matrix M and an integer n, compute pow(M,n).

Prerequisite: Fast Exponentiation using Binary Search

```
Approach:

1. If n is even
    pow(M,n) = pow(M,n/2) * pow(M,n/2)
    
2. If n is odd  
    pow(M,n) = pow(M,n/2) * pow(M,n/2) * M
```

```cpp
vector<vector<long long>> multiply (vector<vector<long long>> &m1, vector<vector<long long>> &m2){
    int n = m1.size();
    int mod = 1000000007;
    vector<vector<long long>> res (n, vector<long long> (n, 0));

    for(int i=0; i<n; i++)
        for(int j=0; j<n; j++)
            for(int k=0; k<n; k++)
                res[i][j] = (res[i][j] + (m1[i][k] * m2[k][j]) % mod) % mod;
        
    return res;
}


vector<vector<long long>> matrix_exponentiation (vector<vector<long long>> &m, int n){
    if(n==1) return m;

    auto a = matrix_exponentiation(m, n/2);

    /* If power is odd */

    if(n&1){
        auto b = multiply(a, a);
        return multiply(b, m);
    }

    /* If power is even */

    else 
        return multiply(a, a);
}
```

<br>

### 2. Find Nth Fibonacci
F1 = 1, F2 = 1, Find the nth fibonacci number Fn = Fn-1 + Fn-2 (n > 2)

![img](https://pbs.twimg.com/media/EXlf0njXsAAXGvp.png)

Note: Use above function for matrix exponentiation

```cpp
int fib (int n) {
    vector<vector<long long>> m = {{1, 1}, {1, 0}};

    auto res = matrix_exponentiation(m, n-1);
    return res[0][0];
}
```

<br>

### 3. Power of Three 
Given an integer n, return true if it is a power of three. Otherwise, return false.

```cpp
/* Using LOGARITHM */

bool isPowerOfThree(int n) {
    if(n==0) return false;
    double temp = log10(n)/log10(3);
    if(floor(temp)==ceil(temp)) return true;
    return false;
}

/* Using INTEGER CONSTRAINTS */

bool isPowerOfThree(int n) {
    int max = pow(3, 19);
    return n>0 && max%n==0;
}
```

<br>

### 4. Power Of Two Integers
Given a positive integer which fits in a 32 bit signed integer, find if it can be expressed as A^P where P > 1 and A > 0. A and P both should be integers.

```cpp
int Solution::isPower(int A) {
    if(A==1) return true;
    int root = sqrt(A);
    
    /* check for all numbers smaller then root(A) whether they can be x which satisfies x^y */  

    for(int i=2; i<=root; i++){
        int temp=A;

        while(temp%i==0)
            temp /= i;
        
        if(temp==1) return true;
    }

    return false;
}
```

<br>

### 5. Perfect Squares 
Given an integer n, return the least number of perfect square numbers that sum to n.

**Theorem: Lagrange's Four Square theorem**
1. Every number can be represented using the sum of maximum for perfect squares. 
2. If a number can be represent in the form of 4^r(8k + 7) => It can only be represented using sum of 4 squares.

```cpp
int is_square(int n){  
    int temp = (int) sqrt(n);  
    return temp * temp == n;  
}  


int numSquares(int n) {  
    while ((n & 3) == 0)                            // --> n % 4 == 0  
        n >>= 2;  
        
    if ((n & 7) == 7)                               // --> n % 8 == 7  
        return 4;                     
        
    if(is_square(n)) 
        return 1;  
        
    int sqrt_n = (int) sqrt(n);  
    
    for(int i = 1; i<= sqrt_n; i++){  
        if (is_square(n-i*i)) 
            return 2;  
    }  
    
    return 3;  
} 
```

<br>

### 6. Reordered Power of 2

You are given an integer n. We reorder the digits in any order (including the original order) such that the leading digit is not zero. Return true if and only if we can do this so that the resulting number is a power of two.

Hint: store frequency of digits of input. Compare it with frequency of digits of every power of 2 till pow(2, 32).

```cpp
bool comp(vector<int> v1, vector<int> v2){
    for(int i=0; i<=9; i++)
        if(v1[i]!=v2[i]) return false;
    
    return true;
}

bool reorderedPowerOf2(int N) {
    if(N==1 || N==2 || N==4 || N==8) return true; 
    vector<int> num(10, 0);
    while(N>0){
        int digit = N%10;
        num[digit]++;
        N /= 10;
    }

    for(int i=4; i<=32; i++){
        long long int p = pow(2,i);
        vector<int> pv (10, 0);
        while(p>0){
            int digit = p%10;
            pv[digit]++;
            p /= 10;
        }
        bool res = comp(num, pv);
        if(res) return true;
    }
    return false;
}
```

<br>



## @ Problems on Sum 

### 1. Missing Number 
Given an array nums containing n distinct numbers in the range [0, n], return the only number in the range that is missing from the array.

PS: In come cases, It might give overflow (Use Swap sort)

```cpp
int missingNumber(vector<int>& nums) {
    int n = nums.size();
    int sum = 0;
    int total_sum = n*(n+1)/2;
    for(int i : nums) sum+=i;
    return total_sum - sum;
}
```

<br>

### 2. Repeat and Missing Number Array
You are given a read only (we can't swap of modify it) array of n integers from 1 to n. Each integer appears exactly once except D which appears twice and M which is missing. Return D and M.

Input:[3 1 2 5 3] 

Output:[3, 4] 

Hint: Form two equations, one with sum and other with sqr_sum.

```cpp
vector<int> Solution::repeatedNumber(const vector<int> &A) {

    /* 
        Solve following two equations:

        actual_sum = sum(A) - Duplicate + Missing
        Missing - Duplicate = actual_sum - sum(A)

        M-D = actual_sum - sum(A)                                           ------> eq1

        And,
        actual_sqr_sum = sum(sqrs(A)) - sqr(Duplicate) + sqr(Missing)
        sqr(Missing) - sqr(Duplicate) = actual_sqr_sum - sum(sqrs(A))
        (M+D)(M-D) = actual_sqr_sum - sum(sqrs(A))

        M+D = (actual_sqr_sum - sum(sqrs(A))) / (actual_sum - sum(A))       -----> eq2 

    */


    long long n = A.size();
    long long sum = 0, actual_sum = 0;
    long long sqr_sum = 0, actual_sqr_sum = 0;

    for(int a : A){
        long long num = a;
        sum += num;
        sqr_sum += num * num;
    }

    actual_sum = n*(n+1)/2;
    actual_sqr_sum = n*(n+1)*(2*n+1)/6;

    long long diff_sqr_sum = actual_sqr_sum - sqr_sum;
    long long diff_sum = actual_sum - sum;

    long long ratio = diff_sqr_sum / diff_sum;

    long long M = (ratio + diff_sum)/2;
    long long D = (ratio - diff_sum)/2;

    return {D, M}; 
}
```

<br>


## @ Some Tricky Problems

### 1. Bulb Switcher (Puzzle)
There are n bulbs that are initially off. You first turn on all the bulbs, then you turn off every second bulb. On the third round, you toggle every third bulb (turning on if it's off or turning off if it's on). For the ith round, you toggle every i bulb. For the nth round, you only toggle the last bulb. Return the number of bulbs that are on after n rounds.

![bulb](https://assets.leetcode.com/uploads/2020/11/05/bulb.jpg)

```cpp
int bulbSwitch(int n) {
    return int(sqrt(n));
}
```

<br>

### 2. Partitioning Into Minimum Number Of Deci-Binary Numbers
A decimal number is called deci-binary if each of its digits is either 0 or 1 without any leading zeros. For example, 101 and 1100 are deci-binary, while 112 and 3001 are not. Given a string n that represents a positive decimal integer, return the minimum number of positive deci-binary numbers needed so that they sum up to n.

Input: n = "32"

Output: 3

Explanation: 10 + 11 + 11 = 32

```cpp
int minPartitions(string n) {
    int ans = 0;
    for(char ch : n){
        int digit = ch-'0';
        ans = max(ans, digit);
    }
    return ans;
}
```

<br>

### 3. Step by Step
Given a target A on an infinite number line, i.e. -infinity to +infinity. You are currently at position 0 and you need to reach the target by moving according to the rule: In ith move you can take i steps forward or backward. Find the minimum number of moves required to reach the target.

Approach:
1. First of all, make target = abs(target) because if magnitude is equal then steps to reach target will also be equal. 
2. Then, if curr_pos < target, then increment the curr_pos and steps
3. Else if curr_pos >= target, and difference of pos and target is odd, then also increment the pos and steps.
4. Else return steps taken so far.

```cpp
int Solution::solve(int target) {
    target = abs(target);
    
    int i=1;
    int sum = 0, steps = 0;
    
    while(sum<target or (sum-target)%2==1){
        sum+=i;
        i++; steps++;
    }
    
    return steps;
}
```

<br>

### 4. Broken Calculator (Think in Reverse)
On a broken calculator that has a number showing on its display, we can perform two operations:

1. Double: Multiply the number on the display by 2, or,
2. Decrement: Subtract 1 from the number on the display.

Initially, the calculator is displaying the number X. Return the minimum number of operations needed to display the number Y.

Hint: Try Y to make X not X to Y

```cpp
int brokenCalc(int X, int Y) {
    if(X==Y) return 0;
    if(X>Y) return X-Y;

    int steps = 0;
    while(Y>X){
        if(Y%2==0) Y/=2;
        else Y++;
        steps++;
    }
    steps+=X-Y;
    return steps;
}
```
