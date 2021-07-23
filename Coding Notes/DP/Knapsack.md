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

### 3. Count of Subset Sum
Given an array arr[] of length N and an integer target, the task is to find the number of subsets with a sum equal to target.

Input: arr[] = {1, 2, 3, 3}, target = 6 

Output: 3 

Hint: Just change return statements and sign in subset sum problem

```cpp
int solve (vector<int> & nums, int n, int target, int idx, int curr_sum, vector<vector<int>> & dp){
    if(curr_sum==target) return 1;                       // -->  Change True to 1
    if(idx==n) return 0;                                 // --> Change False to 0

    if(dp[idx][curr_sum]!=-1) return dp[idx][curr_sum];

    if(curr_sum + nums[idx] <= target){
        int res1 = solve (nums, n, target, idx+1, curr_sum+nums[idx], dp);    
        int res2 = solve (nums, n, target, idx+1, curr_sum, dp);
        return dp[idx][curr_sum] = res1 + res2;           // --> Just change 'OR' from subset sum to '+' sign
    }
    else{
        int res = solve (nums, n, target, idx+1, curr_sum, dp);
        return dp[idx][curr_sum] = res;
    }
}


int count_subset_sum (vector<int> & nums, int target){
    int n = nums.size();
    vector<vector<int>> dp (n+1, vector<int> (target+1, -1));
    return solve (nums, n, target, 0, 0, dp);
}
```

### 4. Minimum Subset Sum Difference
Given an integer array arr of size N, the task is to divide it into two sets S1 and S2 such that the absolute difference between their sums is minimum and find the minimum difference.

Input: N = 4, arr[] = {1, 6, 11, 5} 

Output: 1

Hint: Create a subset sum dp table for totalSum of array. Then last row of dp table stores answer for every possible sum in range [0, totalSum]. If sum of smaller subset S1 is closer to middle line then that will give us min subset difference.

```cpp
/* Here we need to do tabulation method because we want whole last row */

void subsetSum (int arr[], int n, int target, vector<vector<bool>> & dp){
    for(int i=0; i<=n; i++){
        for(int j=0; j<=target; j++){

            if(j==0) dp[i][j] = true;

            else if(i==0) dp[i][j] = false;

            else if(arr[i-1]>j)
                dp[i][j] = dp[i-1][j];

            else
                dp[i][j] = dp[i-1][j] or dp[i-1][j-arr[i-1]];
        }
    }
}


int minDifference(int arr[], int n)  { 
    int target = 0;
    for(int i=0; i<n; i++)
        target += arr[i];

    vector<vector<bool>> dp (n+1, vector<bool> (target+1, false));
    subsetSum (arr, n, target, dp);

    /* start iterating from target/2 and check whether any subset sum exist */
    /* because sum that lie closer to the center line will produce min possible difference */

    for(int i=target/2; i>=0; i--)
        if(dp[n][i]) return target-(2*i);

    return target;
} 
```

### 5. Count Subsets with given difference
Given an integer array arr of size N and a difference diff, the task is to divide it into two sets S1 and S2 such that the absolute difference between their sums is equal to diff.

Hint: Reduce it to count of subset sums

Input: nums = {1, 1, 2, 3}, diff = 3

Output: 3

```cpp
int count_subsetSum (vector<int> & nums, int n, int target, int idx, int curr_sum, vector<vector<int>>& dp){
    if(curr_sum == target) return 1;
    if(idx==n) return 0;

    if(dp[idx][curr_sum]!=-1) return dp[idx][curr_sum];

    if(curr_sum+nums[idx] <= target){
        int res1 = count_subsetSum (nums, n, target, idx+1, curr_sum + nums[idx], dp);
        int res2 = count_subsetSum (nums, n, target, idx+1, curr_sum, dp);
        return dp[idx][curr_sum] = res1+res2;
    }
    else{
        int res = count_subsetSum (nums, n, target, idx+1, curr_sum, dp);
        return dp[idx][curr_sum] = res;
    }
}


int count_subset_with_given_diff (vector<int> & nums, int diff){
    /*
        s2 - s1 = diff   (where s1 is smaller subset sum)
        (totalSum-s1) - s1 = diff --> totalSum - 2*s1 = diff
        s1 = (totalSum - diff)/2;
    */
    int n = nums.size();
    int totalSum = 0;
    for(int i : nums)
        totalSum += i;

    int s1 = (totalSum-diff)/2;
    vector<vector<int>> dp (n+1, vector<int> (s1+1, -1));

    return count_subsetSum (nums, n, s1, 0, 0, dp);
}
```


