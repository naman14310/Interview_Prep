# Problems on Knapsack Pattern

<br>

### @ 0-1 Knapsack (Parent Problem)
Given weights and values of n items, put these items in a knapsack of capacity W to get the maximum total value in the knapsack. You cannot break an item, either pick the complete item or don’t pick it (0-1 property)

<br>

Complexity Analysis: 

1. Time Complexity: O(N*W) 
-> As redundant calculations of states are avoided.

2. Auxiliary Space: O(N*W) 
-> The use of 2D array data structure for storing intermediate states

<br>

Identifying Pattern:

If some array A is given in question and we have choices to choose every item to get considered for filling in a bag W then that problem is of knapsack pattern.

<br>

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

<br>

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

<br>

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

<br>

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

<br>

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

<br>

### 5. Count Subsets with given difference
Given an integer array arr of size N and a difference diff, the task is to divide it into two sets S1 and S2 such that the absolute difference between their sums is equal to diff.

Input: nums = {1, 1, 2, 3}, diff = 3

Output: 3

Hint: Reduce it to count of subset sums.

```
Math Behind the question:

    s2 - s1 = diff   (where s1 is smaller subset sum)
    (totalSum-s1) - s1 = diff --> totalSum - 2*s1 = diff
    s1 = (totalSum - diff)/2;
        
```

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
    int n = nums.size();
    int totalSum = 0;
    
    for(int i : nums)
        totalSum += i;

    int s1 = (totalSum-diff)/2;
    vector<vector<int>> dp (n+1, vector<int> (s1+1, -1));

    return count_subsetSum (nums, n, s1, 0, 0, dp);
}
```

<br>

### 6. Target Sum
You are given an integer array nums and an integer target. You want to build an expression out of nums by adding one of the symbols '+' and '-' before each integer in nums and then concatenate all the integers. Return the number of different expressions that you can build, which evaluates to target.

Input: nums = [1,1,1,1,1], target = 3

Output: 5

```cpp
/* Standard count subset sum memorized solution */

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


int findTargetSumWays(vector<int>& v, int diff) {
    /*
        s2 - s1 = diff   (where s1 is smaller subset sum)
        (totalSum-s1) - s1 = diff --> totalSum - 2*s1 = diff
        s1 = (totalSum - diff)/2;
    */

    int totalSum = accumulate(v.begin(), v.end(), 0);
    int zero_cnt = count(v.begin(), v.end(), 0);

    /* Create new vector nums by removing all zeros */

    vector<int> nums;
    for(int i : v)
        if(i!=0) nums.push_back(i);

    int n = nums.size();

    if(diff > totalSum || (totalSum-diff)%2 !=0 )        // --> Boundary case
        return 0;

    int s1 = (totalSum-diff)/2;
    vector<vector<int>> dp (n+1, vector<int> (s1+1, -1));

    /* While returning the answer, multiply it with pow(2, zero_cnt) */

    return count_subsetSum (nums, n, s1, 0, 0, dp) * pow(2, zero_cnt);
}
```

<br>

### 7. Equal Average Partition
Given an array A with non negative numbers, divide the array into two parts such that the average of both the parts is equal. Return both parts (If exist). If there is no solution. return an empty list. If multiple solutions exist, return the solution where length(A) is minimum. If there is still a tie, return the one where A is lexicographically smallest.

Input: A = [1 7 15 29 11 9]

Output: [[9 15], [1 7 11 29]]

Hint: Reduce it to subset sum problem

```
Simplifiation using Maths:

    S = totalSum of array
    s1 = sum of part1
    n1 = len of part1

    avg(part1) = avg(part2)
    s1/n1 = (S-s1)/(n-n1)

    s1*(n-n1) = (S-s1)*n1
    s1*n - s1*n1 = S*n1 - s1*n1

    s1*n = S*n1

    => s1 = (S*n1)/n    ----> eq1

Hence we, get eq1 which fill form the basis of this quesyion. 
Now we will iterate for each possible value of n1 (i.e. 1 to n/2), and checks whether any subset having sum equals to s1 exists or not.

Return smallest such subset as part1 and remaining elements in part2.

```

```cpp

bool solve (vector<int> &A, vector<int> &part1, int idx, int sum, int len, vector<vector<vector<int>>> &dp){
    if(sum==0) return true;
    if(idx==A.size() or len==0) return false;

    if(dp[idx][sum][len]!=-1) return dp[idx][sum][len];

    if(A[idx]<=sum){
        part1.push_back(idx);
        bool include = solve (A, part1, idx+1, sum-A[idx], len-1, dp);
        if(include) return dp[idx][sum][len] = true;

        part1.pop_back();
        bool not_include = solve(A, part1, idx+1, sum, len, dp);
        return dp[idx][sum][len] = not_include;
    }
    else{
        bool not_include = solve(A, part1, idx+1, sum, len, dp);
        return dp[idx][sum][len] = not_include;
    }
}


