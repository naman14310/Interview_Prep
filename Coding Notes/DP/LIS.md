# Problems on LIS Pattern

**Identification:** 
Whenever there is a need to use prev variable in recursive code so that next element should be either greater or smaller then that prev variable then that question can be solved by using LIS approach.

<br>

### 1. Longest Increasing Subsequence
Given an integer array nums, return the length of the longest strictly increasing subsequence.

**Approach 1 : Dynamic Programming | O(n2)**

```cpp
int lengthOfLIS(vector<int>& nums) {
    int n = nums.size();
    vector<int> dp (n, 1);

    int ans = 1;

    for(int i=1; i<n; i++){
        for(int j=0; j<i; j++){

            if(nums[j]<nums[i])
                dp[i] = max(dp[i], dp[j]+1);

            ans = max(ans, dp[i]);
        }
    }

    return ans;
}
```

**Approach 2 : Binary Search (lower_bound) | O(nlogn)**

Note: This will only give correct length of LIS

```cpp
int lengthOfLIS(vector<int>& nums) {
    vector<int> v;

    for(int num : nums){
    
        if(v.empty() or num>v.back())
            v.push_back(num);

        else{
            int idx = lower_bound(v.begin(), v.end(), num) - v.begin();
            v[idx] = num;
        }
    }

    return v.size();
}
```

<br>

### 2. Number of Longest Increasing Subsequence
Given an integer array nums, return the number of longest increasing subsequences.

Input: nums = [1,3,5,4,7]

Output: 2

Hint: Create one count array for storing count of lis so far at every index.

```cpp
int findNumberOfLIS(vector<int>& nums) {
    int n = nums.size();
    vector<int> dp (n, 1);
    vector<int> count (n, 1);

    int lis = 1;

    for(int i=1; i<n; i++){
        for(int j=0; j<i; j++){

            if(nums[i]>nums[j]){

                if(dp[j]+1 > dp[i]){
                    count[i] = count[j];
                    dp[i] = dp[j]+1;
                }
                else if(dp[j]+1 == dp[i])
                    count[i] += count[j];

            }

            lis = max(lis, dp[i]);
        }
    }

    int ans = 0;

    for(int i=0; i<n; i++){
        if(dp[i]==lis)
            ans += count[i];
    }

    return ans;
}
```

<br>

### 3. Longest Bitonic Subsequence
Given an 1D integer array A of length N, find the length of longest subsequence which is first increasing then decreasing.

Input: A = [1, 11, 2, 10, 4, 5, 2, 1]
 
Output: 6

```cpp
int Solution::longestSubsequenceLength(const vector<int> &A) {
    int n = A.size();
    if(n==0) return 0;

    vector<int> dp1 (n, 1);      
    vector<int> dp2 (n, 1);
    int ans = 1;

    /* computing lis in forward direction */

    for(int j=1; j<n; j++){
        for(int i=0; i<j; i++){
            if(A[i]<A[j]) dp1[j] = max(dp1[j], dp1[i]+1);
        }
    }

    /* computing lis in backward direction */

    for(int i=n-2; i>=0; i--){
        for(int j=i+1; j<n; j++){
            if(A[j]<A[i]) dp2[i] = max(dp2[i], dp2[j]+1);
        }
    }    

    /* ans will be max of dp1[i] + dp2[i] - 1 for every index i*/

    for(int i=0; i<n; i++)
        ans = max(ans, dp1[i]+dp2[i]-1);
    
    return ans;
}

```

<br>

### 4. Longest Arithmetic Subsequence
Given an array nums of integers, return the length of the longest arithmetic subsequence in nums. Recall that a sequence seq is arithmetic if seq[i+1] - seq[i] are all the same value (for 0 <= i < seq.length - 1).

Input: nums = [9,4,7,2,10]

Output: 3

```cpp
int longestArithSeqLength(vector<int>& nums) {
    int n = nums.size();
    int ans = 2;

    /* vector of unordered_map will contain all possible diff of curr_nums with max_len so far */

    vector<unordered_map<int,int>> dp (n, unordered_map<int,int>());

    for(int i=1; i<n; i++){
        for(int j=0; j<i; j++){
            int diff = nums[i]-nums[j];

            if(dp[j].find(diff)!=dp[j].end())
                dp[i][diff] = dp[j][diff]+1;
            else
                dp[i][diff] = 2;

            ans = max(ans, dp[i][diff]);
        }
    }   

    return ans;
}
```

