# Problems on Decision Making

<br>

### 1. Climbing Stairs
You are climbing a staircase. It takes n steps to reach the top. Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

```cpp
int solve (int n, vector<int> & dp){
    if(n<0) return 0;
    if(n==0) return 1;

    if(dp[n]!=-1) 
        return dp[n];

    return dp[n] = solve(n-1, dp) + solve(n-2, dp);
}

int climbStairs(int n) {
    vector<int> dp (n+1, -1);
    return solve (n, dp);
}
```

<br>

### 2. Min Cost Climbing Stairs
You are given an integer array cost where cost[i] is the cost of ith step on a staircase. Once you pay the cost, you can either climb one or two steps. You can either start from the step with index 0, or the step with index 1. Return the minimum cost to reach the top of the floor.

Input: cost = [10,15,20]

Output: 15

```cpp
int solve (vector<int> & cost, int idx, vector<int> & dp){
    if(idx>=0 and idx>=cost.size()) return 0;
    if(idx>=0 and dp[idx]!=-1) return dp[idx];

    int res1 = solve (cost, idx+1, dp);
    int res2 = solve (cost, idx+2, dp);

    int ans = min(res1, res2);
    ans += idx==-1 ? 0 : cost[idx];

    if(idx>=0) dp[idx] = ans;

    return ans;
}


int minCostClimbingStairs(vector<int>& cost) {
    vector<int> dp (cost.size(), -1);
    return solve (cost, -1, dp);
}
```

<br>

### 3. Count Sorted Vowel Strings
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

<br>

### 4. Integer Break (Tricky)
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
        int temp = i * max(n-i, solve(n-i, dp));        // --> max of (if we don't break (n-i), if we break it further)
        ans = max(ans, temp);
    }

    return dp[n] = ans;
}


int integerBreak(int n) {
    vector<int> dp (n+1, -1);
    return solve(n, dp);
}
```

<br>

### 5. Minimum Cost For Tickets
You have planned some train traveling one year in advance. The days of the year in which you will travel are given as an integer array days. Each day is an integer from 1 to 365. Train tickets are sold in three different ways:
1. a 1-day pass is sold for costs[0] dollars
2. a 7-day pass is sold for costs[1] dollars
3. a 30-day pass is sold for costs[2] dollars

For eg, if we get a 7-day pass on day 2, then we can travel for 7 days: 2, 3, 4, 5, 6, 7, and 8. Return the minimum number of dollars you need to travel every day in the given list of days.

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

<br> 

### 6. Paint House
There are a row of N houses, each house can be painted with one of the three colors: red, blue or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a N x 3 cost matrix A. Find the minimum total cost to paint all houses.

```cpp

int mincost (vector<vector<int>> & A, int idx, int prevColor, unordered_map<string, int> &dp){
    if(idx==A.size()) return 0;

    string key = to_string(idx) + "," + to_string(prevColor);
    if(dp.find(key)!=dp.end()) return dp[key];

    int cost = INT_MAX;

    if(prevColor!=0)
        cost = min(cost, A[idx][0] + mincost(A, idx+1, 0, dp));

    if(prevColor!=1)
        cost = min(cost, A[idx][1] + mincost(A, idx+1, 1, dp));

    if(prevColor!=2)
        cost = min(cost, A[idx][2] + mincost(A, idx+1, 2, dp));
    
    return dp[key] = cost;
}

