# Problems based on Decision Making

### 1. Count Sorted Vowel Strings
Given an integer n, return the number of strings of length n that consist only of vowels (a, e, i, o, u) and are lexicographically sorted. A string s is lexicographically sorted if for all valid i, s[i] is the same as or comes before s[i+1] in the alphabet.

Input: n = 2
Output: 15 (["aa","ae","ai","ao","au","ee","ei","eo","eu","ii","io","iu","oo","ou","uu"])

```cpp
int solve (int n, int idx, int choices, vector<vector<int>> & dp){
    if(idx==n) return 1;

    if(dp[idx][choices]!=-1) return dp[idx][choices];

    int ans = 0;

    for(int i=choices; i<5; i++)
        ans += solve(n, idx+1, i, dp);

    return dp[idx][choices] = ans;
}

int countVowelStrings(int n) {
    vector<vector<int>> dp (n+1, vector<int> (5, -1));
    return solve (n, 0, 0, dp);        
}
```

### 2. Count Number of Teams
There are n soldiers standing in a line. Each soldier is assigned a unique rating value. You have to form a team of 3 soldiers amongst them under the following rules:
1. Choose 3 soldiers with index (i, j, k) with rating (rating[i], rating[j], rating[k]).
2. A team is valid if: (rating[i] < rating[j] < rating[k]) or (rating[i] > rating[j] > rating[k]) where (0 <= i < j < k < n).

Return the number of teams you can form given the conditions. (soldiers can be part of multiple teams).

Input: rating = [2,5,3,4,1]

Output: 3

```cpp

/* Here our solution depends on 3 factors i.e. idx, cnt, pattern so we'll create 3D DP */

int solve (vector<int> & rating, int idx, int cnt, int pattern, int prev, vector<vector<vector<int>>>& dp){
    if(cnt==3) return 1;

    if(dp[idx][cnt][pattern]!=-1) return dp[idx][cnt][pattern];

    int ans = 0;

    for(int i=idx; i<rating.size(); i++){

        /* pattern == 1 : we need to choose decreasing */

        if(pattern==1){
            if(rating[i]<prev)
                ans += solve (rating, i+1, cnt+1, 1, rating[i], dp);
        }

        /* pattern == 2 : we need to choose increasing */

        else if(pattern==2){
            if(rating[i]>prev)
                ans += solve (rating, i+1, cnt+1, 2, rating[i], dp);
        }

        /* pattern == 0 : else we have both options to choose */

        else{
            ans += solve (rating, i+1, 1, 1, rating[i], dp);
            ans += solve (rating, i+1, 1, 2, rating[i], dp);
        }

    }

    return dp[idx][cnt][pattern] = ans;
}


int numTeams(vector<int>& rating) {
    int n = rating.size();
    vector<vector<vector<int>>> dp (n+1, vector<vector<int>> (3, vector<int> (3, -1)));
    return solve (rating, 0, 0, 0, 0, dp);
}
```

### 3. Integer Break
Given an integer n, break it into the sum of k positive integers, where k >= 2, and maximize the product of those integers. Return the maximum product you can get.

Input: n = 2

Output: 1

Explanation: 2 = 1 + 1, 1 × 1 = 1.

Hint: Break it into two parts and solve recursively everytime

```cpp
int solve (int n, vector<int> & dp){
    if(n==2) return 1;

    if(dp[n]!=-1) return dp[n];

    int ans = 0;

    for(int i=1; i<n; i++){
        int temp = i * max(n-i, solve(n-i, dp));
        ans = max(ans, temp);
    }

    return dp[n] = ans;
}


int integerBreak(int n) {
    vector<int> dp (n+1, -1);
    return solve(n, dp);
}
```


### 4. Minimum Cost For Tickets
You have planned some train traveling one year in advance. The days of the year in which you will travel are given as an integer array days. Each day is an integer from 1 to 365. Train tickets are sold in three different ways:

1. a 1-day pass is sold for costs[0] dollars,
2. a 7-day pass is sold for costs[1] dollars, and
3. a 30-day pass is sold for costs[2] dollars.

For example, if we get a 7-day pass on day 2, then we can travel for 7 days: 2, 3, 4, 5, 6, 7, and 8. Return the minimum number of dollars you need to travel every day in the given list of days.

Input: days = [1,4,6,7,8,20], costs = [2,7,15]

Output: 11

