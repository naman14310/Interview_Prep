# Problems on Knapsack Pattern

### @ 0-1 Knapsack (Parent Problem)
Given weights and values of n items, put these items in a knapsack of capacity W to get the maximum total value in the knapsack. You cannot break an item, either pick the complete item or don’t pick it (0-1 property)

Complexity Analysis: 

1. Time Complexity: O(N*W) 
-> As redundant calculations of states are avoided.

2. Auxiliary Space: O(N*W) 
-> The use of 2D array data structure for storing intermediate states

Identifying Pattern:

If some array A is given in question and we have choices to choose every item to get considered for filling in a bag W then that problem is of knapsack pattern.

**Method 1 : Memorization (Top-Down)**

```cpp
int solve (int wt[], int val[], int w, int n, int idx, vector<vector<int>> & dp){
    if(idx==n or w==0)
        return 0;

    if(dp[idx][w]!=-1) return dp[idx][w];

    /* If weight of current item <= knapsack, then we have two choices */

    if (wt[idx]<=w){
        int p1 = val[idx] + solve (wt, val, w-wt[idx], n, idx+1, dp);       //--> Either include that item
        int p2 = solve (wt, val, w, n, idx+1, dp);                          //--> Or Not include that item
        return dp[idx][w] = max(p1, p2);    
    }

    /* Else we can't include that item */

    else{
        int p1 = solve (wt, val, w, n, idx+1, dp);
        return dp[idx][w] = p1;
    }
}


int knapSack(int w, int wt[], int val[], int n) { 

    /* Make dp for only those variable which are changing */

    vector<vector<int>> dp (n+1, vector<int> (w+1, -1));

    return solve (wt, val, w, n, 0, dp);
}
```

**Method 2 : Tabulation (Bottom-Up)**

```cpp
int knapSack(int w, int wt[], int val[], int n) { 
    vector<vector<int>> dp (n+1, vector<int> (w+1, 0));

    for(int i=1; i<=n; i++){
        for(int j=1; j<=w; j++){
            if(wt[i-1]>j)
                dp[i][j] = dp[i-1][j];
            else
                dp[i][j] = max(dp[i-1][j], dp[i-1][j-wt[i-1]] + val[i-1]);
        }
    }

    return dp[n][w];
}
```

### 1. Subset Sum Problem
Given an array of non-negative integers, and a value sum, determine if there is a subset of the given set with sum equal to given target sum. 

Input: n = 6, arr[] = {3, 34, 4, 12, 5, 2}, target = 9

Output: 1

**Memorization**

```cpp
bool solve (int arr[], int n, int target, int idx, int curr_sum, vector<vector<int>> & dp){
    if(curr_sum==target) return true;
    if(idx==n) return false;

    if(dp[idx][curr_sum]!=-1) return dp[idx][curr_sum];

    if(curr_sum + arr[idx] <= target){
        bool res1 = solve(arr, n, target, idx+1, curr_sum+arr[idx], dp);    //--> Either include that item
        bool res2 = solve(arr, n, target, idx+1, curr_sum, dp);             //--> Or Not include that item
        return dp[idx][curr_sum] = res1 or res2;
    }
    else{
        bool res = solve (arr, n, target, idx+1, curr_sum, dp);             //--> Cannot include that item
        return dp[idx][curr_sum] = res;
    }
}

bool isSubsetSum(int n, int arr[], int target){
    vector<vector<int>> dp (n+1, vector<int> (target+1, -1));
    return solve (arr, n, target, 0, 0, dp);
}
```

**Tabulation**

```cpp
bool isSubsetSum(int n, int arr[], int target){
    vector<vector<bool>> dp (n+1, vector<bool> (target+1, false));

    for(int i=0; i<=n; i++){
        for(int j=0; j<=target; j++){

            if(j==0)
                dp[i][j] = true;        //--> Initially fill first col with True

            else if(i==0)
                dp[i][j] = false;       //--> After that fill first row with False

            else if(arr[i-1]>j)
                dp[i][j] = dp[i-1][j];

            else
                dp[i][j] = dp[i-1][j] or dp[i-1][j-arr[i-1]];
        }
    }

    return dp[n][target];
}
```

### 2. Partition Equal Subset Sum
Given a non-empty array nums containing only positive integers, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

Input: nums = [1,5,11,5]

Output: true

Hint: Find if there any subset sum exist whose sum is equal to half of totalSum

```cpp
    bool subsetSum (vector<int> & nums, int n, int target, int idx, int curr_sum, vector<vector<int>>& dp){
        if(curr_sum==target) return true;    //--> Always first check truth condition then false condition
        if(idx==n) return false;
        
        if(dp[idx][curr_sum]!=-1) return dp[idx][curr_sum];
        
        if(curr_sum + nums[idx] <= target){
            bool res1 = subsetSum (nums, n, target, idx+1, curr_sum+nums[idx], dp);
            bool res2 = subsetSum (nums, n, target, idx+1, curr_sum, dp);
            return dp[idx][curr_sum] = res1 or res2;
        }
        else{
            bool res = subsetSum (nums, n, target, idx+1, curr_sum, dp);
            return dp[idx][curr_sum] = res;
        }
    }
    
    
    bool canPartition(vector<int>& nums) {
        int n = nums.size();
        
        int sum=0;
        for(int i : nums)
            sum+=i;
        
        if(sum%2!=0) return false;
        
        int target = sum/2;       //--> Now problem reduced to find if any subset sum exist whose sum==target
        
        vector<vector<int>> dp (n+1, vector<int> (target+1, -1));
        return subsetSum (nums, n, target, 0, 0, dp);
    }
```