int solve(vector<vector<int>> &A) {
    unordered_map<string, int> dp;
    return mincost (A, 0, -1, dp);
}
```

<br>

### 7. 2 Keys Keyboard
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

<br>

### 8. Number of Dice Rolls With Target Sum
You have d dice and each die has f faces numbered 1, 2, .., f. Return the number of possible ways (out of fd total ways) modulo 10^9 + 7 to roll the dice so the sum of the face-up numbers equals target.

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

<br>

### 9. Combination Sum IV (basically permutation sum)
Given an array of distinct integers nums and a target, return the number of possible combinations that add up to target. Note that different sequences are counted as different combinations.
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

<br>

### 10. Decode Ways
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

<br>

### 11. Allocate Mailboxes (Tricky)
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

    Hint: Precompute one cost matrix for storing cost values of placing mailboxes 
    at median of every possible subarray (or interval)

*/


/* Use partition into k subarrays logic for placing mailboxes and use cost of placing from cost matrix */

int solve (vector<int> &houses, int n, int idx, int k, vector<vector<int>> &cost, vector<vector<int>> &dp){
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

<br>

### 12. Frog Jump
A frog is crossing a river. The frog can jump on a stone, but it must not jump into the water. Given a list of stones' positions (in units) in sorted ascending order, determine if the frog can cross the river by landing on the last stone. Initially, the frog is on the first stone and assumes the first jump must be 1 unit. If the frog's last jump was k units, its next jump must be either k - 1, k, or k + 1 units. The frog can only jump in the forward direction.

Input: stones = [0,1,3,5,6,8,12,17]

Output: true

```cpp
bool solve (vector<int> & stones, int n, int idx, int prev, unordered_set<string> & vis){
    if(idx==n-1) return true;

    string key = to_string(idx) + "," + to_string(prev);
    if(vis.find(key)!=vis.end()) return false;

    for(int i=idx+1; i<n; i++){

        if(stones[i]-stones[idx]==prev-1){
            bool res = solve (stones, n, i, stones[i]-stones[idx], vis);
            if(res) return true;
        }

        else if(stones[i]-stones[idx]==prev){
            bool res = solve (stones, n, i, stones[i]-stones[idx], vis);
            if(res) return true;
        }

        else if(stones[i]-stones[idx]==prev+1){
            bool res = solve (stones, n, i, stones[i]-stones[idx], vis);
            if(res) return true;
        }

        else if(stones[i]-stones[idx]>prev+1)
            break;
    }

    vis.insert(key);
    return false;
}


bool canCross(vector<int>& stones) {
    int n = stones.size();
    if(n==1) return true;
    if(stones[1]!=1) return false;

    unordered_set<string> vis;
    return solve (stones, n, 1, 1, vis);
}
```

<br>

### 13. Maximum Score from Performing Multiplication Operations (Tricky)
You are given two integer arrays nums and multipliers of size n and m respectively, where n >= m. You begin with a score of 0. You want to perform exactly m operations. On the ith operation (1-indexed), you will:
1. Choose one integer x from either the start or the end of the array nums.
2. Add multipliers[i] * x to your score.
3. Remove x from the array nums.

Return the maximum score after performing m operations.

Input: nums = [1,2,3], multipliers = [3,2,1]

Output: 14

**Naive Memorization - O(m3)**

```cpp
int solve (vector<int>& nums, vector<int>& multipliers, int start, int end, int idx, vector<vector<vector<int>>> & dp){
    if(idx==multipliers.size()) 
        return 0;

    if(dp[start][end][idx]!=INT_MIN) return dp[start][end][idx];

    /* option 1 : pick from start */

    int res1 = nums[start]*multipliers[idx] + solve (nums, multipliers, start+1, end, idx+1, dp);

    /* option 2 : pick from end */

    int res2 = nums[end]*multipliers[idx] + solve (nums, multipliers, start, end-1, idx+1, dp);

    return dp[start][end][idx] = max(res1, res2);
}


