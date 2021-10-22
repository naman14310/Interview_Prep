# Game Playing

<br>

### 1. Nim Game
Initially, there is a heap of stones on the table. You and your friend will alternate taking turns, and you go first. On each turn, the person whose turn it is will remove 1 to 3 stones from the heap. The one who removes the last stone is the winner. Given n, the number of stones in the heap, return true if you can win the game assuming both you and your friend play optimally, otherwise return false.

```cpp
bool canWinNim(int n) {
    vector<bool> res (n+1, true);

    for(int i=4; i<=n; i++)
        res[i] = !res[i-1] or !res[i-2] or !res[i-3];

    return res[n];
}
```

#### Approach 2: [Constant Space Solution](https://leetcode.com/problems/nim-game/discuss/73760/One-line-O(1)-solution-and-explanation)

```cpp
bool canWinNim(int n) {
    return n%4;
}
```

<br>

### 2. Stone Game (Predict the Winner)
Alex and Lee play a game with piles of stones. The objective of the game is to end with the most stones. The total number of stones is odd, so there are no ties. Alex and Lee take turns, with Alex starting first. Each turn, a player takes the entire pile of stones from either the beginning or the end of the row.  This continues until there are no more piles left. Assuming Alex and Lee play optimally, return True if and only if Alex wins the game.

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

<br>

### 3. Stone Game II
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

<br>

### 4. Stone Game III
There are several stones arranged in a row, and each stone has an associated value which is an integer given in the array stoneValue. Alice and Bob take turns, with Alice starting first. On each player's turn, that player can take 1, 2 or 3 stones from the first remaining stones in the row. The score of each player is the sum of values of the stones taken. The score of each player is 0 initially.

The objective of the game is to end with the highest score, and the winner is the player with the highest score and there could be a tie. The game continues until all the stones have been taken. Assume Alice and Bob play optimally. Return "Alice" if Alice will win, "Bob" if Bob will win or "Tie" if they end the game with the same score.

Input: values = [1,2,3,7]

Output: "Bob"

```cpp
/* This function will return index of mx_score from temp vector according to the player's chance */ 

int getMaxScore_idx (vector<pair<int,int>> & temp, bool alice){

    if(alice){
        int mx_score = temp[0].first;
        int mx_idx = 0;

        for(int i=1; i<temp.size(); i++){
            if(temp[i].first > mx_score){
                mx_score = temp[i].first;
                mx_idx = i; 
            }
        }

        return mx_idx;
    }
    else{
        int mx_score = temp[0].second;
        int mx_idx = 0;

        for(int i=1; i<temp.size(); i++){
            if(temp[i].second > mx_score){
                mx_score = temp[i].second;
                mx_idx = i; 
            }
        }

        return mx_idx;
    }
}


pair<int,int> solve (vector<int>& stoneValue, int n, int idx, bool alice, vector<vector<pair<int,int>>> & dp){
    if(idx>=n) return {0,0};

    if(dp[idx][alice].first!=-1) return dp[idx][alice];

    pair<int,int> ans = {0,0};
    vector<pair<int,int>> temp;

    /* Iterating for all choices using for loop and saving each result in temp vector */

    for(int i=idx; i<min(idx+3, n); i++){

        auto res = solve (stoneValue, n, i+1, !alice, dp);
        temp.push_back(res);
    }


    /* If curr player is alice, then she will choose the score according to her mx score */

    if(alice){

        for(int i=0; i<temp.size(); i++){
            for(int j=0; j<=i; j++)
                temp[i].first += stoneValue[idx+j];
        }

        int mx_idx = getMaxScore_idx (temp, alice);
        ans = temp[mx_idx];
    }

    /* If curr player is bob, then he will choose the score according to his mx score */

    else{

        for(int i=0; i<temp.size(); i++){
            for(int j=0; j<=i; j++)
                temp[i].second += stoneValue[idx+j];
        }

        int mx_idx = getMaxScore_idx (temp, alice);
        ans = temp[mx_idx];
    }

    return dp[idx][alice] = ans;
}


string stoneGameIII(vector<int>& stoneValue) {
    int n = stoneValue.size();
    vector<vector<pair<int,int>>> dp (n, vector<pair<int,int>> (2, {-1, -1}));

    auto score = solve (stoneValue, n, 0, true, dp);

    if(score.first>score.second)
        return "Alice";

    else if(score.first<score.second)
        return "Bob";

    else return "Tie";
}
```

