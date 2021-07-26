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