vector<vector<int> > Solution::avgset(vector<int> &A) {
    int n = A.size();
    sort(A.begin(), A.end());       // --> sort A to get lexicographically smallest answer  

    int totalSum = accumulate(A.begin(), A.end(), 0);

    /* Iterate for each possible val of n1 and find corresponding s1 using eq1 */

    for(int n1=1; n1<=n/2; n1++){
        if((totalSum*n1)%n!=0) continue;    // --> If s1 comes out to be non-integral val, simply continue

        int s1 = (totalSum*n1)/n;

        vector<int> part1;
        vector<vector<vector<int>>> dp (n, vector<vector<int>> (s1+1, vector<int> (n1+1, -1)));

        /* Check whether any subset of length n1 and sum s1 exists or not (also form that subset) */  

        bool sumExist = solve(A, part1, 0, s1, n1, dp);

        /* if subset exist, return respective part and part2, else check for higher n1 val */

        if(sumExist){

            vector<int> p1, p2;
            int i=0, j=0;

            while(i<n){
                if(j<n1 and i==part1[j]){
                    p1.push_back(A[i]);
                    i++; j++;
                }
                else{
                    p2.push_back(A[i]);
                    i++;
                }
            }

            return {p1, p2};
        }

    }

    return {};
}
```

<br>

### 8. Last Stone Weight II (Tricky)
You are given an array of integers stones where stones[i] is the weight of the ith stone. We are playing a game with the stones. On each turn, we choose any two stones and smash them together. Suppose the stones have weights x and y with x <= y. The result of this smash is:
1. If x == y, both stones are destroyed, and
2. If x != y, the stone of weight x is destroyed, and the stone of weight y has new weight y - x.

At the end of the game, there is at most one stone left. Return the smallest possible weight of the left stone. If there are no stones left, return 0.

Input: stones = [2,7,4,1,8,1]

Output: 1

Hint: Similar to Minimum Subset sum difference

```cpp
/* Similar to Minimum Subset Sum Difference */

int lastStoneWeightII(vector<int>& stones) {
    int n = stones.size();

    int sum = 0;
    for(int s : stones)
        sum += s;

    vector<vector<bool>> dp (n+1, vector<bool> (sum+1, false));

    for(int i=0; i<=n; i++){
        for(int j=0; j<=sum; j++){

            if(j==0) dp[i][j] = true;

            else if(i==0) dp[i][j] = false;

            else if(stones[i-1]>j)
                dp[i][j] = dp[i-1][j];

            else
                dp[i][j] = dp[i-1][j] or dp[i-1][j-stones[i-1]];
        }
    }


    for(int k=sum/2; k>0; k--){
        if(dp[n][k])
            return (sum-2*k);
    }

    return sum;    // --> If n==1 then return sum
}
```

<br>

### 9. Ones and Zeroes (Tricky)
You are given an array of binary strings strs and two integers m and n. Return the size of the largest subset of strs such that there are at most m 0's and n 1's in the subset.

Input: strs = ["10","0001","111001","1","0"], m = 5, n = 3

Output: 4

Hint: Just apply knapsack property on two variables.

```cpp
int solve (vector<pair<int,int>> & v, int idx, int m, int n, vector<vector<vector<int>>> & dp){
    if(idx==v.size()) return 0;

    if(dp[idx][m][n]!=-1) 
        return dp[idx][m][n];

    if(v[idx].first<=m and v[idx].second<=n){

        int res1 = 1 + solve (v, idx+1, m-v[idx].first, n-v[idx].second, dp);
        int res2 = solve (v, idx+1, m, n, dp);

        return dp[idx][m][n] = max(res1, res2);
    }        

    else{
        int res = solve (v, idx+1, m, n, dp);
        return dp[idx][m][n] = res;
    }
}