```cpp
int solve (vector<int>& days, vector<int>& costs, int idx, int n, int validity, vector<int>& dp){
    if(idx==n) return 0;

    if(days[idx]<=validity)
        return solve (days, costs, idx+1, n, validity, dp);

    if(dp[idx]!=-1) return dp[idx];

    /* choice 1 : 1-day pass */

    int c1 = costs[0] + solve (days, costs, idx+1, n, days[idx], dp);

    /* choice 2 : 7-day pass */

    int c2 = costs[1] + solve (days, costs, idx+1, n, days[idx]+6, dp);

    /* choice 3 : 30-day pass */

    int c3 = costs[2] + solve (days, costs, idx+1, n, days[idx]+29, dp);

    int cost = min(min(c1, c2), c3);
    return dp[idx] = cost;
}


int mincostTickets(vector<int>& days, vector<int>& costs) {
    int n = days.size();
    vector<int> dp (n+1, -1);

    return solve (days, costs, 0, n, 0, dp);
}
```

### 5. Best Time to Buy and Sell Stock with Cooldown
You are given an array prices where prices[i] is the price of a given stock on the ith day. Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions: After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).

Input: prices = [1,2,3,0,2]

Output: 3

```cpp
int solve (vector<int> & prices, int idx, int n, bool buy, vector<vector<int>> & dp){
    if(idx>=n) return 0;

    if(dp[idx][buy]!=-1) return dp[idx][buy];

    if(buy){
        int p1 = solve(prices, idx+1, n, !buy, dp) - prices[idx];       // --> Buy
        int p2 = solve(prices, idx+1, n, buy, dp);                      // --> No Buy

        return dp[idx][buy] = max(p1, p2);
    }
    else{
        int p1 = solve(prices, idx+2, n, !buy, dp) + prices[idx];       // --> Sell
        int p2 = solve(prices, idx+1, n, buy, dp);                      // --> No sell

        return dp[idx][buy] = max(p1, p2);
    }

}

int maxProfit(vector<int>& prices) {
    int n = prices.size();
    vector<vector<int>> dp (n+1, vector<int> (2, -1));

    return solve(prices, 0, n, true, dp);
}
```

### 6. Best Time to Buy and Sell Stock with Transaction Fee
You are given an array prices where prices[i] is the price of a given stock on the ith day, and an integer fee representing a transaction fee. Find the maximum profit you can achieve. You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction.

Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

Input: prices = [1,3,2,8,4,9], fee = 2

Output: 8

```cpp
int solve (vector<int> & prices, int n, int idx, int fee, bool buy, vector<vector<int>> & dp){
    if(idx==n) return 0;

    if(dp[idx][buy]!=-1) return dp[idx][buy];

    if(buy){
        int p1 = solve (prices, n, idx+1, fee, !buy, dp) - fee - prices[idx];       // --> Choice 1 : buy at current price
        int p2 = solve (prices, n, idx+1, fee, buy, dp);                            // --> Choice 2 : Don't buy at current price

        return dp[idx][buy] = max(p1, p2);
    }
    else{
        int p1 = solve (prices, n, idx+1, fee, !buy, dp) + prices[idx];     // --> Choice 1 : Sell at current price
        int p2 = solve (prices, n, idx+1, fee, buy, dp);                    // --> Choice 2 : Don't sell at current price

        return dp[idx][buy] = max(p1, p2);
    }
}


int maxProfit(vector<int>& prices, int fee) {
    int n = prices.size();
    vector<vector<int>> dp (n+1, vector<int> (2, -1));

    return solve (prices, n, 0, fee, true, dp);    
}
```

### 7. Largest Sum of Averages
You are given an integer array nums and an integer k. You can partition the array into at most k non-empty adjacent subarrays. The score of a partition is the sum of the averages of each subarray. Note that the partition must use every integer in nums, and that the score is not necessarily an integer. Return the maximum score you can achieve of all the possible partitions.

Input: nums = [9,1,2,3,9], k = 3

Output: 20.00000

