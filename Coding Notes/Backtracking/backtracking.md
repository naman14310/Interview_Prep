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

#### 3. Permutations II
Given a collection of numbers, nums, that might contain duplicates, return all possible unique permutations in any order.

Input: nums = [1,1,2]

Output: [[1,1,2], [1,2,1], [2,1,1]]

Hint: Use sorting beforehand for avoiding duplicates

```cpp
void solve (vector<int> & nums, int idx, vector<vector<int>> & res, vector<int> & v, vector<bool> & vis){
    if(idx==nums.size()){
        res.push_back(v);
        return;
    }

    int prev = INT_MIN;

    for(int i = 0; i<nums.size(); i++){

        if(prev==nums[i]) continue;       // Same number should not be filled on same index more then once

        if(!vis[i]){

            v[idx] = nums[i];
            vis[i] = true;

            solve (nums, idx+1, res, v, vis);

            vis[i] = false;
            prev = nums[i];
        }
    }
}


vector<vector<int>> permuteUnique(vector<int>& nums) {
    vector<vector<int>> res;
    vector<int> v (nums.size(), 0);
    vector<bool> vis (nums.size(), false);

    /* Sorting should be done to prevent duplicate permutations */

    sort(nums.begin(), nums.end());

    solve (nums, 0, res, v, vis);
    return res;
}
```

#### 4. Subsets
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

#### 5. Letter Tile Possibilities
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

#### 6. Generate Parentheses
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

#### 7. Combinations
Given two integers n and k, return all possible combinations of k numbers out of the range [1, n].

Input: n = 4, k = 2

Output:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]

```cpp
void solve (int n, int k, int idx, vector<vector<int>> & res, vector<int> & v){
    if(v.size()==k){
        res.push_back(v);
        return;
    }

    for(int i=idx; i<=n; i++){
        v.push_back(i);
        solve (n, k, i+1, res, v);
        v.pop_back();
    }
}


vector<vector<int>> combine(int n, int k) {
    vector<vector<int>> res;
    vector<int> v;

    solve(n, k, 1, res, v);
    return res;
}
```


#### 8. Combination Sum (Same Number can be choosen unlimited times)
Given an array of distinct integers candidates and a target integer target, return a list of all unique combinations of candidates where the chosen numbers sum to target. The same number may be chosen from candidates an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

Input: candidates = [2,3,6,7], target = 7

Output: [[2,2,3],[7]]

```cpp
int getSum(vector<int> & v){
    int sum = 0;

    for(int i : v)
        sum += i;

    return sum;
}

void solve (vector<int> & candidates, int target, int idx, vector<vector<int>> & res, vector<int> & v){
    if(idx==candidates.size()) return;

    int sum = getSum(v);

    if(sum>target) return;
    if(sum==target) res.push_back(v);

    /* Here we have a choice to fill candidates[idx, candidates.size()] to fill at every pos, where idx is same as prev number */

    for(int i=idx; i<candidates.size(); i++){
        v.push_back(candidates[i]);
        solve(candidates, target, i, res, v);
        v.pop_back();
    }
}

vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
    vector<vector<int>> res;
    vector<int> v;

    solve(candidates, target, 0, res, v);
    return res;
}
```

#### 9. Combination Sum II (Each number should be only used once + Does not conatain duplicate combinations)
Given a collection of candidate numbers (candidates) and a target number (target), find all unique combinations in candidates where the candidate numbers sum to target. Each number in candidates may only be used once in the combination.

Note: The solution set must not contain duplicate combinations.

Input: candidates = [10,1,2,7,6,1,5], target = 8

Output: 
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]

Hint: Use Sorting beforehand for avoiding duplicates

```cpp
int getSum(vector<int> & v){
    int sum = 0;
    for(int i : v)
        sum += i;

    return sum;
}

void solve (vector<int> & candidates, int target, int idx, vector<vector<int>> & res, vector<int> & v){
    int sum = getSum(v);
    if(sum>target) return;

    if(sum==target){
        res.push_back(v);
        return;
    }

    int prev = INT_MIN;

    for(int i=idx; i<candidates.size(); i++){
        if(candidates[i]==prev) continue;      // --> dont put same number at same index more then once

        v.push_back(candidates[i]);
        prev = candidates[i];

        solve (candidates, target, i+1, res, v);
        v.pop_back();
    }
}

vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
    vector<vector<int>> res;
    vector<int> v;

    /* Sort candidates array for removing duplicates */

    sort(candidates.begin(), candidates.end());

    solve (candidates, target, 0, res, v);
    return res;
}
```

#### 10. Combination Sum III
Find all valid combinations of k numbers that sum up to n such that the following conditions are true:
1. Only numbers 1 through 9 are used.
2. Each number is used at most once.

Return a list of all possible valid combinations. The list must not contain the same combination twice, and the combinations may be returned in any order.

Input: k = 3, n = 9

Output: [[1,2,6],[1,3,5],[2,3,4]]

```cpp
int getSum (vector<int> & v){
    int sum = 0;

    for(int i : v)
        sum += i;

    return sum;
}


void solve (int k, int target, int idx, vector<vector<int>> & res, vector<int> & v){
    if(v.size()==k){
        if(getSum(v)==target)
            res.push_back(v);
        return;
    }

    /* we can choose any number for index i in vector v in range between [idx-9] where idx is one greater then the prev number */ 

    for(int i=idx; i<=9; i++){
        v.push_back(i);
        solve (k, target, i+1, res, v);
        v.pop_back();
    }
}


vector<vector<int>> combinationSum3(int k, int n) {
    vector<vector<int>> res;
    vector<int> v;

    solve(k, n, 1, res, v);
    return res;
}
```

