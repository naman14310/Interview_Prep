# DP with Bit Masking

<br>

### 1. Can I Win
In the "100 game" two players take turns adding, to a running total, any integer from 1 to 10. The player who first causes the running total to reach or exceed 100 wins. For example, two players might take turns drawing from a common pool of numbers from 1 to 15 without replacement until they reach a total >= 100. Given two integers maxChoosableInteger and desiredTotal, return true if the first player to move can force a win, otherwise, return false. Assume both players play optimally.

**Bitmasking using string**
```cpp
bool solve (int maxChoosableInteger, int desiredTotal, bool player1, int sum, unordered_map<string, bool> & dp, string & used){
    if(dp.find(used)!=dp.end()) 
        return dp[used];

    for(int i=maxChoosableInteger; i>=1; i--){   
        if(used[i]=='1') continue;

        if(sum+i>=desiredTotal) 
            return player1;

        used[i] = '1';
        bool res = solve (maxChoosableInteger, desiredTotal, !player1, sum+i, dp, used);
        used[i] = '0';

        if(player1 and res) return dp[used] = true;

        if(!player1 and !res) return dp[used] = false;    
    }

    return dp[used] = player1 ? false : true;
}


bool canIWin(int maxChoosableInteger, int desiredTotal) {
    int maxPossible = maxChoosableInteger * (maxChoosableInteger+1) /2;
    if(maxPossible < desiredTotal) return false;

    unordered_map<string, bool> dp;
    string used = "000000000000000000000";

    return solve (maxChoosableInteger, desiredTotal, true, 0, dp, used);
}
```

**Bitmasking using Bit manipulations (Faster)**

```cpp
bool solve (int maxChoosableInteger, int desiredTotal, bool player1, int sum, unordered_map<int,bool> & dp, int key, vector<int> & pow){
    if(dp.find(key)!=dp.end()) 
        return dp[key];

    for(int i=1; i<=maxChoosableInteger; i++){   
        if((key&pow[i])!=0) continue;

        if(sum+i>=desiredTotal) 
            return player1;

        key += pow[i];
        bool res = solve (maxChoosableInteger, desiredTotal, !player1, sum+i, dp, key, pow);
        key -= pow[i];

        if(player1 and res) return dp[key] = true;

        if(!player1 and !res) return dp[key] = false;    
    }

    return dp[key] = player1 ? false : true;
}


bool canIWin(int maxChoosableInteger, int desiredTotal) {
    int maxPossible = maxChoosableInteger * (maxChoosableInteger+1) /2;
    if(maxPossible < desiredTotal) return false;

    /* precomputing power of 2 for avoiding repeatitive calculations */
    
    vector<int> pow(21, 1);
    int curr = 2;
    for(int i=1; i<=20; i++){
        pow[i] = curr;
        curr *= 2;
    }

    unordered_map<int,bool> dp;
    return solve (maxChoosableInteger, desiredTotal, true, 0, dp, 0, pow);
}
```
