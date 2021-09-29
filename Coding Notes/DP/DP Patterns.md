# Some Common DP Patterns 

<br>

## @ House Robber Pattern

### 1. House Robber
Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police. Two adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

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

<br>

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

<br>

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

    /* After that, implement house robber logic on frequency array*/

    for(int i=2; i<res.size(); i++)
        res[i] = max(res[i-2]+res[i]*i, res[i-1]);

    return res.back();
}
```

<br>



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

<br>

### 2. Ugly Number II
Given an integer n, return the nth ugly number.

Hint: Initially point three pointer i2, i3 and i5 to dp[0], Now to fill dp[i], we need to find next multiple of 2, 3 and 5 which is only divisible by (2,3 or 5). And then increment the pointer, which is minimum.

[Video Explaination](https://www.youtube.com/watch?v=Lj68VJ1wu84)

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

<br>

### 3. Super Ugly Number
A super ugly number is a positive integer whose prime factors are in the array primes. Given an integer n and an array of integers primes, return the nth super ugly number.

Input: n = 12, primes = [2,7,13,19]

Output: 32

Hint: Genralize above approach using vectors instead of variables.

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

<br>



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

<br>

### 2. Word Break II
Given a string s and a dictionary of strings wordDict, add spaces in s to construct a sentence where each word is a valid dictionary word. Return all such possible sentences in any order. Same word in the dictionary may be reused multiple times in the segmentation.

Input: s = "catsanddog", wordDict = ["cat","cats","and","sand","dog"]

Output: ["cats and dog","cat sand dog"]

```cpp
vector<string> solve (string & s, int n, int idx, unordered_set<string> & dict, unordered_map<string, vector<string>> & dp){
    if(idx==n) return vector<string>();

    string key = s.substr(idx, n-idx+1);
    if(dp.find(key)!=dp.end()) return dp[key];

    vector<string> v;
    string temp = "";

    for(int i=idx; i<n; i++){
        temp.push_back(s[i]);

        if(dict.find(temp)!=dict.end()){
            if(i==n-1) 
                v.push_back(temp);
            else{
                auto res = solve (s, n, i+1, dict, dp);
                for(string str : res)
                    v.push_back(temp + " " + str);
            }
        }
    }

    return dp[key] = v;
}


vector<string> wordBreak(string s, vector<string>& wordDict) {
    int n = s.length();

    unordered_set<string> dict;
    for(string w : wordDict)
        dict.insert(w);

    unordered_map<string, vector<string>> dp;
    return solve (s, n, 0, dict, dp);
}
```

<br>

### 3. Palindrome Partitioning II
Given a string s, partition s such that every substring of the partition is a palindrome. Return the minimum cuts needed for a palindrome partitioning of s.

```cpp
bool isPalindrome (string & s, int i, int j){
    while(i<j){
        if(s[i]!=s[j]) return false;
        i++; j--;
    }
    return true;
}


int solve (string & s, int i, int j, vector<vector<int>> & dp){    
    if(i>=j or isPalindrome(s, i, j)) return 0;
    if(dp[i][j]!=-1) return dp[i][j];

    int ans = INT_MAX;

    for(int k=i; k<j; k++){

        /* 
            Instead of writing below standard line
            We will recurse for only right part
            Only when left part turns out to be palindrome

            int temp =  solve (s, i, k, dp, palindrome) + solve (s, k+1, j, dp, palindrome) + 1;
        */

        if(isPalindrome(s, i, k)){                         
            int temp = solve (s, k+1, j, dp) + 1;
            ans = min (ans, temp);
        }
    }

    return dp[i][j] = ans;
}


int minCut(string s) {
    int n = s.length();
    vector<vector<int>> dp (n+1, vector<int> (n+1, -1));

    return solve (s, 0, n-1, dp);
}
```

**Optimization:** Since second variable j is not changing in recursive call, Hence inplace of 2D DP, we can use 1D DP

```cpp

/* isPalindrome function will be similar to prev code */

