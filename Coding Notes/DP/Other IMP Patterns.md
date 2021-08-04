## @ House Robber Pattern

### 1. Delete and Earn
You are given an integer array nums. You want to maximize the number of points you get by performing the following operation any number of times: Pick any nums[i] and delete it to earn nums[i] points. Afterwards, you must delete every element equal to nums[i] - 1 and every element equal to nums[i] + 1. Return the maximum number of points you can earn by applying the above operation some number of times.

Input: nums = [3,4,2]

Output: 6

```cpp
int deleteAndEarn(vector<int>& nums) {
    int mx = *max_element(nums.begin(), nums.end());
    vector<int> res (mx+1, 0);   

    /* storing frequency of every number in array */ 

    for(int n : nums)
        res[n]++;

    /* After that, implement house robber logic */

    for(int i=2; i<res.size(); i++)
        res[i] = max(res[i-2]+res[i]*i, res[i-1]);

    return res.back();
}
```

## @ Problems on Ugly Numbers
An ugly number is a positive integer whose prime factors are limited to 2, 3, and 5.

```
We can find whether a number is ugly or not by using following code:

bool isUgly(int n) {
    if(n<1) return false;

    while(n>1){
        if(n%2==0) n/=2;
        else if(n%3==0) n/=3;
        else if(n%5==0) n/=5;
        else return false;
    }

    return true;
}
```

### 1. Ugly Number II
An ugly number is a positive integer whose prime factors are limited to 2, 3, and 5.

[Video Explaination](https://www.youtube.com/watch?v=78Yx7oLA43s)

```
int nthUglyNumber(int n) {
    vector<int> dp (n, 1);

    int i2=0, i3=0, i5=0;
    int nxt2 = 2, nxt3 = 3, nxt5 = 5;

    int i=1;

    while(i<n){
        
        /* If nxt2 is smallest */
        
        if(nxt2<=nxt3 and nxt2<=nxt5){
            dp[i] = nxt2;
            i2++;
            nxt2 = dp[i2]*2;
        }
        
        /* If nxt3 is smallest */
        
        else if(nxt3<=nxt2 and nxt3<=nxt5){
            dp[i] = nxt3;
            i3++;
            nxt3 = dp[i3]*3;
        }
        
        /* If nxt5 is smallest */
        
        else{
            dp[i] = nxt5;
            i5++;
            nxt5 = dp[i5]*5;
        }

        if(dp[i]!=dp[i-1]) i++;       // --> For avoiding redundancy of factors
    }

    return dp[n-1];
}
```
