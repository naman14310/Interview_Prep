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



