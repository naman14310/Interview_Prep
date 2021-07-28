# Problems on MCM Pattern

Identifying Pattern:
When we need to break a problem at different pos like palindrome partitioning, then that question follows MCM pattern.

General Steps:
1. Figure out pos for i and j where i moves from left to right and j moves from right to left.
2. Figure out base condition by thinking of a invalid input.
3. Run a loop for k from i to j and compute temp answer for every break
4. compute final answer and return it.

### @ Matrix Chain Multiplication (Parent Question)
Given a sequence of matrices, find the most efficient way to multiply these matrices together. The efficient way is the one that involves the least number of multiplications. The dimensions of the matrices are given in an array arr[] of size N (such that N = number of matrices + 1) where the ith matrix has the dimensions (arr[i-1] x arr[i]).

Cost Estimation: Let A,B be two matrices of given dimensions then cost will be,

A = 20 X 10, B = 10 X 30 --> cost = 20 X 10 X 30

[Video Explaination](https://www.youtube.com/watch?v=kMK148J9qEE)

```cpp
int solve (int arr[], int i, int j, vector<vector<int>> & dp){

    if(i>=j) return 0;    // --> base condition occurs when there is only one element left

    if(dp[i][j]!=-1) return dp[i][j];

    int ans = INT_MAX;
    
    /* compute temp answer for every pos of k and update ans accordingly */
    
    for(int k=i; k<j; k++){
        int temp = solve (arr, i, k, dp) + solve (arr, k+1, j, dp) + (arr[i-1] * arr[k] * arr[j]);
        ans = min (ans, temp);
    }

    return dp[i][j] = ans;
}

int matrixMultiplication(int n, int arr[]){
    vector<vector<int>> dp (n+1, vector<int> (n+1, -1));
    return solve (arr, 1, n-1, dp);
}
```

### 1. Palindrome Partitioning II
Given a string s, partition s such that every substring of the partition is a palindrome. Return the minimum cuts needed for a palindrome partitioning of s.

```cpp
bool isPalindrome (string & s, int i, int j){
    while(i<j){
        if(s[i]!=s[j]) return false;
        i++; j--;
    }
    return true;
}


int solve (string & s, int i, int j, vector<vector<int>> & dp){    
    if(i>=j or isPalindrome(s, i, j)) return 0;
    if(dp[i][j]!=-1) return dp[i][j];

    int ans = INT_MAX;

    for(int k=i; k<j; k++){

        /* 
            Instead of writing below standard line
            We will recurse for only right part
            Only when left part turns out to be palindrome

            int temp =  solve (s, i, k, dp, palindrome) + solve (s, k+1, j, dp, palindrome) + 1;
        */

        if(isPalindrome(s, i, k)){                         
            int temp = solve (s, k+1, j, dp) + 1;
            ans = min (ans, temp);
        }
    }

    return dp[i][j] = ans;
}


int minCut(string s) {
    int n = s.length();
    vector<vector<int>> dp (n+1, vector<int> (n+1, -1));

    return solve (s, 0, n-1, dp);
}
```

### 2. Boolean Parenthesization 
Given a boolean expression S of length N with following symbols.
```
Symbols
    'T' ---> true
    'F' ---> false
and following operators filled between symbols
Operators
    &   ---> boolean AND
    |   ---> boolean OR
    ^   ---> boolean XOR
```
Count the number of ways we can parenthesize the expression so that the value of expression evaluates to true.

Input: N = 5, S = T^F|F

Output: 2

Hint: Just compute total true_count and false_count for each subproblem and compute total truth_cnts using them

```cpp
pair<int,int> find_temp_ans (int left_true, int left_false, int right_true, int right_false, char op){
    int true_cnt, false_cnt;

    if(op=='&'){
        true_cnt = left_true * right_true;
        false_cnt = (left_true * right_false) + (left_false * right_true) + (left_false * right_false);
    }
    else if(op=='|'){
        true_cnt = (left_true * right_true) + (left_true * right_false) + (left_false * right_true);
        false_cnt = left_false * right_false;
    }
    else{
        true_cnt = (left_true * right_false) + (left_false * right_true);
        false_cnt = (left_true * right_true) + (left_false * right_false);
    }

    return {true_cnt%1003, false_cnt%1003};
}

// --> pair of {count of True, count of False}

pair<int, int> solve (string s, int i, int j, vector<vector<pair<int, int>>> & dp){

    if(i==j){
        if(s[i]=='T') return {1, 0};
        else return {0,1};
    }

    if(dp[i][j].first!=-1)
        return dp[i][j];


    pair<int, int> ans = {0,0};

    for(int k = i+1; k<j; k+=2){
        auto left = solve (s, i, k-1, dp);
        auto right = solve (s, k+1, j, dp);

        auto temp = find_temp_ans (left.first, left.second, right.first, right.second, s[k]);
        ans.first += temp.first;
        ans.second += temp.second;
    }

    return dp[i][j] = ans;
}


int countWays(int n, string s){
    vector<vector<pair<int, int>>> dp (n+1, vector<pair<int, int>> (n+1, {-1, -1}));

    auto res = solve (s, 0, n-1, dp);
    return res.first%1003;
}
```

### 3. Scramble String
We can scramble a string s to get a string t using the following algorithm:

1. If the length of the string is 1, stop.
2. If the length of the string is > 1, do the following:
3. Split the string into two non-empty substrings at a random index, i.e., if the string is s, divide it to x and y where s = x + y.
4. Randomly decide to swap the two substrings or to keep them in the same order. i.e., after this step, s may become s = x + y or s = y + x.
5. Apply step 1 recursively on each of the two substrings x and y.

Given two strings s1 and s2 of the same length, return true if s2 is a scrambled string of s1, otherwise, return false.

Input: s1 = "great", s2 = "rgeat"

Output: true

```
Explaination: We have following two choices,

Case 1. Without swap    -->     gr|eat      rg|eat
Recurse for (part1 of s1, part1 of s2) and (part2 of s1, part2 of s2)

Case 2. With swap       -->     gr|eat      rge|at 
Recurse for (part1 of s1, part2 of s2) and (part2 of s1, part1 of s2)    
```

```cpp
bool solve (string s1, string s2, unordered_set<string> & vis){
    int n = s1.length();
    if(n==1) return s1==s2;

    if(s1==s2) return true;

    string key = s1 + "," + s2;
    if(vis.find(key)!=vis.end()) return false;

    for(int k=1; k<n; k++){

        /* Case 1 : without swapping */

        bool noswap = solve (s1.substr(0, k), s2.substr(0, k), vis) and solve (s1.substr(k, n-k), s2.substr(k, n-k), vis);

        /* Case 2 : with swapping */

        bool swap = solve (s1.substr(0, k), s2.substr(n-k, k), vis) and solve (s1.substr(k, n-k), s2.substr(0, n-k), vis);

        if(swap or noswap)
            return true;
    }

    vis.insert(key);
    return false;
}


bool isScramble(string s1, string s2) {
    unordered_set<string> vis;
    return solve (s1, s2, vis);
}
```

### 4. Super Egg Drop
You are given e identical eggs and you have access to a building with n floors labeled from 1 to n. You know that there exists a floor f where 0 <= f <= n such that any egg dropped at a floor higher than f will break, and any egg dropped at or below floor f will not break. Each move, you may take an unbroken egg and drop it from any floor x. If the egg breaks, you can no longer use it. However, if the egg does not break, you may reuse it in future moves. Return the minimum number of moves that you need to determine with certainty what the value of f is.

```cpp
int solve (int e, int f, vector<vector<int>> & dp){

    /* 
        Base Cond: If we have only 1 egg, then we need to check every floor one by one
        or If there is only one floor left then we need only 1 chance to get critical floor
    */

    if(e==1 or f==1) return f;    

    if(dp[e][f]!=-1) return dp[e][f];

    int ans = f;

    /* we will try on every possible floor */

    for(int k=1; k<=f; k++){

        /* We have two options at every floor */

        /* option1 : If egg breaks from the curr floor, then it will also break from all above floors */

        int egg_breaks = solve (e-1, k-1, dp); 

        /* option2 : If egg not breaks from curr floor, then it will not break from any of the below floors */

        int egg_not_breaks = solve (e, f-k, dp);

        int temp = 1 + max(egg_breaks, egg_not_breaks);     // --> here we took max becoz we need to consider worst cases

        ans = min(ans, temp);           //--> here we took min becoz we need to choose minimum moves from all worst cases
    }

    return dp[e][f] = ans;
}

int superEggDrop(int e, int f) {
    vector<vector<int>> dp (e+1, vector<int> (f+1, -1));
    return solve (e, f, dp);
}
```

### 5. Partition Array for Maximum Sum
Given an integer array arr, partition the array into (contiguous) subarrays of length at most k. After partitioning, each subarray has their values changed to become the maximum value of that subarray. Return the largest sum of the given array after partitioning.

Input: arr = [1,15,7,9,2,5,10], k = 3

Output: 84

Explanation: arr becomes [15,15,15,9,10,10,10]

```cpp
int solve (vector<int>& arr, int i, int j, int subarr_size, vector<vector<int>>& dp){

    if(dp[i][j]!=-1) return dp[i][j];

    /* Base condition is when we get subarray of size lesser then subarr_size*/

    if(j-i+1 <= subarr_size){
        int mx = 0;
        for(int itr=i; itr<=j; itr++)
            mx = max(mx, arr[itr]);

        return mx*(j-i+1);
    }

    int ans = 0;

    for(int k=i; k<j; k++){
        int temp = solve (arr, i, k, subarr_size, dp) + solve (arr, k+1, j, subarr_size, dp);
        ans = max(ans, temp);
    }

    return dp[i][j] = ans;
}

int maxSumAfterPartitioning(vector<int>& arr, int subarr_size) {
    int n = arr.size();
    vector<vector<int>> dp (n+1, vector<int> (n+1, -1));
    return solve(arr, 0, n-1, subarr_size, dp);
}
```

**Optimized approach**

Instead of recursing for both side we can recurse for only one side by breaking k at every first possible subarray. (Somewhat similar to palindrome partitioning) 

```cpp
int solve (vector<int> & arr, int n, int i, int subarr_size, vector<int> & dp){
    if(i==n) return 0;   // --> Base cond : If starting index i exceeds n, the return 0 

    if(dp[i]!=-1) return dp[i];

    int mx = 0;
    int ans = 0;

    for(int k=i; k<min(i+subarr_size, n); k++){
        mx = max(mx, arr[k]);

        int temp = mx*(k-i+1) + solve (arr, n, k+1, subarr_size, dp);
        ans = max(temp, ans);
    }

    return dp[i] = ans;
}


int maxSumAfterPartitioning(vector<int>& arr, int k) {
    int n = arr.size();
    vector<int> dp (n, -1);
    return solve (arr, n, 0, k, dp);
}
```

### 6. Minimum Cost Tree From Leaf Values
Given an array arr of positive integers, consider all binary trees such that:
1. Each node has either 0 or 2 children;
2. The value of each non-leaf node is equal to the product of the largest leaf value in its left and right subtree respectively.

Among all possible binary trees considered, return the smallest possible sum of the values of each non-leaf node.

```
Input: arr = [6,2,4]
Output: 32
Explanation:
There are two possible trees.  The first has non-leaf node sum 36, and the second has non-leaf node sum 32.

    24            24
   /  \          /  \
  12   4        6    8
 /  \               / \
6    2             2   4
```

```cpp
/* rtype : pair of {sum, max_val} */

pair<int,int> solve (vector<int>& arr, int i, int j, vector<vector<pair<int,int>>> & dp){

    if(i==j) return {0, arr[i]};  // --> Base cond : When there is only one element left in arr    

    if(dp[i][j].first!=-1) return dp[i][j];

    pair<int,int> ans = {INT_MAX, INT_MAX};

    for(int k=i; k<j; k++){
        auto left = solve (arr, i, k, dp);
        auto right = solve (arr, k+1, j, dp);

        int lmax = left.second, rmax = right.second;
        int temp = (lmax * rmax) + left.first + right.first;

        if(temp<ans.first)
            ans = {temp, max(lmax, rmax)};
    }

    return dp[i][j] = ans;
}

int mctFromLeafValues(vector<int>& arr) {
    int n = arr.size();
    vector<vector<pair<int,int>>> dp (n+1, vector<pair<int,int>> (n+1, {-1, -1}));

    auto ans = solve (arr, 0, n-1, dp);
    return ans.first;
}
```
