# Math

## Easy

#### 1. Missing Number 
Given an array nums containing n distinct numbers in the range [0, n], return the only number in the range that is missing from the array.

```cpp
int missingNumber(vector<int>& nums) {
    int n = nums.size();
    int sum = 0;
    int total_sum = n*(n+1)/2;
    for(int i : nums) sum+=i;
    return total_sum - sum;
}
```

#### 2. Happy Number (Floyds Cycle Algo)
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

#### 3. Power of Three 
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

#### 4. Factorial Trailing Zeroes
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

## Medium

#### 1. Reordered Power of 2

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

#### 2. Broken Calculator (Think in Reverse)
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

#### 3. Perfect Squares (Lagrange's Four Square theorem)
Given an integer n, return the least number of perfect square numbers that sum to n.

Theorem: Every number can be represented using the sum of maximum for perfect squares.

If a number can be represent in the form of 4^r(8k + 7) => It can only be represented using sum of 4 squares

```cpp
int is_square(int n){  
    int temp = (int) sqrt(n);  
    return temp * temp == n;  
}  
int numSquares(int n) {  
    while ((n & 3) == 0)                          //n % 4 == 0  
        n >>= 2;  
    if ((n & 7) == 7) return 4;                   //n % 8 == 7  
    if(is_square(n)) return 1;  
    int sqrt_n = (int) sqrt(n);  
    for(int i = 1; i<= sqrt_n; i++){  
        if (is_square(n-i*i)) return 2;  
    }  
    return 3;  
} 
```

#### 4. Bulb Switcher (Puzzle)
There are n bulbs that are initially off. You first turn on all the bulbs, then you turn off every second bulb. On the third round, you toggle every third bulb (turning on if it's off or turning off if it's on). For the ith round, you toggle every i bulb. For the nth round, you only toggle the last bulb. Return the number of bulbs that are on after n rounds.

[bulb](https://assets.leetcode.com/uploads/2020/11/05/bulb.jpg)

```cpp
int bulbSwitch(int n) {
    return int(sqrt(n));
}
```