<br>

### 5. Length of Longest Fibonacci Subsequence
A sequence x1, x2, ..., xn is Fibonacci-like if:
1. n >= 3
2. xi + xi+1 == xi+2 for all i + 2 <= n

Given a strictly increasing array arr of positive integers forming a sequence, return the length of the longest Fibonacci-like subsequence of arr. If one does not exist, return 0.

Input: arr = [1,3,7,11,12,14,18]

Output: 3

```cpp
int lenLongestFibSubseq(vector<int>& arr) {
    int n = arr.size();
    int ans = 2;
    vector<unordered_map<int,int>> dp (n, unordered_map<int,int>());

    for(int i=1; i<n; i++){
        for(int j=0; j<i; j++){

            int diff = arr[i] - arr[j];

            if(dp[j].find(diff)!=dp[j].end())
                dp[i][arr[j]] = dp[j][diff]+1;
            else
                dp[i][arr[j]] = 2;

            ans = max(ans, dp[i][arr[j]]);
        }
    }

    return ans==2 ? 0 : ans;
}
```

<br>

### 6. Best Team With No Conflicts
A conflict exists if a younger player has a strictly higher score than an older player. A conflict does not occur between players of the same age. Given two lists, scores and ages, where each scores[i] and ages[i] represents the score and age of the ith player, respectively, return the highest overall score of all possible basketball teams.

Input: scores = [4,5,6,5], ages = [2,1,2,1]

Output: 16

**Approach 1 : Memorization**

```cpp
int solve (vector<pair<int,int>> & player, int minScore, int idx, int n, int prev, vector<vector<int>> & dp){
    if(idx==n) return 0;

    if(dp[idx][minScore]!=-1) return dp[idx][minScore];

    if(player[idx].second==prev or player[idx].first >= minScore){

        int res1 = solve(player, max(minScore, player[idx].first), idx+1, n, player[idx].second, dp) + player[idx].first; 
        int res2 = solve(player, minScore, idx+1, n, prev, dp);

        return dp[idx][minScore] = max(res1, res2);
    }
    else{

        int res = solve(player, minScore, idx+1, n, prev, dp);
        return dp[idx][minScore] = res;
    }

}


/* sort by ages in ascending order and if ages are equal sort by their scores in ascending order */

bool static comp (pair<int,int> & a, pair<int,int> & b){
    if (a.second<b.second) return true;
    else if(a.second==b.second and a.first<b.first) return true;
    else return false;
}


int bestTeamScore(vector<int>& scores, vector<int>& ages) {
    int n = scores.size();
    vector<pair<int,int>> player;
    vector<vector<int>> dp (n, vector<int> (1000000, -1));

    for(int i=0; i<n; i++)
        player.push_back({scores[i], ages[i]});

    sort(player.begin(), player.end(), comp);

    return solve (player, 0, 0, n, 0, dp);
}
```

**Approach 2 : Iterative DP (LIS Pattern)**

```cpp
/* sort by ages in ascending order and if ages are equal sort by their scores in ascending order */

bool static comp (pair<int,int> & a, pair<int,int> & b){
    if (a.second<b.second) return true;
    else if(a.second==b.second and a.first<b.first) return true;
    else return false;
}


int bestTeamScore(vector<int>& scores, vector<int>& ages) {
    int n = scores.size();
    vector<pair<int,int>> player;

    for(int i=0; i<n; i++)
        player.push_back({scores[i], ages[i]});

    sort(player.begin(), player.end(), comp);

    vector<int> dp (n, 0);
    for(int i=0; i<n; i++)
        dp[i] = player[i].first;

    int ans = dp[0];

    for(int i=1; i<n; i++){
        for(int j=0; j<i; j++){

            /* If score of curr_player is greater then score of prev_player then we can use that dp */

            if(player[i].first>=player[j].first)
                dp[i] = max(dp[i], dp[j]+player[i].first);

            ans = max(ans, dp[i]);
        }
    }

    return ans;
}
```

<br>

### 7. Russian Doll Envelopes
You are given a 2D array of integers envelopes where envelopes[i] = [wi, hi] represents the width and the height of an envelope. One envelope can fit into another if and only if both the width and height of one envelope are greater than the other envelope's width and height. Return the maximum number of envelopes you can Russian doll (i.e., put one inside the other). You cannot rotate an envelope.

Input: envelopes = [[5,4],[6,4],[6,7],[2,3]]