int solve (string & s, int idx, vector<int> & dp){    
    if(isPalindrome(s, idx, s.length()-1)) return 0;

    if(dp[idx]!=-1) 
        return dp[idx];

    int ans = INT_MAX;

    for(int k=idx; k<s.length(); k++){
        if(isPalindrome(s, idx, k)){                         
            int temp = solve (s, k+1, dp) + 1;
            ans = min (ans, temp);
        }
    }

    return dp[idx] = ans;
}


int minCut(string s) {
    int n = s.length();
    vector<int> dp (n+1, -1);

    return solve (s, 0, dp);
}
```

<br>

### 4. Palindrome Partitioning IV
Given a string s, return true if it is possible to split the string s into three non-empty palindromic substrings. Otherwise, return false.

Input: s = "abcbdd"

Output: true

```cpp
bool isPalindrome (string & s, int i, int j){
    while(i<j){
        if(s[i]!=s[j]) return false;
        i++; j--;
    }
    return true;
}


bool solve (string & s, int n, int idx, int cnt, vector<vector<int>> & dp){
    if(dp[idx][cnt]!=-1) return false;

    if(cnt==1) return dp[idx][cnt] = isPalindrome(s, idx, n-1);

    for(int k=idx; k<n-cnt+1; k++){
        if(isPalindrome(s, idx, k)){
            bool res = solve (s, n, k+1, cnt-1, dp);
            if(res) return true;
        }
    }

    return dp[idx][cnt] = false;
}


bool checkPartitioning(string s) {
    int n = s.length();
    vector<vector<int>> dp (n+1, vector<int> (4, -1));
    
    return solve (s, n, 0, 3, dp);      //--> For this question cnt is 3
}
```

<br>

### 5. Arrange II (IB)
You are given a sequence of black and white horses, and a set of K stables. You have to accommodate the horses into the stables such that:
1. You fill the horses into the stables preserving the relative order of horses. 
2. No stable should be empty and no horse should be left unaccommodated.
3. Take the product (number of white horses * number of black horses) for each stable and take the sum of all these products. This value should be the minimum among all possible accommodation arrangements.

Input: "WWWB" , K = 2

Output: 0

```cpp

int solve (string &A, int idx, int n, vector<vector<int>> &dp, int k, vector<int> &whiteCnt, vector<int> &blackCnt){
    if(idx==A.size()) return 0;
    if(k==1) return whiteCnt[idx] * blackCnt[idx];

    if(dp[idx][k]!=-1) return dp[idx][k];

    int res = INT_MAX;
    int wc = 0, bc = 0;

    for(int i=idx; i<n; i++){
        if(A[i]=='W') wc++;
        else bc++;

        res = min(wc*bc + solve(A, i+1, n, dp, k-1, whiteCnt, blackCnt), res);
    } 

    return dp[idx][k] = res;
}

