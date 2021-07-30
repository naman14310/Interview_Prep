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

### 3. Minimum Cost For Tickets
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

### 4. Best Time to Buy and Sell Stock with Transaction Fee
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
