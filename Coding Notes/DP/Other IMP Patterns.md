## @ House Robber Pattern

### 1. House Robber
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night. Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

Input: nums = [2,7,9,3,1]

Output: 12

```cpp
int rob(vector<int>& nums) {
    int n = nums.size();

    if(n==1) return nums[0];
    if(n==2) return max(nums[0], nums[1]);

    int loot = 0;
    vector<int> dp (n, 0);
    dp[0] = nums[0];
    dp[1] = max(nums[0], nums[1]);

    for(int i=2; i<n; i++)
        dp[i] = max(dp[i-2]+nums[i], dp[i-1]);

    return dp[n-1];
}
```

### 2. House Robber II (arranged in a circle)
Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

Input: nums = [1,2,3,1]

Output: 4

```cpp
/* 
    Implement the house robber logic two times, 
    one from [0 to n-2] and second for [1 to n-1] 
    and return max of two 
*/

int house_robber(vector<int> & nums, int start, int end){
    int len = nums.size()-1;
    vector<int> dp (len, 0);

    dp[0] = nums[start];
    dp[1] = max(nums[start], nums[start+1]);

    for(int i=start+2; i<=end; i++)
        dp[i-start] = max(dp[i-start-2] + nums[i], dp[i-start-1]);

    return dp[len-1];
}

int rob(vector<int>& nums) {
    int n = nums.size();

    if(n==1) return nums[0];
    if(n==2) return max(nums[0], nums[1]);

    int res1 = house_robber(nums, 0, n-2);
    int res2 = house_robber(nums, 1, n-1);

    return max(res1, res2);
}
```

### 3. Delete and Earn
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

### 1. Ugly Number I
Find whether a number is ugly or not.

```cpp
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

### 2. Ugly Number II
Given an integer n, return the nth ugly number.

[Video Explaination](https://www.youtube.com/watch?v=78Yx7oLA43s)

```cpp
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

### 3. Super Ugly Number
A super ugly number is a positive integer whose prime factors are in the array primes. Given an integer n and an array of integers primes, return the nth super ugly number.

Input: n = 12, primes = [2,7,13,19]

Output: 32

Hint: Genralize above approach using vectors instead of variables

```cpp
int nthSuperUglyNumber(int n, vector<int>& primes) {

    vector<int> ptr (primes.size(), 0);
    vector<int> nxt = primes;
    vector<int> dp (n, 1);

    int i=1;

    while(i<n){

        int idx = min_element(nxt.begin(), nxt.end()) - nxt.begin();
        dp[i] = nxt[idx];

        ptr[idx]++;
        nxt[idx] = dp[ptr[idx]] * primes[idx];

        if(dp[i] != dp[i-1]) i++;
    }

    return dp[n-1];
}
```

## @ Problems on Word Break

### 1. Word Break
Given a string s and a dictionary of strings wordDict, return true if s can be segmented into a space-separated sequence of one or more dictionary words.

Input: s = "leetcode", wordDict = ["leet","code"]

Output: true

```cpp
bool solve (string s, int n, int idx, unordered_set<string> & dict, unordered_set<string> & dp){
    if(idx==s.length()) return true;

    string key = s.substr(idx, n-idx+1);
    if(dp.find(key)!=dp.end()) return false;

    string temp = "";

    for(int i=idx; i<s.length(); i++){
        temp.push_back(s[i]);

        if(dict.find(temp)!=dict.end()){
            bool res = solve(s, n, i+1, dict, dp);
            if(res) return true;
        }
    }

    dp.insert(key);
    return false;
}


bool wordBreak(string s, vector<string>& wordDict) {
    int n = s.length();
    unordered_set<string> dict;

    for(string w : wordDict)
        dict.insert(w);

    unordered_set<string> dp;

    return solve (s, n, 0, dict, dp);
}
```