Output: 3

```cpp
int maxEnvelopes(vector<vector<int>>& envelopes) {
    int n = envelopes.size();
    vector<pair<int,int>> evp;

    for(auto e : envelopes)
        evp.push_back({e[0], e[1]});

    /* Sort above vector by weight and height in ascending order */

    sort(evp.begin(), evp.end());

    int ans = 1;
    vector<int> dp (n, 1);

    for(int i=1; i<n; i++){
        for(int j=0; j<i; j++){

            if(evp[j].first<evp[i].first and evp[j].second<evp[i].second)
                dp[i] = max(dp[i], dp[j]+1);

            ans = max(ans, dp[i]);
        }
    }

    return ans;
}
```

<br>

### 8. Largest Divisible Subset (Tricky)
Given a set of distinct positive integers nums, return the largest subset answer such that every pair (answer[i], answer[j]) of elements in this subset satisfies:
1. answer[i] % answer[j] == 0, or
2. answer[j] % answer[i] == 0

Input: nums = [1,2,3,6,8,12,16,20,32]

Output: [1,2,8,16,32]

**Approach 1 : Memorization** (will give TLE)

```cpp
vector<int> solve (vector<int> & nums, int idx, int n, int prev, unordered_map<string,vector<int>> & dp){
    if(idx==n) return {};

    string key = to_string(idx) + "," + to_string(prev);
    if(dp.find(key)!=dp.end()) return dp[key];

    /* If curr number is divisible by prev (which is max so far) then we have a choice to add it in curr subset */ 

    if(nums[idx] % prev==0){

        vector<int> res1 = solve (nums, idx+1, n, nums[idx], dp); 
        vector<int> res2 = solve (nums, idx+1, n, prev, dp);

        if(res1.size()+1 > res2.size()){
            res1.insert(res1.begin(), nums[idx]);
            return dp[key] = res1;
        }
        else return dp[key] = res2;
    }

    /* Else we cannot add curr number to subset */

    else{
        vector<int> res = solve (nums, idx+1, n, prev, dp);
        return dp[key] = res;
    }
}

vector<int> largestDivisibleSubset(vector<int>& nums) {
    int n = nums.size();
    sort(nums.begin(), nums.end());            // --> we will do sorting to arrange all numbers in ascending order

    unordered_map<string, vector<int>> dp;
    return solve (nums, 0, n, 1, dp);
}
```

**Approach 2 : Iterative DP (LIS Pattern)**

```cpp
vector<int> largestDivisibleSubset(vector<int>& nums) {
    int n = nums.size();
    sort(nums.begin(), nums.end());

    int lis = 1, lis_idx = 0;

    vector<int> dp (n, 1);              // --> will store lis till every index
    vector<int> prev (n, -1);           // --> will store index of prev element of every index according to lis 

    for(int i=1; i<n; i++){
        for(int j=0; j<i; j++){

            /* If ith element is divisible by jth element and also dp[j]+1 > dp[i] then we will update dp as well as prev idx */

            if(nums[i]%nums[j]==0 and dp[j]+1 > dp[i]){
                dp[i] = dp[j]+1;
                prev[i] = j;
            }

            /* Everytime when we found greater lis, we will update lis_idx to keep track of last index of lis */ 

            if(dp[i]>lis){
                lis = dp[i];
                lis_idx = i;
            }
        }
    }

    vector<int> res;

    /* Simple go backward from last index of lis using prev vector */

    while(lis_idx>=0){
        res.push_back(nums[lis_idx]);
        lis_idx = prev[lis_idx];
    }

    return res;
}
```

<br>

### 9. Maximum Profit in Job Scheduling (Tricky)
We have n jobs, where every job is scheduled to be done from startTime[i] to endTime[i], obtaining a profit of profit[i]. You're given the startTime, endTime and profit arrays, return the maximum profit you can take such that there are no two jobs in the subset with overlapping time range. If you choose a job that ends at time X you will be able to start another job that starts at time X.