```cpp
double solve (vector<int>& nums, int k, int idx, int n, vector<vector<double>> & dp){

    if(dp[idx][k]!=-1) return dp[idx][k];

    /* Base condition is when we want no further partition */

    if(k==1){
        double sum = 0;
        for(int i=idx; i<n; i++)
            sum += nums[i];

        return sum/(n-idx);
    }

    double ans = 0;
    double sum = 0;
    int cnt = 0;

    for(int i=idx; i<=n-k; i++){
        sum += nums[i];
        cnt++;

        double temp = sum/cnt + solve (nums, k-1, i+1, n, dp);
        ans = max(ans, temp);
    }

    return dp[idx][k] = ans;
}


double largestSumOfAverages(vector<int>& nums, int k) {
    int n = nums.size();
    vector<vector<double>> dp (n+1, vector<double> (k+1, -1));

    return solve (nums, k, 0, n, dp);
}
```

### 8. 2 Keys Keyboard
There is only one character 'A' on the screen of a notepad. You can perform two operations on this notepad for each step:
1. Copy All: You can copy all the characters present on the screen (a partial copy is not allowed).
2. Paste: You can paste the characters which are copied last time.

Given an integer n, return the minimum number of operations to get the character 'A' exactly n times on the screen.

```cpp
pair<bool,int> solve (int curr_count, int n, int buffer_count, vector<vector<pair<bool,int>>> & dp){
    if(curr_count>n) return {false, 0};
    if(curr_count==n) return {true, 0};

    if(dp[curr_count][buffer_count].second!=-1) return dp[curr_count][buffer_count];

    pair<bool, int> copy = {false, 0};
    pair<bool, int> paste = {false, 0};

    /* If number of A's on screen is same as number of A's in buffer, then no need to copy same thing again */

    if(curr_count!=buffer_count)
        copy = solve (curr_count, n, curr_count, dp);

    /* If buffer count == 0 then we have nothing to paste on screen */

    if(buffer_count > 0)
        paste = solve (curr_count + buffer_count, n, buffer_count, dp);

    if(copy.first and paste.first)
        return dp[curr_count][buffer_count] = {true,min(copy.second, paste.second)+1};

    else if(copy.first)
        return dp[curr_count][buffer_count] = {true,copy.second+1};

    else if(paste.first) 
        return dp[curr_count][buffer_count] = {true,paste.second+1};

    else return dp[curr_count][buffer_count] = {false, 0};
}


int minSteps(int n) {
    vector<vector<pair<bool,int>>> dp (2*n, vector<pair<bool,int>> (2*n, {false, -1}));

    auto res = solve (1, n, 0, dp);
    return res.second;
}
```

### 9. Greatest Sum Divisible by Three
Given an array nums of integers, we need to find the maximum possible sum of elements of the array such that it is divisible by three.

Input: nums = [3,6,5,1,8]

Output: 18

```cpp
pair<bool, int> solve (vector<int>&nums, int idx, int rem, vector<vector<pair<bool,int>>> & dp){
    if(idx==nums.size()){
        if(rem==0) return {true, 0};
        else return {false, 0};
    }

    if(dp[idx][rem].second!=-1) return dp[idx][rem];

    auto res1 = solve(nums, idx+1, (rem+nums[idx])%3, dp);          // --> Including idx
    auto res2 = solve(nums, idx+1, rem, dp);                        // --> Not including idx

    if(res1.first and res2.first)
        return dp[idx][rem] = {true, max(nums[idx]+res1.second, res2.second)};

    else if(res1.first)
        return dp[idx][rem] = {true, nums[idx]+res1.second};

    else if(res2.first)
        return dp[idx][rem] = {true, res2.second};

    else return dp[idx][rem] = {false, 0};
}

int maxSumDivThree(vector<int>& nums) {
    int n = nums.size();
    vector<vector<pair<bool,int>>> dp (n, vector<pair<bool,int>> (3, {true, -1}));

    auto res = solve (nums, 0, 0, dp);

    if(res.first) return res.second;
    else return 0;
}
```

### 10. Number of Dice Rolls With Target Sum
You have d dice and each die has f faces numbered 1, 2, ..., f. Return the number of possible ways (out of fd total ways) modulo 109 + 7 to roll the dice so the sum of the face-up numbers equals target.

Input: d = 2, f = 6, target = 7

Output: 6

