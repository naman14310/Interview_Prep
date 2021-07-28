# Recursive + Memorization (DP)

## @ 1D

### Easy

#### 1. Climbing Stairs
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

#### 2. Min Cost Climbing Stairs
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