![img](https://assets.leetcode.com/uploads/2019/10/10/sample22_1584.png)

Input: startTime = [1,2,3,4,6], endTime = [3,5,10,6,9], profit = [20,20,100,70,60]

Output: 150

**Approach 1 : Memorization**

```cpp
struct job{
    int stime, etime, profit; 
    job(int st, int et, int p){
        stime = st; etime = et;
        profit = p;
    }
};


static bool comp(job j1, job j2){
    return j1.stime < j2.stime; 
}


int solve (vector<job> & jobs, int n, int idx, int prev, unordered_map<string, int> & dp){
    if(idx==n) return 0;

    string key = to_string(idx) + "," + to_string(prev);
    if(dp.find(key)!=dp.end()) return dp[key];

    /* If stime of current job is >= prev (etime of prev job) then we can either choose it or leave it */

    if(jobs[idx].stime >= prev){

        int p1 = solve (jobs, n, idx+1, jobs[idx].etime, dp) + jobs[idx].profit;
        int p2 = solve (jobs, n, idx+1, prev, dp);

        return dp[key] = max(p1, p2);
    }

    /* Else we can't choose the current job due to overlapping interval */

    else{
        int p = solve (jobs, n, idx+1, prev, dp);
        return dp[key] = p;
    }
}


int jobScheduling(vector<int>& startTime, vector<int>& endTime, vector<int>& profit) {
    int n = profit.size();
    vector<job> jobs;

    for(int i=0; i<n; i++){
        job jb (startTime[i], endTime[i], profit[i]);
        jobs.push_back(jb);
    }

    /* Sort jobs according to their starting time and then use memoization to find subset that yeild best profit */

    sort(jobs.begin(), jobs.end(), comp);

    unordered_map<string, int> dp;
    return solve (jobs, n, 0, 0, dp);
}
```

**Approach 2 : Iterative LIS Pattern**

```cpp
struct job{
    int stime, etime, profit; 
    job(int st, int et, int p){
        stime = st; etime = et;
        profit = p;
    }
};


static bool comp(job j1, job j2){
    return j1.etime < j2.etime; 
}


int jobScheduling(vector<int>& startTime, vector<int>& endTime, vector<int>& profit) {
    int n = profit.size();
    vector<job> jobs;

    for(int i=0; i<n; i++){
        job jb (startTime[i], endTime[i], profit[i]);
        jobs.push_back(jb);
    }

    /* Sort jobs according to their ending time */

    sort(jobs.begin(), jobs.end(), comp);

    vector<int> dp (n, 0);

    /* 
        Fill dp vector by profit of each individual jobs 
        (this will indicate profit of each job when they are the only one to be executed)
    */

    for(int i=0; i<n; i++)
        dp[i] = jobs[i].profit;

    int ans = dp[0];

    /* Simply use LIS pattern to find the max profit that can be achieved before executing curr job */

    for(int i=1; i<n; i++){
        for(int j=0; j<i; j++){

            if(jobs[j].etime <= jobs[i].stime)
                dp[i] = max(dp[i], jobs[i].profit+dp[j]);

            ans = max(ans, dp[i]); 
        }
    }

    return ans;
}
```

**Approach 3 : DP + Binary Search**

Hint: Calculate maxprofit so far for every endTime and and run binary search to find maxprofit before starting curr Job

```cpp
struct job{
    int stime, etime, profit; 
    job(int st, int et, int p){
        stime = st; etime = et;
        profit = p;
    }
};


static bool comp(job j1, job j2){
    return j1.etime < j2.etime; 
}


int jobScheduling(vector<int>& startTime, vector<int>& endTime, vector<int>& profit) {
    int n = profit.size();
    vector<job> jobs;

    for(int i=0; i<n; i++){
        job jb (startTime[i], endTime[i], profit[i]);
        jobs.push_back(jb);
    }

    /* Sort jobs according to their ending time */

    sort(jobs.begin(), jobs.end(), comp);

    vector<pair<int,int>> dp;       // --> vector of pair {endTime, profit}

    dp.push_back({jobs[0].etime, jobs[0].profit});

    /* Since jobs vector is sorted by endtime, we can use binary search to find max profit so far */

    for(int i=1; i<n; i++){

        /* 
            start and end defines the range of binary search as we need to find max profit 
            before the start time of current job
        */

        int start = 0, end = i-1;
        int maxProfit = 0;

        while(start<=end){
            int mid = start + (end-start)/2;

            if(dp[mid].first <= jobs[i].stime){
                maxProfit = dp[mid].second;
                start = mid+1;
            }
            else
                end = mid-1;
        }

        /* Max profit =  max (profit including this job, profit with incuding this job so far) */

        maxProfit = max({maxProfit+jobs[i].profit, dp.back().second});

        dp.push_back({jobs[i].etime, maxProfit});
    }

    return dp.back().second;
}
```
