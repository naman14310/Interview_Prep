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
