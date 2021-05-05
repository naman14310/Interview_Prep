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