```cpp
int solve (int dice_count, int d, int f, int sum, int target, vector<vector<int>> & dp){
    if(sum>target) return 0;

    if(dice_count==d){  
        if(sum==target) return 1;
        else return 0;
    } 

    if(dp[dice_count][sum]!=-1) 
        return dp[dice_count][sum];

    int ans = 0;

    for(int num=1; num<=f; num++){
        int temp = solve (dice_count+1, d, f, sum+num, target, dp);
        ans = (ans % 1000000007 + temp % 1000000007) % 1000000007;
    }

    return dp[dice_count][sum] = ans;
}


int numRollsToTarget(int d, int f, int target) {
    vector<vector<int>> dp (d+1, vector<int> (target+1, -1));
    return solve (0, d, f, 0, target, dp);
}
```

### 11. Combination Sum IV (basically permutation sum)
Given an array of distinct integers nums and a target integer target, return the number of possible combinations that add up to target. Note that different sequences are counted as different combinations.
```
Input: nums = [1,2,3], target = 4
Output: 7

Explanation:
The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
```

```cpp
int solve (vector<int> & nums, int target, vector<int> & dp){
    if(target==0) return 1;

    if(dp[target]!=-1) return dp[target];

    int ans = 0;
    
    /* Now we can choose any element in this place (Similar to permutations) */
    
    for(int i=0; i<nums.size(); i++){
        if(nums[i]<=target)        
            ans += solve(nums, target-nums[i], dp);
    }

    return dp[target] = ans;
}

int combinationSum4(vector<int>& nums, int target) {
    vector<int> dp (target+1, -1);
    return solve (nums, target, dp);
}
```

### 12. Find Two Non-overlapping Sub-arrays Each With Target Sum
Given an array of integers arr and an integer target. You have to find two non-overlapping sub-arrays of arr each with a sum equal target. Return the minimum sum of the lengths of the two required sub-arrays, or return -1 if you cannot find such two sub-arrays.

Input: arr = [3,1,1,1,5,1,2,1], target = 3

Output: 3

**Approach 1 : Memorization**

```cpp
/* Memorization for finding two intervals (representing end points of subarrays) with min total sum */

int solve (vector<pair<int,int>> subarr, int idx, int n, int cnt, int prev, unordered_map<string,int> & dp){
    if(cnt==2) return 0;
    if(idx==n) return -1;

    string key = to_string(idx) + "," + to_string(cnt) + "," + to_string(prev);
    if(dp.find(key)!=dp.end()) return dp[key];

    if(subarr[idx].first>prev){

        int res1 = solve (subarr, idx+1, n, cnt+1, subarr[idx].second, dp); 
        int res2 = solve (subarr, idx+1, n, cnt, prev, dp);

        if(res1==-1 and res2==-1) 
            return dp[key] = -1;

        else if(res1==-1) 
            return dp[key] = res2;

        else if(res2==-1)
            return dp[key] = res1 + subarr[idx].second - subarr[idx].first + 1;

        else  
            return dp[key] = min(res1 + subarr[idx].second - subarr[idx].first + 1, res2);
    }

    else{
        int res = solve (subarr, idx+1, n, cnt, prev, dp);
        return dp[key] = res;
    }

}


int minSumOfLengths(vector<int>& arr, int target) {
    int n = arr.size();

    unordered_map<int,int> mp;
    mp[0] = -1;

    int csum = 0;

    /* for avoiding full overlapping, next subarr should start after first so prev will store the start of first subarr */

    int prev = INT_MIN;    
    vector<pair<int,int>> subarrs;

    for(int i=0; i<n; i++){
        csum += arr[i];
        int diff = csum - target;

        if(mp.find(diff)!=mp.end() and mp[diff]>prev){
            int subArr_len = i-mp[diff];
            subarrs.push_back({mp[diff]+1, i});         // --> insert endpoint of subarray in form of interval

            prev = mp[diff];       // --> update prev everytime when we find subarr
        }

        mp[csum] = i;
    }

    unordered_map<string,int> dp;
    return solve (subarrs, 0, subarrs.size(), 0, -1, dp);
}
```

**Approach 2 : Iterative DP**

