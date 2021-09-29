# Problems on Matrix Chain Multiplication (MCM) Pattern

Identifying Pattern:
When we need to break a problem at different pos like palindrome partitioning, then that question follows MCM pattern.

General Steps:
1. Figure out pos for i and j where i moves from left to right and j moves from right to left.
2. Figure out base condition by thinking of a invalid input.
3. Run a loop for k from i to j and compute temp answer for every break
4. compute final answer and return it.

<br>

## @ Matrix Chain Multiplication (Parent Question)
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

<br>

### 1. Merge elements
Given an integer array A of size N. Choose any two adjacent elements of the array with values say X and Y and merge them into a single element with value (X + Y) paying a total cost of (X + Y). Return the minimum possible cost of merging all elements.

Input: A = [1, 3, 7]

Output: 15

Hint: Cost for merging left part and right part at any point k will be, leftSum + rightSum + leftPrevCost + rightPrevCost

```cpp

/* return type: pair {cost_so_far, sum_computed} */

pair<int,int> mcm (vector<int> &A, int i, int j, vector<vector<pair<int,int>>> &dp){
    if(i==j) return {0, A[i]};
    
    if(dp[i][j].first!=-1) return dp[i][j];

    int minCost = INT_MAX; 
    pair<int,int> res;

    for(int k=i; k<j; k++){
        auto left = mcm (A, i, k, dp);
        auto right = mcm (A, k+1, j, dp);

        int sum = left.second + right.second;       // --> will contain total sum after merging
        int cost = sum+left.first+right.first;      // --> will contain total cost after merging

        if(cost<minCost){
            res = {cost, sum};
            minCost = cost;
        }
    }

    return dp[i][j] = res;
}


int solve(vector<int> &A) {
    int n = A.size();
    vector<vector<pair<int,int>>> dp(n+1, vector<pair<int,int>> (n+1, {-1,-1}));

    auto p = mcm(A, 0, n-1, dp);
    return p.first;
}

```

<br>

### 2. Minimum Cost Tree From Leaf Values
Given an array arr of positive integers, consider all binary trees such that:
1. Each node has either 0 or 2 children;
2. The value of each non-leaf node is equal to the product of the largest leaf value in its left and right subtree respectively.

Among all possible binary trees considered, return the smallest possible sum of the values of each non-leaf node.

```
Input: arr = [6,2,4]
Output: 32
Explanation: There are two possible trees. The first has non-leaf node sum 36, and the second has non-leaf node sum 32.

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

<br>

### 3. Burst Balloons (Tricky)
You are given n balloons, indexed from 0 to n - 1. You are asked to burst all the balloons. If you burst the ith balloon, you will get nums[i - 1] * nums[i] * nums[i + 1] coins. If i - 1 or i + 1 goes out of bounds of the array, then treat it as if there is a balloon with a 1 painted on it. Return the maximum coins you can collect by bursting the balloons wisely.

Input: nums = [1,5]

Output: 10

**Approach:** 

If we choose the kth balloon to be the first one to be burst, we can't make the subproblems independent. Let's try the other way round. We choose the kth balloon as the last one to be burst. Now the subproblems will become independent since (k - 1)th balloon and (k + 1)th balloon won't need each other in order to calculate the answer. 

Now for each k starting from i to j, we choose the kth balloon to be the last one to be burst and calculate the profit by solving the subproblems recursively. Whichever choice of k gives us the best answer, we store it and return.

Important point to be noted here is that the balloons in the range (i, k - 1) and (k + 1, j) will be burst BEFORE kth balloon. So, when we burst the kth balloon, the profit will be nums[i - 1] * nums[k] * nums[j + 1] PROVIDED that index i - 1 and j + 1 are valid.

[Explained Solution](https://leetcode.com/problems/burst-balloons/discuss/892552/For-those-who-are-not-able-to-understand-any-solution-WITH-DIAGRAM)

![img](https://assets.leetcode.com/users/images/1bafbe44-cb85-4ade-adb5-54ee5095baea_1602579692.7557607.png)

```cpp
int solve (vector<int> & nums, int i, int j, int n, vector<vector<int>> & dp){

    if(i>j) return 0;  // --> If no balloon left, return 0

    if(dp[i][j]!=-1) return dp[i][j];

    /* 
        If i==j, then only one balloon left in this window, 
        so return it by multiplying it with the left outer and right outer val
    */

    if(i==j){
        int l = i-1 >= 0 ? nums[i-1] : 1;
        int r = j+1 < n ? nums[j+1] : 1;

        return dp[i][j] = nums[i]*l*r;
    } 

    int ans = 0;

    /* 
        One by one iterate over all the balloons inside this window 
        and treat them as last balloon of that window

        Now since it is last balloon all its adjacent balloons inside this window are already bursted
        So, left outer balloon and right outer balloon outside the window will now become adjacent
        hence multiply their cost and solve recursively

    */

    for(int k=i; k<=j; k++){

        int l = i-1 >= 0 ? nums[i-1] : 1;
        int r = j+1 < n ? nums[j+1] : 1;

        int left = solve (nums, i, k-1, n, dp);
        int right = solve (nums, k+1, j, n, dp);

        int temp = left + right + (nums[k]*l*r);
        ans = max(ans, temp);
    }

    return dp[i][j] = ans;
}


