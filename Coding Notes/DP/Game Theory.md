# Problems based on Game Playing

## @ Game Theory

### 1. Stone Game (Predict the Winner)
Alex and Lee play a game with piles of stones. The objective of the game is to end with the most stones. The total number of stones is odd, so there are no ties. Alex and Lee take turns, with Alex starting first.  Each turn, a player takes the entire pile of stones from either the beginning or the end of the row.  This continues until there are no more piles left. Assuming Alex and Lee play optimally, return True if and only if Alex wins the game.

Input: piles = [5,3,4,5]

Output: true

```cpp
pair<int,int> solve (vector<int> & piles, int start, int end, bool alex, vector<vector<pair<int,int>>> & dp){
    if(start>end) return {0,0};

    if(dp[start][end].first!=-1) 
        return dp[start][end];

    pair<int, int> ans;

    auto res1 = solve (piles, start+1, end, !alex, dp);
    auto res2 = solve (piles, start, end-1, !alex, dp);

    if(alex){
        res1.first += piles[start];
        res2.first += piles[end];

        if(res1.first>res2.first) ans = res1;
        else ans = res2;
    }
    else{
        res1.second += piles[start];
        res2.second += piles[end];

        if(res1.second>res2.second) ans = res1;
        else ans = res2;
    }

    return dp[start][end] = ans;
}


bool stoneGame(vector<int>& piles) {
    int n = piles.size();
    vector<vector<pair<int,int>>> dp (n+1, vector<pair<int,int>> (n+1, {-1, -1}));

    auto res = solve (piles, 0, n-1, true, dp);
    return res.first > res.second;
}
```

### 2. Stone Game II
There are a number of piles arranged in a row, and each pile has a positive integer number of stones piles[i]. The objective of the game is to end with the most stones. Alice and Bob take turns, with Alice starting first. Initially, M = 1. On each player's turn, that player can take all the stones in the first X remaining piles, where 1 <= X <= 2M. Then, we set M = max(M, X). The game continues until all the stones have been taken. Assuming Alice and Bob play optimally, return the maximum number of stones Alice can get.

Input: piles = [2,7,9,4,4]

Output: 10

```cpp
pair<int,int> solve (vector<int> & piles, int n, int idx, int M, bool alice, unordered_map<string, pair<int,int>> & dp){
    if(idx==piles.size()) return {0,0};
    
    /* In this question, our answer will depend on 3 variables so we will consider all three of them */
    
    string key = to_string(idx) + "," + to_string(M) + "," + to_string(alice);
    if(dp.find(key)!=dp.end()) return dp[key];

    int score = 0;
    pair<int,int> ans = {0,0};

    for(int i=idx; i<min(n, idx+2*M); i++){
        score += piles[i];

        int x = i-idx+1;
        int m = max(M, x);

        auto res = solve (piles, n, i+1, m, !alice, dp);

        if(alice){
            res.first += score;

            if(res.first > ans.first)
                ans = res;
        }
        else{
            res.second += score;

            if(res.second > ans.second)
                ans = res;
        }
    }

    return dp[key] = ans;
}


int stoneGameII(vector<int>& piles) {
    int n = piles.size();
    unordered_map<string, pair<int,int>> dp;

    auto res = solve (piles, n, 0, 1, true, dp);
    return res.first;
}
```