```cpp
int solve (vector<pair<int,int>> & subarr, int n){
    int ans = INT_MAX;
    unordered_map<int,int> mp;                  // --> will store pair of {start, end}
    vector<int> dp (n+1, INT_MAX);              // --> will store minLength of subarrs on right of every index

    for(auto p : subarr)
        mp[p.first] = p.second;

    /* Fill dp by checking every index i in the map */

    for(int i=n-1; i>=0; i--){
        dp[i] = dp[i+1];

        if(mp.find(i)!=mp.end())
            dp[i] = min(dp[i], mp[i]-i+1);
    }

    /* Iterating start of every subarr and find minlen on its right in dp and update minSum of both */

    for(auto p : mp){
        int curr_subarr_len = p.second-p.first+1;

        if(dp[p.second+1]!=INT_MAX)
            ans = min (ans, curr_subarr_len + dp[p.second+1]);
    }

    return ans==INT_MAX ? -1 : ans;
}


int minSumOfLengths(vector<int>& arr, int target) {
    int n = arr.size();

    unordered_map<int,int> mp;
    mp[0] = -1;

    int csum = 0;
    int mn = n, mn2 = n;

    /* for avoiding full overlapping, next subarr should start after first so prev will store the start of first subarr */

    int prev = INT_MIN;    
    vector<pair<int,int>> subarrs;

    for(int i=0; i<n; i++){
        csum += arr[i];
        int diff = csum - target;

        if(mp.find(diff)!=mp.end() and mp[diff]>prev){
            int subArr_len = i-mp[diff];
            subarrs.push_back({mp[diff]+1, i});

            prev = mp[diff];       // --> update prev everytime when we find subarr
        }

        mp[csum] = i;
    }

    return solve (subarrs, n);
}
```

### 13. Valid Parenthesis String
Given a string s containing only three types of characters: '(', ')' and '*', return true if s is valid. The following rules define a valid string:
1. Any left parenthesis '(' must have a corresponding right parenthesis ')'.
2. Any right parenthesis ')' must have a corresponding left parenthesis '('.
3. Left parenthesis '(' must go before the corresponding right parenthesis ')'.
4. '*' could be treated as a single right parenthesis ')' or a single left parenthesis '(' or an empty string "".

Input: s = "(*))"

Output: true

```cpp
bool solve (string & s, int idx, int n, int br_cnt, vector<vector<int>> & dp){

    /* 
        Following are the two Base Conditions:
        1. If br_cnt<0 that means we got extra ')' bracket, hence return false
        2. If idx==n that means we reached at the end 
           so if br_cnt==0 that means string is balanced hence return true else return false
    */

    if(br_cnt<0) return false;
    if(idx==n) return br_cnt==0;

    if(dp[idx][br_cnt]!=-1) return dp[idx][br_cnt];

    /* If curr char is '(', we will increase the br_cnt and recurse */

    if(s[idx]=='(')
        return dp[idx][br_cnt] = solve (s, idx+1, n, br_cnt+1, dp);

    /* If curr char is ')', we will decrease the br_cnt and recurse */

    else if(s[idx]==')')
        return dp[idx][br_cnt] = solve (s, idx+1, n, br_cnt-1, dp);

    /* If curr char is '*', we have following 3 choices */

    else{
        bool res1 = solve(s, idx+1, n, br_cnt+1, dp);           // --> choice 1 : consider it as '('
        bool res2 = solve(s, idx+1, n, br_cnt-1, dp);           // --> choice 2 : consider it as ')'
        bool res3 = solve(s, idx+1, n, br_cnt, dp);             // --> choice 3 : consider it as ''

        return dp[idx][br_cnt] = res1 or res2 or res3;
    }
}

bool checkValidString(string s) {
    int n = s.length();
    vector<vector<int>> dp (n, vector<int> (n, -1));
    return solve (s, 0, n, 0, dp);
}
```

### 14. Decode Ways
To decode an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example, "11106" can be mapped into:
1. "AAJF" with the grouping (1 1 10 6)
2. "KJF" with the grouping (11 10 6)

Note that the grouping (1 11 06) is invalid because "06" cannot be mapped into 'F' since "6" is different from "06". Given a string s containing only digits, return the number of ways to decode it.

Input: s = "226"

Output: 3

```cpp
int solve (string & s, int idx, int n, vector<int> & dp){
    if(idx==n) return 1;
    if(s[idx]=='0') return 0;       // --> Single 0 can't exist so it will become invalid case

    if(dp[idx]!=-1) return dp[idx];

    int ans = 0;

    /* If curr char is 1 we have two choices either to consider one char or consider two at a time */

    if(s[idx]=='1'){
        ans += solve(s, idx+1, n, dp);

        if(idx!=n-1)
            ans += solve(s, idx+2, n, dp);            
    }

    /* If curr char is 2 then also we have two choices if next char is less then 6 */

    else if(s[idx]=='2'){
        ans += solve(s, idx+1, n, dp);

        if(idx!=n-1 and s[idx+1] <= '6')
            ans += solve(s, idx+2, n, dp);
    }

    /* Else we have only one choice */

    else ans += solve (s, idx+1, n, dp);

    return dp[idx] = ans;
}


int numDecodings(string s) {
    int n = s.length();
    vector<int> dp (n, -1);
    return solve (s, 0, n, dp);
}
```

