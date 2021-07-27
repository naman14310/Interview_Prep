# Problems based on making choices at each index

### 1. Count Sorted Vowel Strings
Given an integer n, return the number of strings of length n that consist only of vowels (a, e, i, o, u) and are lexicographically sorted. A string s is lexicographically sorted if for all valid i, s[i] is the same as or comes before s[i+1] in the alphabet.

Input: n = 2
Output: 15 (["aa","ae","ai","ao","au","ee","ei","eo","eu","ii","io","iu","oo","ou","uu"])

```cpp
int solve (int n, int idx, int choices, vector<vector<int>> & dp){
    if(idx==n) return 1;

    if(dp[idx][choices]!=-1) return dp[idx][choices];

    int ans = 0;

    for(int i=choices; i<5; i++)
        ans += solve(n, idx+1, i, dp);

    return dp[idx][choices] = ans;
}

int countVowelStrings(int n) {
    vector<vector<int>> dp (n+1, vector<int> (5, -1));
    return solve (n, 0, 0, dp);        
}
```

### 2. Count Number of Teams
There are n soldiers standing in a line. Each soldier is assigned a unique rating value. You have to form a team of 3 soldiers amongst them under the following rules:
1. Choose 3 soldiers with index (i, j, k) with rating (rating[i], rating[j], rating[k]).
2. A team is valid if: (rating[i] < rating[j] < rating[k]) or (rating[i] > rating[j] > rating[k]) where (0 <= i < j < k < n).

Return the number of teams you can form given the conditions. (soldiers can be part of multiple teams).

Input: rating = [2,5,3,4,1]

Output: 3

```cpp

/* Here our solution depends on 3 factors i.e. idx, cnt, pattern so we'll create 3D DP */

int solve (vector<int> & rating, int idx, int cnt, int pattern, int prev, vector<vector<vector<int>>>& dp){
    if(cnt==3) return 1;

    if(dp[idx][cnt][pattern]!=-1) return dp[idx][cnt][pattern];

    int ans = 0;

    for(int i=idx; i<rating.size(); i++){

        /* pattern == 1 : we need to choose decreasing */

        if(pattern==1){
            if(rating[i]<prev)
                ans += solve (rating, i+1, cnt+1, 1, rating[i], dp);
        }

        /* pattern == 2 : we need to choose increasing */

        else if(pattern==2){
            if(rating[i]>prev)
                ans += solve (rating, i+1, cnt+1, 2, rating[i], dp);
        }

        /* pattern == 0 : else we have both options to choose */

        else{
            ans += solve (rating, i+1, 1, 1, rating[i], dp);
            ans += solve (rating, i+1, 1, 2, rating[i], dp);
        }

    }

    return dp[idx][cnt][pattern] = ans;
}


int numTeams(vector<int>& rating) {
    int n = rating.size();
    vector<vector<vector<int>>> dp (n+1, vector<vector<int>> (3, vector<int> (3, -1)));
    return solve (rating, 0, 0, 0, 0, dp);
}
```
