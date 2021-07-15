# Bit Manipulation

## Easy

#### 1. Power of Two
Given an integer n, return true if it is a power of two. Otherwise, return false. An integer n is a power of two, if there exists an integer x such that n == 2x.

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