int maximumScore(vector<int>& nums, vector<int>& multipliers) {
    int n = nums.size();
    int m = multipliers.size();

    vector<vector<vector<int>>> dp (n, vector<vector<int>> (n, vector<int> (m, INT_MIN)));
    return solve (nums, multipliers, 0, n-1, 0, dp);
}
```

**Optimization : Reducing DP variables - O(n2)**

```cpp
int solve (vector<int>& nums, vector<int>& multipliers, int start, int idx, int n, int m, vector<vector<int>> & dp){
    if(idx==m) 
        return 0;

    if(dp[start][idx]!=INT_MIN) 
        return dp[start][idx];

    int end = n - (idx-start) - 1;      // --> end can be simply computed by using other two variables

    /* option 1 : pick from start */

    int res1 = nums[start]*multipliers[idx] + solve (nums, multipliers, start+1, idx+1, n, m, dp);

    /* option 2 : pick from end */

    int res2 = nums[end]*multipliers[idx] + solve (nums, multipliers, start, idx+1, n, m, dp);

    return dp[start][idx] = max(res1, res2);
}


int maximumScore(vector<int>& nums, vector<int>& multipliers) {
    int n = nums.size();
    int m = multipliers.size();

    vector<vector<int>> dp (m, vector<int> (m, INT_MIN));
    return solve (nums, multipliers, 0, 0, n, m, dp);
}
```

<br>

### 14. Find Two Non-overlapping Sub-arrays Each With Target Sum
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

<br>

### 15. [Ways to form Max Heap](https://www.interviewbit.com/problems/ways-to-form-max-heap/)

```cpp
/*
        First calculate height, h = log2(A)+1;

        If tree is completely filled then,
            total_nodes = pow(2, h)-1; 

        Internal Nodes Calculation
        ---------------------------

        Number of internal nodes = total_nodes/2 (including root node)

        internal_nodes present in Left and Right = (total_nodes/2 - 1)/2


        Leaf Nodes Calculation
        -----------------------

        Now, we need to distribute leaves in left and right subtree, and we will use the 
        property that leaves are filled in contiguos fashion from left to right. 

        If tree is completely filled then, total_leaf_nodes =  (total_nodes/2)+1
        
        Given total_nodes = A
        Hence actual_leaf_nodes = A - total_nodes/2

        If (actual_leaf_nodes <= total_leaf_nodes/2):
            Left_leaf_nodes = actual_leaf_nodes
            Right_left_nodes = 0
        Else:
            Left_leaf_nodes = total_leaf_nodes/2
            Right_leaf_nodes = actual_leaf_nodes - Left_leaf_nodes


        Total Left and Right Cnt 
        -------------------------

        L =  internal_nodes_at_left + Left_leaf_nodes
        R = internal_nodes_at_right + Right_leaf_nodes


        Recursive Equation
        -------------------

        MaxElement will always be present on the root. Now we are A-1 elements are there,
        We can pick L number of elements from (A-1) and put them in left side. Hence,

        T(A) = nCr(A-1, L) * T(L) * T(R)
*/

long long fact (int n){
    if(n==1) return 1;
    return n * fact(n-1);
}


long long nCr (int n, int r){
    if(n==0 or r==0) return 1;
    return fact(n) / (fact(r) * fact(n-r));
}


int total_ways (int A, vector<long long> &dp){
    if(A==1 or A==2) return 1;

    if(dp[A]!=-1) return dp[A];
    
    int MOD = 1000000007;

    int h = log2(A)+1;
    long long total_nodes = pow(2, h)-1;
    long long total_leaf_nodes = (total_nodes/2)+1;
    long long actual_leaf_nodes = A - total_nodes/2;

    long long L = (total_nodes/2 - 1)/2;
    long long R = (total_nodes/2 - 1)/2;

    if(actual_leaf_nodes <= total_leaf_nodes/2){
        L += actual_leaf_nodes;
    }
    else{
        L += total_leaf_nodes/2;
        R += actual_leaf_nodes - total_leaf_nodes/2;
    }

    long long choosen = nCr(A-1, L) % MOD;
    return dp[A] = (choosen * ((total_ways(L, dp) * total_ways(R, dp)) % MOD)) % MOD;
}


int Solution::solve(int A) {
    vector<long long> dp (A+1, -1);
    return total_ways (A, dp);
}

```



