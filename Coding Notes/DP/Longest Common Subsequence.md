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

### 1. Longest Common Substring
Given two strings. The task is to find the length of the longest common substring.

Input: S1 = "ABCDGH", S2 = "ACDGHR"

Output: 4

```cpp
int longestCommonSubstr (string s1, string s2, int n, int m){
    vector<vector<int>> dp (n+1, vector<int> (m+1, 0));
    int ans = 0;

    for(int i=1; i<=n; i++){
        for(int j=1; j<=m; j++){
            
            /* When curr_char of s1 == curr_char of s2, fill cell with 1 + top-left diagonal */
            
            if(s1[i-1]==s2[j-1])
                dp[i][j] = 1 + dp[i-1][j-1];

            ans = max(ans, dp[i][j]);       // --> Ans will be max value in dp table                     
        }
    }

    return ans;
}
```

### 2. Print Longest Common Subsequence
Given two sequences, print the longest subsequence present in both of them. LCS for input Sequences “AGGTAB” and “GXTXAYB” is “GTAB” of length 4.

```cpp
string printLCS (string & s1, string & s2){
    int n = s1.length(), m = s2.length();
    vector<vector<int>> dp (n+1, vector<int> (m+1, 0));

    for(int i=1; i<=n; i++){
        for(int j=1; j<=m; j++){
            if(s1[i-1]==s2[j-1])
                dp[i][j] = 1 + dp[i-1][j-1];
            else
                dp[i][j] = max (dp[i-1][j], dp[i][j-1]);
        }
    }

    /* --------------------- Printing Logic ----------------------- */

    int i = n, j = m;
    string lcs = "";

    while(i>0 and j>0){

        if(s1[i-1]==s2[j-1]){
            lcs.push_back(s1[i-1]);
            i--; j--;
        }
        else{
            if(dp[i-1][j] > dp[i][j-1]) i--;
            else j--;
        }
    }

    reverse(lcs.begin(), lcs.end());
    return lcs;
}
```

### 3. Shortest Common Supersequence
Given two strings s1 and s2 of lengths m and n respectively, find the length of the smallest string which has both, s1 and s2 as its sub-sequences.

Input: s1 = abcd, s2 = xycd

Output: 6

Hint: len(supersequence) = len(s1) + len(s2) - lcs_len

```cpp
int lcs (string & s1, string & s2, int m, int n){
    vector<vector<int>> dp (m+1, vector<int> (n+1, 0));

    for(int i=1; i<=m; i++){
        for(int j=1; j<=n; j++){

            if(s1[i-1]==s2[j-1])
                dp[i][j] = 1 + dp[i-1][j-1];
            else
                dp[i][j] = max (dp[i-1][j], dp[i][j-1]);
        }
    }

    return dp[m][n];
}

int shortestCommonSupersequence(string s1, string s2, int m, int n){
    int lcs_len = lcs (s1, s2, m, n);
    return m+n-lcs_len;
}
```

### 4. Print Shortest Common Supersequence
Given two strings s1 and s2, return the shortest string that has both str1 and str2 as subsequences.  If multiple answers exist, you may return any of them.

Input: str1 = "abac", str2 = "cab"

Output: "cabac"

```cpp
string shortestCommonSupersequence(string s1, string s2) {
    int n = s1.size(), m = s2.size();
    vector<vector<int>> dp (n+1, vector<int> (m+1, 0));

    for(int i=1; i<=n; i++){
        for(int j=1; j<=m; j++){

            if(s1[i-1]==s2[j-1])
                dp[i][j] = 1 + dp[i-1][j-1];
            else
                dp[i][j] = max (dp[i-1][j], dp[i][j-1]);
        }
    }

    /* ---------------- Print Supersequence Logic ----------------- */

    int i = n, j = m;
    string supersequence = "";

    while(i>0 and j>0){

        /* If both char are equal, push it in string one time and move to top-left diagonal cell */

        if(s1[i-1]==s2[j-1]){
            supersequence.push_back(s1[i-1]);
            i--; j--;
        }

        /* Move towards left or up whichever is max, and push the char of the row or col which we are leaving */

        else{

            if(dp[i-1][j] > dp[i][j-1]){
                supersequence.push_back(s1[i-1]);
                i--;
            }
            else{
                supersequence.push_back(s2[j-1]);
                j--;
            }
        }
    }

    /* Don't forget to pushback remaining chars of s1 and s2 */

    while(i>0){
        supersequence.push_back(s1[i-1]);
        i--;
    }

    while(j>0){
        supersequence.push_back(s2[j-1]);
        j--;
    }

    reverse(supersequence.begin(), supersequence.end());
    return supersequence;
}
```

### 5. Minimum number of deletions and insertions to transform one string into another
Given two strings ‘s1’ and ‘s2’ of size m and n respectively. The task is to remove and insert the minimum number of characters from s1 to transform it into s2. It could be possible that the same character needs to be removed from one point of s1 and inserted to some another point.

![img](https://media.geeksforgeeks.org/wp-content/uploads/20200817135845/picture2-660x402.jpg)

Hint: Total operations = len(s1) + len(s2) - (2*lcs_len)

```cpp
int lcs (string & s1, string & s2, int m, int n){
    vector<vector<int>> dp (m+1, vector<int> (n+1, 0));

    for(int i=1; i<=m; i++){
        for(int j=1; j<=n; j++){

            if(s1[i-1]==s2[j-1])
                dp[i][j] = 1 + dp[i-1][j-1];
            else
                dp[i][j] = max (dp[i-1][j], dp[i][j-1]);
        }
    }

    return dp[m][n];
}

int minOperations(string s1, string s2) { 
    int m = s1.length(), n = s2.length();
    int lcs_len = lcs(s1, s2, m, n);

    return m+n-(2*lcs_len);
} 
```
