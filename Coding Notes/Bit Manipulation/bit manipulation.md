# Bit Manipulation

## Easy

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
