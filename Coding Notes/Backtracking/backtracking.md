# Recursion & Backtracking 

## 1D Problems

#### 1. Letter Case Permutation
Given a string s, we can transform every letter individually to be lowercase or uppercase to create another string. Return a list of all possible strings we could create.

Input: s = "a1b2"

Output: ["a1b2","a1B2","A1b2","A1B2"]

```cpp
void solve (string & ip, string op, int idx, int n, vector<string> & res){
    if(idx==n){
        res.push_back(op);
        return;
    }

    if(isdigit(ip[idx]))
        solve(ip, op, idx+1, n, res);

    else{   
        op[idx] = tolower(op[idx]);
        solve (ip, op, idx+1, n, res);

        op[idx] = toupper(op[idx]);
        solve (ip, op, idx+1, n, res);
    }
}

vector<string> letterCasePermutation(string s) {
    int n = s.length();
    vector<string> res;

    solve (s, s, 0, n, res);
    return res;
}
```

#### 2. Permutations
Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.

Input: nums = [1,2,3]

Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

```cpp
void solve (vector<int> & nums, int idx, vector<vector<int>> & res, vector<int> & v, vector<bool> & vis){
    if(idx==nums.size()){
        res.push_back(v);
        return;
    }
    
    /* we can fill any number at index idx which is not visited yet */  
    
    for(int i=0; i<nums.size(); i++){
        if(!vis[i]){
            v[idx] = nums[i];
            vis[i] = true;

            solve (nums, idx+1, res, v, vis);

            vis[i] = false;
        }
    }
}

vector<vector<int>> permute(vector<int>& nums) {
    vector<vector<int>> res;
    vector<int> v (nums.size(), 0);
    vector<bool> vis (nums.size(), false);

    solve(nums, 0, res, v, vis);
    return res;
}
```

#### 3. Subsets
Given an integer array nums of unique elements, return all possible subsets (the power set). The solution set must not contain duplicate subsets.

Input: nums = [1,2,3]

Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

```cpp
/* we need to choose whether we include element of that pos or not */

void solve (vector<int> & nums, vector<vector<int>> & res, int idx, vector<int> v){
    if(idx==nums.size()){
        res.push_back(v);
        return;
    }

    /* vector without include element at idx*/

    solve (nums, res, idx+1, v);

    /* vector with include element at idx */

    v.push_back(nums[idx]);
    solve (nums, res, idx+1, v);
}

vector<vector<int>> subsets(vector<int>& nums) {
    vector<vector<int>> res;
    vector<int> v;

    solve(nums, res, 0, v);
    return res;
}
```

#### 4. Letter Tile Possibilities
You have n  tiles, where each tile has one letter tiles[i] printed on it. Return the number of possible non-empty sequences of letters you can make using the letters printed on those tiles.

Input: tiles = "AAB",  Output: 8

Explanation: The possible sequences are "A", "B", "AA", "AB", "BA", "AAB", "ABA", "BAA".

```cpp
void solve (vector<int> & freq, int idx, int n, unordered_set<string> & vis, string & s, int & count){
    if(idx==n) return; 

    /* At every pos we have 26 options (A-Z) to fill */

    for(int i=0; i<26; i++){

        /* If freq > 0 that means we can fill it */

        if(freq[i]>0){   

            /* Decrement its freq and push that char in string */

            freq[i]--;
            s.push_back('A'+i);

            /* If this string s is not vis before, mark it vis and increament the count */ 

            if(vis.find(s)==vis.end()){
                vis.insert(s);
                count++;
            }

            /* and simply recurse */

            solve(freq, idx+1, n, vis, s, count);

            /* after coming back, redo the changes */

            s.pop_back();
            freq[i]++;
        }
    }
}

int numTilePossibilities(string tiles) {
    int n = tiles.size();

    vector<int> freq (26, 0);          // --> for storing freq of all chars
    unordered_set<string> vis;         // --> for keeping track of visited strings

    string s = "";
    int count = 0;

    for(char ch : tiles)
        freq[ch-'A']++;

    solve (freq, 0, n, vis, s, count);
    return count;
}
```

#### 5. Generate Parentheses
Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

Input: n = 3

Output: ["((()))","(()())","(())()","()(())","()()()"]

```cpp
void solve (int n, int lc, int rc, string s, vector<string> & res){
    if(lc==n and rc==n){
        res.push_back(s);
        return;
    }

    /* If left bracket count is greater then right bracket count, we can fill both the brackets */

    if(lc>rc){
        if(lc<n) solve(n, lc+1, rc, s+"(", res);
        solve(n, lc, rc+1, s+")", res);
    }

    /* Else we can only fill left bracket */

    else 
        solve(n, lc+1, rc, s+"(", res);
}

vector<string> generateParenthesis(int n) {
    vector<string> res;
    solve(n, 0, 0, "", res);
    return res;
}
```
