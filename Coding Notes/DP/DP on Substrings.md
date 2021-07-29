# Problems based on DP on Substrings

### 2. Longest Palindromic Substring
Given a string s, return the longest palindromic substring in s.

Input: s = "babad"

Output: "bab"

Hint: Avoid redundant complexity of checking palindrome for substrings of all sizes.  

```cpp
/* dp[i][j] denotes whether a substring starts from i and end at j is palindrome or not */

string solve (string & s, int n, vector<vector<bool>> & dp){
    int substr_size = 1;
    int starting_index = 0;
    int ans_len = 0;

    /* Iterating for substring for all sizes starting from 2 */

    while(substr_size<=n){

        for(int i=0; i<=n-substr_size; i++){
            int j = i+substr_size-1;

            /* if starting and ending char of substr is same then we will check for inner substring */

            if(s[i]==s[j]){

                /* start and end describes the boundary of inner substring */

                int start = i+1, end = j-1;

                /* If there is only one or zero char in inner substr then substring is palindrome */ 

                if(start>=end) 
                    dp[i][j] = true;

                /* Else if inner substr is palindrome then also outer substring is palindrome */ 

                else if(dp[start][end])
                    dp[i][j] = true;

                else dp[i][j] = false;

                /* update ans */

                if(dp[i][j] and j-i+1 > ans_len){
                    ans_len = j-i+1;
                    starting_index = i;
                }
            }
        }

        substr_size++;
    }

    string res = s.substr(starting_index, ans_len);
    return res;
}


string longestPalindrome(string s) {
    int n = s.length();
    if(n==1) return s;

    vector<vector<bool>> dp (n+1, vector<bool> (n+1, false));

    return solve (s, n, dp);
}
```
