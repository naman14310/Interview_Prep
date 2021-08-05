# Problems on LIS Pattern

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

### 3. Longest Arithmetic Subsequence
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

### 4. Length of Longest Fibonacci Subsequence
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

### 5. Best Team With No Conflicts
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