int maxCoins(vector<int>& nums) {
    int n = nums.size();
    vector<vector<int>> dp (n, vector<int> (n, -1));

    return solve (nums, 0, n-1, n, dp);
}
```

<br>

### 4. Minimum Cost to Cut a Stick (Tricky)
Given a wooden stick of length n units. Given an integer array cuts where cuts[i] denotes a position you should perform a cut at. You should perform the cuts, you can change the order of the cuts as you wish. The cost of one cut is the length of the stick to be cut, the total cost is the sum of costs of all cuts. Return the minimum total cost of the cuts.

Input: n = 7, cuts = [1,3,4,5]

Output: 16

![img](https://assets.leetcode.com/uploads/2020/07/23/e1.jpg)

```cpp
/* 
    Here i and j are the endpoints of stick (used for calculating cost at every state by computing stick len) 
    whereas start and end are pointers to mark the boundaries over cuts vector
*/

int solve (vector<int> & cuts, int i, int j, int start, int end, vector<vector<int>> & dp){

    if(start>end) return 0;     // --> Base condition is when no cutpoints left

    if(dp[start][end]!=-1) return dp[start][end];

    int ans = INT_MAX;

    for(int k=start; k<=end; k++){

        int temp = solve(cuts, i, cuts[k], start, k-1, dp) + solve(cuts, cuts[k], j, k+1, end, dp) + j-i;
        ans = min(ans, temp);
    }

    return dp[start][end] = ans;
}


int minCost(int n, vector<int>& cuts) {
    int len = cuts.size();

    /* We sort the cuts vector so we can easily itearte for cut points for any part of the stick */

    sort(cuts.begin(), cuts.end());   

    vector<vector<int>> dp (len, vector<int> (len, -1));
    return solve (cuts, 0, n, 0, len-1, dp);
}
```

<br>

### 5. Scramble String
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

### 6. Super Egg Drop
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

<br>

### 7. Boolean Parenthesization 
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

<br>

### 8. Different Ways to Add Parentheses
Given a string expression of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. You may return the answer in any order (repeated answers should also be returned).

Input: expression = "2-1-1"

Output: [0,2]

Explanation: ((2-1)-1) = 0,  (2-(1-1)) = 2

```cpp
int perform_operation (int num1, int num2, char op){
    if(op=='+') return num1+num2;
    else if(op=='-') return num1-num2;
    else return num1*num2;
}


vector<int> calculate (vector<int> & left, vector<int> & right, char op){
    vector<int> v;

    for(int num1 : left){
        for(int num2 : right){
            int ans = perform_operation (num1, num2, op);
            v.push_back(ans);
        }
    }

    return v;
}


bool baseCase (string & expression, int i, int j){
    while(i<=j){
        if(expression[i]=='+' or expression[i]=='-' or expression[i]=='*') return false;
        i++;
    }
    return true;
}


vector<int> solve (string & expression, int i, int j, vector<vector<vector<int>>> & dp){

    if(dp[i][j].size()!=0) return dp[i][j];     // --> Since datatype is vector, we will check it by size

    /* baseCase function return true when no operator lies between i and j */

    if(baseCase(expression, i, j)){
        string num = expression.substr(i, j-i+1);
        return {stoi(num)};
    } 

    vector<int> ans;

    for(int k=i+1; k<j; k++){

        /* We will only break at operator, so we'll continue if curr_char is not an operator */

        if(expression[k]!='+' and expression[k]!='-' and expression[k]!='*') continue;

        /* left and right are vectors of multiple solutions of subproblems */

        auto left = solve (expression, i, k-1, dp);
        auto right = solve (expression, k+1, j, dp);

        /* We will compute temp by solving every left with every right (N2 loop) using calculate function */

        auto temp = calculate (left, right, expression[k]);

        ans.insert(ans.end(), temp.begin(), temp.end());   // --> and we will append all answer to the ans vector
    }

    return dp[i][j] = ans;
}


vector<int> diffWaysToCompute(string & expression) {
    int n = expression.size();

    /* Here we will store whole vector inside dp cell */

    vector<vector<vector<int>>> dp (n+1, vector<vector<int>> (n+1, vector<int>()));

    return solve (expression, 0, n-1, dp);
}
```