int findMaxForm(vector<string>& strs, int m, int n) {
    vector<pair<int,int>> v;

    for(string s : strs){
        int one_cnt = 0, zero_cnt = 0;

        for(char ch : s){
            if(ch=='0') zero_cnt++;
            else one_cnt++;
        }

        v.push_back({zero_cnt, one_cnt});
    }

    vector<vector<vector<int>>> dp (v.size(), vector<vector<int>> (101, vector<int> (101, -1)));

    return solve (v, 0, m, n, dp);
}
```

<br>

### 10. Greatest Sum Divisible by Three (Tricky)
Given an array nums of integers, we need to find the maximum possible sum of elements of the array such that it is divisible by three.

Input: nums = [3,6,5,1,8]

Output: 18

Hint: Use remainder in place of sum

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

<br>


------

<br>


### @ Unbounded Knapsack (Parent Problem)
This is different from classical Knapsack problem, here we are allowed to use unlimited number of instances of an item.

<br>

**Method 1 : Memorization (Top-Down)**

```cpp
int solve (int wt[], int val[], int n, int w, int idx, vector<vector<int>> & dp){
    if(idx==n or w==0) return 0;

    if(dp[idx][w]!=-1) return dp[idx][w];

    if(wt[idx]<=w){

        /* 
            There is just one difference b/w Knapsack and Unbounded Knapsack is that
            If include any item, then it will be treated as unprocessed and same idx
            will be passed to recursion function

        */

        int p1 = val[idx] + solve (wt, val, n, w-wt[idx], idx, dp);      // --> including item
        int p2 = solve (wt, val, n, w, idx+1, dp);                       // --> not including item
        return dp[idx][w] = max(p1, p2);
    }
    else{
        int p = solve (wt, val, n, w, idx+1, dp);
        return dp[idx][w] = p;
    }
}

int knapSack(int n, int w, int val[], int wt[]){
    vector<vector<int>> dp (n+1, vector<int> (w+1, -1));
    return solve (wt, val, n, w, 0, dp);
}
```

**Method 2 : Tabulation (Bottom-Up)**

```cpp
int knapSack(int n, int w, int val[], int wt[]){
    vector<vector<int>> dp (n+1, vector<int> (w+1, 0));

    for(int i=1; i<=n; i++){
        for(int j=1; j<=w; j++){

            /* If weight of curr_item is greater the curr_knapsack then we cannot include it anyhow */

            if(wt[i-1]>j)
                dp[i][j] = dp[i-1][j];

            /* 
                In unbounded knapsack, if we include any item we treat it as unprocessed 
                so after subtracting weight of curr_item from knapsack, 
                we will take the profit from same row (i instead of i-1)

            */

            else
                dp[i][j] = max(dp[i-1][j], dp[i][j-wt[i-1]] + val[i-1]);    //--> Just one small change from knapsack
        }
    }

    return dp[n][w];
}
```

<br>

### 1. Rod Cutting Problem
Given a rod of length n inches and an array of prices that includes prices of all pieces of size smaller than n. Determine the maximum value obtainable by cutting up the rod and selling the pieces.

```cpp
int solve (vector<int> & price, int n, int idx, int remaining_len, vector<vector<int>> & dp){
    if(idx==n or remaining_len==0) return 0; 

    if(dp[idx][remaining_len]!=-1) return dp[idx][remaining_len];

    if(idx+1 <= remaining_len){
        int p1 = price[idx] + solve (price, n, idx, remaining_len-(idx+1), dp);
        int p2 = solve (price, n, idx+1, remaining_len, dp);
        return dp[idx][remaining_len] = max(p1, p2);
    }
    else{
        int p = solve (price, n, idx+1, remaining_len, dp);
        return dp[idx][remaining_len] = p;
    }
}

/* price arrays contains the price of every possible len from 1 to n, where n is total len of rod*/

int rod_cutting_max_profit (vector<int> & price){
    int n = price.size();
    vector<vector<int>> dp (n+1, vector<int>(n+1, -1));
    return solve (price, n, 0, n, dp);
}
```

<br>

### 2. Coin Change I (Find Minimum number of coins)
You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money. Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1. You may assume that you have an infinite number of each kind of coin.

Input: coins = [1,2,5], amount = 11

Output: 3

```cpp
int solve (vector<int> & coins, int n, int amount, int idx, vector<vector<int>> & dp){
    if(idx==n) return -1;       // --> If we reached end It means we didn't found the way to exchange coins
    if(amount==0) return 0;     // --> If amount becomes 0, that means we found the way to exchange

    if(dp[idx][amount]!=INT_MIN) return dp[idx][amount];

    /* If current coin val is lesser then or equal to amount, we have two choices */

    if(coins[idx]<=amount){
        int res1 = solve (coins, n, amount-coins[idx], idx, dp);    // --> Either include that coin
        int res2 = solve (coins, n, amount, idx+1, dp);             // --> Or Not include that coin

        if(res1==-1 and res2==-1)         // Case 1: If using both coices, we failed to exchange
            dp[idx][amount] = -1;

        else if(res1==-1 or res2==-1)     // Case 2: If either of one case is used for exchange
            dp[idx][amount] = res1!=-1 ? res1+1 : res2;    

        else                              // Case 3: If both cases leads to some solution then we will take min of both
            dp[idx][amount] = min (res1+1, res2);

        return dp[idx][amount];
    }

    /* Else we cannot include that coin so only one choice left */ 

    else{
        int res = solve (coins, n, amount, idx+1, dp);
        return dp[idx][amount] = res;
    }
}


