# Problems on LCS Pattern

### @ Longest Common Subsequence (Parent Problem)
Given two strings s1 and s2, return the length of their longest common subsequence.

**Method 1 : Memorization (Top-Down)**

```cpp
int solve (string & s1, string & s2, int i, int j, vector<vector<int>>& dp){

    /* Base condition is when any ptr reaches at the end of string */

    if(i==s1.length() or j==s2.length()) return 0;   

    if(dp[i][j]!=-1) return dp[i][j];

    /* If curr_char of s1 is equal to curr_char of s2 then we add 1 and increment both i and j */

    if(s1[i]==s2[j]){
        int res = 1 + solve(s1, s2, i+1, j+1, dp);
        return dp[i][j] = res;
    }

    /* Else we have two choices, either to increment i or increment j */

    else{
        int res1 = solve (s1, s2, i+1, j, dp);
        int res2 = solve (s1, s2, i, j+1, dp);
        return dp[i][j] = max(res1, res2);
    }
}


int longestCommonSubsequence(string text1, string text2) {
    vector<vector<int>> dp (text1.size()+1, vector<int> (text2.size()+1, -1));
    return solve (text1, text2, 0, 0, dp);
}
```

**Method 2 : Tabulation (Bottom-Up)**

```cpp
int longestCommonSubsequence(string s1, string s2) {

    /* Initialize first row and col with 0 */

    vector<vector<int>> dp (s1.length()+1, vector<int> (s2.length()+1, 0));

    for(int i=1; i<=s1.length(); i++){
        for(int j=1; j<=s2.length(); j++){

            if(s1[i-1]==s2[j-1])
                dp[i][j] = 1 + dp[i-1][j-1];                    // -- 1 + top-left diagonal

            else
                dp[i][j] = max (dp[i-1][j], dp[i][j-1]);        // --> max (top, left)
        }
    }

    return dp[s1.length()][s2.length()];
}
```