#### 11. Letter Combinations of a Phone Number
Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

Input: digits = "23"

Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]

```cpp
void solve (string & digits, int idx, vector<string> & res, string & s, unordered_map<char, string> & mp){
    if(idx==digits.length()){
        res.push_back(s);
        return;
    }

    for(char ch : mp[digits[idx]]){
        s.push_back(ch);
        solve (digits, idx+1, res, s, mp);
        s.pop_back();
    }
}

vector<string> letterCombinations(string digits) {
    if(digits.size()==0)
        return vector<string> ();

    unordered_map<char, string> mp = { {'2', "abc"}, {'3', "def"}, {'4', "ghi"}, {'5', "jkl"}, {'6', "mno"}, {'7', "pqrs"}, {'8', "tuv"}, {'9', "wxyz"} };

    vector<string> res;
    string s = "";

    solve (digits, 0, res, s, mp);

    return res;
}
```

#### 12. Palindrome Partitioning
Given a string s, partition s such that every substring of the partition is a palindrome. Return all possible palindrome partitioning of s.

Input: s = "aab"

Output: [["a","a","b"],["aa","b"]]

```cpp
bool isPalindrome (string& s){
    if(s.size()==0) return false;

    int i=0, j=s.length()-1;
    while(i<j){
        if(s[i]!=s[j]) return false;
        i++; j--;
    }
    return true;
}

void solve (string s, int idx, vector<vector<string>>& res, vector<string> & v){
    if(idx==s.length()){
        res.push_back(v);
        return;
    }

    string temp = "";

    /* pushing char in temp one by one */

    for(int i=idx; i<s.length(); i++){
        temp.push_back(s[i]);

        /* If temp is palindrome, then push it in vector v and recurse for remaining string */

        if(isPalindrome(temp)){
            v.push_back(temp);
            solve (s, i+1, res, v);
            v.pop_back();
        }
    }
}

vector<vector<string>> partition(string s) {
    vector<vector<string>> res;
    vector<string> v;

    solve (s, 0, res, v);
    return res;
}
```

#### 13. Decode String
Given an encoded string, return its decoded string. The encoding rule is: k[encoded_string], where the encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.

Input: s = "3[a]2[bc]", Output: "aaabcbc"

Input: s = "3[a2[c]]", Output: "accaccacc"

```cpp
/* Take special care with "idx" --> that will act as a global index */

string solve (string & s, int & idx){
    string ans = "";

    for(int i=idx; i<s.length(); i++){

        if(isalpha(s[i]))
            ans.push_back(s[i]);

        else if (s[i]==']'){
            idx = i;              // --> Before returning on ']', update the global idx
            return ans;
        }

        else{

            /* integer can be of more then 1 digit (eg: 100) , so handle that also */

            string freq_s = "";
            while(s[i]!='['){
                freq_s.push_back(s[i]);
                i++;
            }

            int freq = stoi(freq_s);   
            idx = i+1;

            string temp = solve(s, idx); 

            for(int f=0; f<freq; f++)
                ans += temp;

            i = idx;   // --> after getting res from recursive call, update i to global idx
        }
    }

    return ans;
}


string decodeString(string s) {
    int idx = 0;
    return solve(s, idx);
}
```


## 2D Problems

#### 1. Path with Maximum Gold
In a gold mine grid of size m x n, each cell in this mine has an integer representing the amount of gold in that cell, 0 if it is empty. Return the maximum amount of gold you can collect under the conditions:

1. Every time you are located in a cell you will collect all the gold in that cell.
2. From your position, you can walk one step to the left, right, up, or down.
3. You can't visit the same cell more than once.
4. Never visit a cell with 0 gold.
5. You can start and stop collecting gold from any position in the grid that has some gold.

```cpp
bool isInside(int x, int y, int row, int col){
    return x>=0 and y>=0 and x<row and y<col;
}


int backtrack (vector<vector<int>>& grid, int row, int col, int x, int y, vector<vector<bool>> & curr_path){
    int gold = 0;
    curr_path[x][y] = true; 

    int dx[] = {1, 0, -1, 0};
    int dy[] = {0, 1, 0, -1};

    for(int i=0; i<4; i++){
        int xnew = x + dx[i];
        int ynew = y + dy[i];

        if(isInside(xnew, ynew, row, col) and grid[xnew][ynew]!=0 and !curr_path[xnew][ynew])
            gold = max(gold, backtrack(grid, row, col, xnew, ynew, curr_path));
    }

    gold += grid[x][y];

    curr_path[x][y] = false;
    return gold;
}


int getMaximumGold(vector<vector<int>>& grid) {
    int row = grid.size(), col = grid[0].size();
    vector<vector<bool>> curr_path (row, vector<bool> (col, false));
    int max_gold = 0;

    for(int i=0; i<row; i++){
        for(int j=0; j<col; j++){

            if(grid[i][j]!=0)
                max_gold = max(max_gold, backtrack(grid, row, col, i, j, curr_path));
        }
    }

    return max_gold;
}
```