int coinChange(vector<int>& coins, int amount) {
    int n = coins.size();
    vector<vector<int>> dp (n+1, vector<int> (amount+1, INT_MIN));

    return solve (coins, n, amount, 0, dp);
}
```

<br>

### 3. Coin Change II (Find Max number of ways)
You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money. Return the number of combinations that make up that amount. If that amount of money cannot be made up by any combination of the coins, return 0. You may assume that you have an infinite number of each kind of coin.

Input: amount = 5, coins = [1,2,5]

Output: 4

Hint: Combination of unbounded knapsack and count subsetSum

```cpp
int solve (vector<int> & coins, int n, int amount, int idx, vector<vector<int>> & dp){
    if(idx==n) return 0;
    if(amount==0) return 1;

    if(dp[idx][amount]!=INT_MIN) return dp[idx][amount];

    if(coins[idx]<=amount){
        int res1 = solve (coins, n, amount-coins[idx], idx, dp);
        int res2 = solve (coins, n, amount, idx+1, dp);

        return dp[idx][amount] = res1 + res2;
    }
    else{
        int res = solve (coins, n, amount, idx+1, dp);
        return dp[idx][amount] = res;
    }
}


int change(int amount, vector<int>& coins) {
    int n = coins.size();
    vector<vector<int>> dp (n+1, vector<int> (amount+1, INT_MIN));

    return solve (coins, n, amount, 0, dp);
}
```

<br>

### 4. Perfect Squares
Given an integer n, return the least number of perfect square numbers that sum to n.

Input: n = 12

Output: 3

Hint: Similar to Coin Change I

```cpp
int solve (vector<int> & sqrs, int n, int idx, int sum, vector<vector<int>> & dp){
    if(sum==n) return 0;
    if(idx==sqrs.size() or sum>n) return -1;

    if(dp[idx][sum]!=INT_MIN) return dp[idx][sum];

    auto res1 = solve (sqrs, n, idx, sum+sqrs[idx], dp);        // --> Include idx
    auto res2 = solve (sqrs, n, idx+1, sum, dp);                // --> Not include idx

    if(res1!=-1 and res2!=-1)
        return dp[idx][sum] = min(res1+1, res2);

    else if(res1!=-1)
        return dp[idx][sum] = res1+1;

    else if(res2!=-1)
        return dp[idx][sum] = res2;

    else return dp[idx][sum] = -1;
}


int numSquares(int n) {
    vector<vector<int>> dp (n+1, vector<int> (n+1, INT_MIN));
    vector<int> sqrs;

    for(int i=1; i*i<=n; i++)
        sqrs.push_back(i*i);

    return solve (sqrs, n, 0, 0, dp);
}
```

<br>

### 5. Tushar's Birthday Party
Given the eating capacity of each friend, filling capacity of each dish and cost of each dish. A friend is satisfied if the sum of the filling capacity of dishes he ate is equal to his capacity. Find the minimum cost such that all of Tushar’s friends are satisfied (reached their eating capacity). Note that,
1. Each dish is supposed to be eaten by only one person. Sharing is not allowed.
2. Each friend can take any dish unlimited number of times.
3. There always exists a dish with filling capacity 1 so that a solution always exists.

Hint: Use Tabulation Method for Unbounded knapsack. Last row of matrix will give minCost for All capacities (No need to compute again-n-again).

```cpp
vector<vector<int>> knapsack (const vector<int> &weight, const vector<int>&cost, int n){
    int row = n+1, col = 1001;
    vector<vector<int>> dp (row, vector<int> (col, -1));

    for(int i=0; i<row; i++){
        for(int j=0; j<col; j++){

            if(j==0) dp[i][j] = 0;

            else if(i==0) dp[i][j] = -1;

            else if(weight[i-1]>j)
                dp[i][j] = dp[i-1][j];
            
            else{
                int c1 = dp[i-1][j], c2 = dp[i][j-weight[i-1]];

                if(c1==-1 and c2==-1) dp[i][j] = -1;
                else if(c1==-1) dp[i][j] = c2 + cost[i-1];
                else if(c2==-1) dp[i][j] = c1;
                else dp[i][j] = min (c1, c2+cost[i-1]);
            }
        }
    }

    return dp;
}


int Solution::solve(const vector<int> &A, const vector<int> &B, const vector<int> &C) {
    int totalCost = 0;
    int n = B.size();

    auto matrix = knapsack (B, C, n);

    for(int w : A)
        totalCost += matrix[n][w];
    
    return totalCost;
}
```