### 15. Minimum Difficulty of a Job Schedule
You want to schedule a list of jobs in d days. Jobs are dependent (i.e To work on the i-th job, you have to finish all the jobs j where 0 <= j < i). You have to finish at least one task every day. The difficulty of a job schedule is the sum of difficulties of each day of the d days. The difficulty of a day is the maximum difficulty of a job done in that day.

Given an array of integers jobDifficulty and an integer d. The difficulty of the i-th job is jobDifficulty[i]. Return the minimum difficulty of a job schedule. If you cannot find a schedule for the jobs return -1.

Input: jobDifficulty = [7,1,7,1,7,1], d = 3

Output: 15

```cpp
int solve (vector<int> & jobs, int n, int idx, int d, vector<vector<int>> & dp){
    if(idx==n) return 0;

    if(dp[idx][d]!=-1) return dp[idx][d];

    if(d==1) return 
        *max_element(jobs.begin()+idx, jobs.end());

    int mx = 0;
    int ans = INT_MAX;

    for(int i=idx; i<=n-d; i++){

        mx = max(mx, jobs[i]);
        int res = solve (jobs, n, i+1, d-1, dp);

        ans = min(ans, res+mx);
    }

    return dp[idx][d] = ans;
}


int minDifficulty(vector<int>& jobDifficulty, int d) {
    int n = jobDifficulty.size();
    if(n<d) return -1;

    vector<vector<int>> dp (n, vector<int> (d+1, -1));

    return solve (jobDifficulty, n, 0, d, dp);
}
```


### 16. Allocate Mailboxes (Tricky)
Given the array houses and an integer k. where houses[i] is the location of the ith house along a street, your task is to allocate k mailboxes in the street. Return the minimum total distance between each house and its nearest mailbox.

![img](https://assets.leetcode.com/uploads/2020/05/07/sample_11_1816.png)

Input: houses = [1,4,8,10,20], k = 3

Output: 5

Hint: Mailbox should be placed at Median house so as to minimize the cost.

```cpp

/* 
    Problem Breakup
    ---------------

    We need to divide whole array into k subarrays such that every subarray is a cluster
    of closely situated houses having 1 mailbox between them

    Now For placing 1 mailbox in a subarray, 
    we should place it at the median house to minimize the whole cost

    Hint: Precompute one cost matrix for stroring cost values of placing mailboxes 
    at median of every possible subarray (or interval)

*/


/* Use partion into k subarrays logic for placing mailboxes and use cost of placing from cost matrix */

int solve (vector<int> & houses, int n, int idx, int k, vector<vector<int>> & cost, vector<vector<int>> & dp){
    if(k==1) return cost[idx][n-1];

    if(dp[idx][k]!=-1) return dp[idx][k];

    int ans = INT_MAX;

    /* We can use all k mailboxes so we will check cost till n-k+1 houses for placing curr mailbox */ 

    for(int i=idx; i<n-k+1; i++){
        int temp = cost[idx][i] + solve (houses, n, i+1, k-1, cost, dp);
        ans = min(ans, temp);
    }

    return dp[idx][k] = ans;
}


int minDistance(vector<int>& houses, int k) {
    int n = houses.size();     
    if(k==n) return 0;

    sort(houses.begin(), houses.end());

    vector<vector<int>> cost (n, vector<int> (n, 0));

    /* Filling cost of placing mail box in median point of every interval in cost matrix */

    for(int i=0; i<n; i++){
        for(int j=i; j<n; j++){

            int median = i+(j-i+1)/2;
            int mailbox = houses[median];
            int c = 0;

            for(int k=i; k<=j; k++)
                c += abs(houses[k]-mailbox);    

            cost[i][j] = c;
        }
    }

    vector<vector<int>> dp (n+1, vector<int> (k+1, -1));    // --> For avoiding overlapping computations
    return solve (houses, n, 0, k, cost, dp);
}
```

