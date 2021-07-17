# Recursive + Memorization (DP)

#### 1. Predict the Winner
You are given an integer array nums. Two players are playing a game with this array. Player 1 and player 2 take turns, with player 1 starting first. Both players start the game with a score of 0. At each turn, the player takes one of the numbers from either end of the array (i.e., nums[0] or nums[nums.length - 1]) which reduces the size of the array by 1. The player adds the chosen number to their score. The game ends when there are no more elements in the array.

Return true if Player 1 can win the game. If the scores of both players are equal, then player 1 is still the winner, and you should also return true. You may assume that both players are playing optimally.

Input: nums = [1,5,233,7]

Output: true

```cpp
pair<int, int> solve (vector<int> & nums, bool player1, int start, int end, unordered_map<string, pair<int,int>> & dp){
    if(start>end) return {0,0};

    string key = to_string(start) + "-" + to_string(end);
    if(dp.find(key)!=dp.end())
        return dp[key];

    int p1_score = 0, p2_score = 0;

    /* Player has two choices, either to pick from start or from end */

    auto res1 = solve (nums, !player1, start, end-1, dp);
    auto res2 = solve (nums, !player1, start+1, end, dp);

    /* If current player is P1 then decision will be made by P1 (res.first) */

    if(player1){
        res1.first += nums[end];
        res2.first += nums[start];

        if(res1.first>res2.first){
            p1_score += res1.first;
            p2_score += res1.second;
        }
        else{
            p1_score += res2.first;
            p2_score += res2.second;
        }            
    }

    /* Else if current player is P2 then decision will made by P2 (res.second) */

    else{
        res1.second += nums[end];
        res2.second += nums[start];

        if(res1.second>res2.second){
            p1_score += res1.first;
            p2_score += res1.second;
        }
        else{
            p1_score += res2.first;
            p2_score += res2.second;
        }
    }

    return dp[key] = {p1_score, p2_score};
}


bool PredictTheWinner(vector<int>& nums) {
    unordered_map<string, pair<int,int>> dp;

    auto score = solve (nums, true, 0, nums.size()-1, dp);
    return score.first>=score.second;
}
```
