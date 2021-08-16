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

### 3. Minimum Number of Taps to Open to Water a Garden (Tricky)
There is a one-dimensional garden on the x-axis. The garden starts at the point 0 and ends at the point n. There are n + 1 taps located at points [0, 1, ..., n] in the garden. Given an integer n and an integer array ranges of length n + 1 where ranges[i] (0-indexed) means the i-th tap can water the area [i - ranges[i], i + ranges[i]] if it was open. Return the minimum number of taps that should be open to water the whole garden, If the garden cannot be watered return -1.

Input: n = 7, ranges = [1,2,1,0,2,1,0,1]

Output: 3

```cpp
/*      
        Let n = 4, ranges = [2, 2, 0, 1, 0]             


        2       2       0       1       0               |       Ranges
                                                        |                    
        x--------------->                               |       [-2, 2]
        <-------x--------------->                       |       [-1, 3]
                        x                               |          x
                        <-------x------->               |       [2, 4]
                                        x               |          x


        x represents every tap at index i
        and line represents the range of every tap
*/


int minTaps(int n, vector<int>& ranges) {

    int reach = 0;
    int taps_required = 0;
    int idx = 0;

    while(reach<n){

        /* Search for the tap whose range starts before the curr reach and go more farthest from all others */

        int farthest = reach;

        for(int i=idx; i<=n; i++){

            if(i-ranges[i]<=reach and i+ranges[i]>farthest){
                farthest = i+ranges[i];
                idx = i;                // --> we will only chk for tap which are ahead when we updated farthest
            }
        }

        /* If value of farthest does not increase that means we can't go more farther to right using these taps */

        if(farthest==reach) return -1;

        /* Else update our reach and increment taps_required */

        reach = farthest;
        taps_required++;
    }

    return taps_required;
}
```

### 4. Video Stitching
You are given a series of video clips. These video clips can be overlapping with each other and have varying lengths. Each video clip is described by an array clips where clips[i] = [starti, endi] indicates that the ith clip started at starti and ended at endi.We can cut these clips into segments freely. For example, a clip [0, 7] can be cut into segments [0, 1] + [1, 3] + [3, 7].

Return the minimum number of clips needed so that we can cut the clips into segments that cover the entire sporting event [0, time]. If the task is impossible, return -1.

Hint: Use same concept as above

```cpp
int videoStitching(vector<vector<int>>& clips, int time) {
    int reach = 0; 
    int clips_required = 0;

    while(reach<time){
        int farthest = reach;

        for(int i=0; i<clips.size(); i++){

            if(clips[i][0]<=reach and clips[i][1]>farthest)
                farthest = clips[i][1];
        }

        if(farthest==reach) return -1;

        reach = farthest;
        clips_required++;
    }

    return clips_required;
}
```


### 5. Jump Game VII
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