int Solution::arrange(string A, int k) {
    int n = A.size();
    if(n<k) return -1;

    vector<int> whiteCnt (n, 0);
    vector<int> blackCnt (n, 0);

    for(int i=n-1; i>=0; i--){
        if(i<n-1){
            whiteCnt[i] = whiteCnt[i+1];
            blackCnt[i] = blackCnt[i+1];
        }

        if(A[i]=='W') whiteCnt[i]++;
        else blackCnt[i]++;
    }

    vector<vector<int>> dp (n, vector<int> (k+1, -1));
    return solve (A, 0, n, dp, k, whiteCnt, blackCnt);
}
```

<br>



## @ Jump Game Pattern

### 1. Jump Game
You are given an integer array nums. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position. Return true if you can reach the last index, or false otherwise.

Input: nums = [2,3,1,1,4]

Output: true

```cpp
bool canJump(vector<int>& nums) {
    int n = nums.size();
    int reach = 0;

    for(int i=0; i<=min(n-1, reach); i++){
        reach = max(reach, i+nums[i]);
        if(reach>=n-1) return true;
    }

    return false;
}
```

<br>

### 2. Jump Game II
Given an array of non-negative integers nums, you are initially positioned at the first index of the array. Each element in the array represents your maximum jump length at that position. Your goal is to reach the last index in the minimum number of jumps.

Input: nums = [2,3,0,1,4]

Output: 2

**Approach 1 : DP - O(n2) | O(n)**

```cpp
int jump(vector<int>& nums) {
    int n = nums.size();

    vector<int> dp (n, INT_MAX);
    dp[0] = 0;

    for(int i=1; i<n; i++){
        for(int j=0; j<i; j++){

            if(j+nums[j]>=i)
                dp[i] = min (dp[i], dp[j]+1);

        }
    }

    return dp[n-1];
}
```

**Approach 2 : Greedy - O(n) | O(1)**

```cpp
int jump(vector<int>& nums) {
    int n = nums.size();

    int steps = 0;
    int begin = 0, end = 0, farthest = 0;

    for(int i=0; i<n-1; i++){

        farthest = max(farthest, i+nums[i]);

        /* If we reached to the end of prev jump, then we need to increment jump steps and update end to farthest */

        if(i==end){
            end = farthest;
            steps++;
        }
    }

    return steps;
}
```

<br>

### 3. Jump Game VII
You are given a 0-indexed binary string s and two integers minJump and maxJump. In the beginning, you are standing at index 0, which is equal to '0'. You can move from index i to index j if the following conditions are fulfilled:
1. i + minJump <= j <= min(i + maxJump, s.length - 1), and
2. s[j] == '0'.

Return true if you can reach index s.length - 1 in s, or false otherwise.

Input: s = "011010", minJump = 2, maxJump = 3

Output: true

**Approach 1 : Memorization**

```cpp
bool solve (string & s, int minJump, int maxJump , int idx, int n, vector<int> & dp){
    if(idx==n-1) return true;

    if(dp[idx]!=-1) return dp[idx];

    for(int i=idx+minJump; i<=min(n-1, idx+maxJump); i++){
        if(s[i]=='0'){
            bool res = solve (s, minJump, maxJump, i, n, dp);
            if(res) return dp[idx] = true;
        }    
    }    

    return dp[idx] = false;
}


bool canReach(string s, int minJump, int maxJump) {
    int n = s.length();
    vector<int> dp (n, -1);

    return solve (s, minJump, maxJump, 0, n, dp);
}
```

**Approach 2 : Iterative DP + Sliding window - O(n)**

```cpp
bool canReach(string s, int minJump, int maxJump) {
    int n = s.length(); 

    vector<bool> dp (n, false);
    dp[0] = true;

    int cnt = 0;                    // --> will store cnt of zeros in sliding window

    for(int i=minJump; i<n; i++){

        if(dp[i-minJump]) 
            cnt++;

        if(i-maxJump-1>=0 and dp[i-maxJump-1])
            cnt--;

        /* If s[i]==0 and cnt>0, it means that we have atleast one pos in window to jump at curr index i */

        if(s[i]=='0' and cnt>0) dp[i] = true;    
    }

    return dp[n-1];
}
```

<br>



## @ Stock Buy-Sell

### 1. Best Time to Buy and Sell Stock IV
You are given an integer array prices where prices[i] is the price of a given stock on the ith day, and an integer k. Find the maximum profit you can achieve. You may complete at most k transactions. You must sell the stock before you buy again.

Input: k = 2, prices = [3,2,6,5,0,3]

Output: 7

```cpp
int solve (vector<int> & prices, int n, int idx, int k, bool buySell, vector<vector<vector<int>>> & dp){
    if(idx==n or k==0) return 0;

    if(dp[idx][k][buySell]!=INT_MIN) return dp[idx][k][buySell];

    /* If buySell==true, means we already have one stock to sell */

    if(buySell){
        int sell = solve (prices, n, idx+1, k-1, false, dp) + prices[idx];
        int nosell = solve (prices, n, idx+1, k, true, dp);

        return dp[idx][k][buySell] = max(sell, nosell);
    }

    /* Else we can either buy stock at price[idx] or leave it */

    else{
        int buy = solve (prices, n, idx+1, k, true, dp) - prices[idx];
        int nobuy = solve (prices, n, idx+1, k, false, dp);

        return dp[idx][k][buySell] = max(buy, nobuy);
    }
}


int maxProfit(int k, vector<int>& prices) {
    int n = prices.size();
    vector<vector<vector<int>>> dp (n+1, vector<vector<int>> (k+1, vector<int> (2, INT_MIN)));

    return solve (prices, n, 0, k, false, dp);
}
```

<br>