<br>

### 5. Stone Game IV
Initially, there are n stones in a pile. On each player's turn, that player makes a move consisting of removing any non-zero square number of stones in the pile. Also, if a player cannot make a move, he/she loses the game. Given a positive integer n. Return True if and only if Alice wins the game otherwise return False, assuming both players play optimally with Alice starting first.

Input: n = 4

Output: true

```cpp
/*
    Our function will return:
    True if Alice wins 
    False if Bob wins
*/


bool solve (int n, vector<int> & sqrs, bool alice, vector<vector<int>> & dp){

    if(dp[n][alice]!=-1) return dp[n][alice];

    for(int i=0; i<sqrs.size(); i++){
        if(sqrs[i]>n) return !alice;
        if(sqrs[i]==n) return alice;

        bool res = solve (n-sqrs[i], sqrs, !alice, dp);

        if(alice and res)
            return dp[n][alice] = true;

        if(!alice and !res) 
            return dp[n][alice] = false;
    }

    return dp[n][alice] = alice ? false : true;
}


bool winnerSquareGame(int n) {
    vector<int> sqrs;
    vector<vector<int>> dp (n+1, vector<int> (2, -1));

    for(int i=1; i*i<=n; i++)
        sqrs.push_back(i*i);

    return solve (n, sqrs, true, dp);
}
```

<br>

### 6. Stone Game VII
Alice and Bob take turns playing a game, with Alice starting first.

There are n stones arranged in a row. On each player's turn, they can remove either the leftmost stone or the rightmost stone from the row and receive points equal to the sum of the remaining stones' values in the row. The winner is the one with the higher score when there are no stones left to remove.

Bob found that he will always lose this game, so he decided to minimize the score's difference. Alice's goal is to maximize the difference in the score. Return the difference in Alice and Bob's score if they both play optimally.

Input: stones = [5,3,1,4,2]

Output: 6

```cpp
pair<int, int> solve (vector<int> & stones, int start, int end, bool alice, int totalSum, vector<vector<pair<int,int>>> & dp){
    if (start>end) return {0, 0};

    if(dp[start][end].first!=-1) return dp[start][end];

    pair<int,int> ans;

    auto res1 = solve (stones, start+1, end, !alice, totalSum-stones[start], dp);
    auto res2 = solve (stones, start, end-1, !alice, totalSum-stones[end], dp);

    if(alice){
        res1.first += totalSum - stones[start];
        res2.first += totalSum - stones[end];

        /* Deciding criteria for Alice will be the maxDifference */ 

        if(res1.first-res1.second > res2.first-res2.second) ans = res1;
        else ans = res2;
    }
    else{
        res1.second += totalSum - stones[start];
        res2.second += totalSum - stones[end];

        /* Deciding criteria for Bob will be the minDifference */ 

        if(res1.first-res1.second < res2.first-res2.second) ans = res1;
        else ans = res2;
    }

    return dp[start][end] = ans;
}


int stoneGameVII(vector<int>& stones) {
    int n = stones.size();
    vector<vector<pair<int,int>>> dp (n+1, vector<pair<int,int>> (n+1, {-1,-1}));
    int totalSum = 0;

    for(int stn : stones)
        totalSum += stn;

    auto res = solve (stones, 0, n-1, true, totalSum, dp);
    return res.first - res.second;
}
```

