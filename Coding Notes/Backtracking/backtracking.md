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

#### 5. Subsets II (Only in this question, for removing duplicates -> hashset will be used)
Given an integer array nums that may contain duplicates, return all possible subsets (the power set). The solution set must not contain duplicate subsets.

Input: nums = [1,2,2]

Output: [[],[1],[1,2],[1,2,2],[2],[2,2]]

```cpp
bool isExist (vector<int> & v, unordered_set<string> & vis){
    string s = "";

    for(int i : v)
        s.push_back('0'+i);

    if(vis.find(s)==vis.end()){
        vis.insert(s);
        return false;
    }

    return true;
}


void solve (vector<int>& nums, int idx, vector<vector<int>>& res, vector<int> v, unordered_set<string> & vis){
    if(idx==nums.size()){
        if(!isExist(v, vis))
            res.push_back(v);

        return;
    }

    /* option 1 : without including idx */

    solve(nums, idx+1, res, v, vis);

    /* option 2 : with including idx */

    v.push_back(nums[idx]);
    solve(nums, idx+1, res, v, vis);
}


vector<vector<int>> subsetsWithDup(vector<int>& nums) {
    vector<vector<int>> res;
    vector<int> v;
    unordered_set<string> vis;

    sort(nums.begin(), nums.end());   // --> for avoiding duplicates

    solve (nums, 0, res, v, vis);
    return res;
}
```


#### 6. Letter Tile Possibilities
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

#### 7. Generate Parentheses
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

#### 8. Combinations
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


#### 9. Combination Sum (Same Number can be choosen unlimited times)
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

#### 10. Combination Sum II (Each number should be only used once + Does not conatain duplicate combinations)
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

#### 11. Combination Sum III
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

#### 12. Letter Combinations of a Phone Number
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

#### 13. Palindrome Partitioning
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

#### 14. Decode String
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

#### 15. Partition Equal Subset Sum
Given an array arr[] of size N, check if it can be partitioned into two parts such that the sum of elements in both parts is the same.

```cpp
int solve (int arr[], int N, int idx, int target, int sum){
    if(idx==N) return 0;
    if(sum==target) return 1;

    for(int i=idx; i<N; i++){
        if(sum+arr[i]>target) continue;

        bool res = solve (arr, N, i+1, target, sum+arr[i]);
        if(res) return 1;
    }

    return 0;
}


int equalPartition(int N, int arr[]){
    int sum = 0;
    for(int i=0; i<N; i++)
        sum+=arr[i];

    if(sum%2!=0)
        return 0;

    int target = sum/2;

    return solve (arr, N, 0, target, 0);    
}
```

#### 16. Tug of War
Given a set of n integers, divide the set in two subsets of n/2 sizes each such that the difference of the sum of two subsets is as minimum as possible. If n is even, then sizes of two subsets must be strictly n/2 and if n is odd, then size of one subset must be (n-1)/2 and size of other subset must be (n+1)/2. Return such possible min difference.

```cpp
void solve (vector<int>& v, int idx, int curr_sum, int total_sum, int & min_diff, int skip, int cnt){
    if(cnt==v.size()/2){
        int diff = abs((total_sum-curr_sum) - curr_sum);
        min_diff = min(min_diff, diff);
        return;
    }

    if(skip>0)
        solve(v, idx+1, curr_sum, total_sum, min_diff, skip-1, cnt);

    solve(v, idx+1, curr_sum+v[idx], total_sum, min_diff, skip, cnt+1);
}

int tug_of_war (vector<int> & v){
    int n = v.size();
    int min_diff = INT_MAX;
    int skip = n-(n/2);
    int total_sum = 0;

    for(int i : v)
        total_sum += i;

    solve(v, 0, 0, total_sum, min_diff, skip, 0);
    return min_diff;
}
```

#### 17. Partition to K Equal Sum Subsets (IMP)
Given an integer array nums and an integer k, return true if it is possible to divide this array into k non-empty subsets whose sums are all equal.

Input: nums = [4,3,2,3,5,2,1], k = 4

Output: true

```cpp
bool solve (vector<int> & nums, int target, int idx, int curr_sum, vector<bool> & vis, int k){

    /* If we found k-1 subsets of sum==target then return true */

    if(k==1) return true;    

    /*  Else if k>1 and curr_sum=target then backtrack to find other subsets */

    if(curr_sum==target) 
        return solve (nums, target, 0, 0, vis, k-1);

    /* Else continue to find current subset of sum==target in range idx to end */

    for(int i=idx; i<nums.size(); i++){

        /* If curr element is already vis or sum will exceed target, then ignore this element */

        if(vis[i] or curr_sum + nums[i] > target) continue;

        /* Else mark this element vis and backtrack */

        vis[i] = true;

        bool found_target = solve (nums, target, i+1, curr_sum + nums[i], vis, k);
        if(found_target) return true;

        /* If res is false then try for other elements and also unmark curr element to unvisited*/

        vis[i] = false;
    }

    return false;
}


bool canPartitionKSubsets(vector<int>& nums, int k) {
    int sum = 0;
    for(int n : nums)
        sum += n;

    if(sum%k!=0) return false;

    int target = sum/k;
    vector<bool> vis (nums.size(), false);

    return solve (nums, target, 0, 0, vis, k);
}
```

#### 18. Matchsticks to Square
You are given an integer array matchsticks where matchsticks[i] is the length of the ith matchstick. You want to use all the matchsticks to make one square. You should not break any stick, but you can link them up, and each matchstick must be used exactly one time. Return true if you can make this square and false otherwise.

![img](https://assets.leetcode.com/uploads/2021/04/09/matchsticks1-grid.jpg)

Input: matchsticks = [1,1,2,2,2]

Output: true


```cpp
/* Similar to partition array into k equal sum subsets (here k=4) */

bool solve (vector<int> & matchsticks, int idx, int target, int curr_sum, int k, vector<bool> & vis){
    if(k==1) return true;

    if(curr_sum == target)
        return solve (matchsticks, 0, target, 0, k-1, vis);

    for(int i=idx; i<matchsticks.size(); i++){

        if(vis[i] or curr_sum + matchsticks[i] > target) continue;

        vis[i] = true;

        bool sqr_formed = solve (matchsticks, i+1, target, curr_sum + matchsticks[i], k, vis);
        if(sqr_formed) return true;

        vis[i] = false;
    }

    return false;
}


bool makesquare(vector<int>& matchsticks) {
    int sum = 0;
    for(int m : matchsticks)
        sum += m;

    if(sum%4!=0) return false;

    sort (matchsticks.begin(), matchsticks.end(), greater<int>());   // --> for making our solution faster

    int target = sum/4;
    vector<bool> vis (matchsticks.size(), false);

    return solve (matchsticks, 0, target, 0, 4, vis);
}
```

#### 19. Increasing Subsequences
Given an integer array nums, return all the different possible increasing subsequences of the given array with at least two elements. You may return the answer in any order. The given array may contain duplicates, and two equal integers should also be considered a special case of increasing sequence.

Input: nums = [4,6,7,7,1]

Output: [[4,6],[4,6,7],[4,6,7,7],[4,7],[4,7,7],[6,7],[6,7,7],[7,7]]

```cpp
bool exist(string & s, unordered_set<string> & vis){
    if(vis.find(s)==vis.end()){
        vis.insert(s);
        return false;
    }

    return true;
}

void solve (vector<int>& nums, int idx, vector<vector<int>>& res, vector<int>& v, unordered_set<string> & vis, string & s){
    if(v.size()>=2 and !exist(s, vis))
        res.push_back(v);

    for(int i=idx; i<nums.size(); i++){

        if(v.empty() or v.back() <= nums[i]){
            s.push_back('0'+nums[i]);
            v.push_back(nums[i]);
            solve (nums, i+1, res, v, vis, s);
            v.pop_back();
            s.pop_back();
        }
    }
}

vector<vector<int>> findSubsequences(vector<int>& nums) {
    vector<vector<int>> res;
    vector<int> v;
    unordered_set<string> vis;     // --> used for removing duplicates
    string s = "";

    solve (nums, 0, res, v, vis, s);
    return res;
}
```

#### 20. K-th Symbol in Grammar
We build a table of n rows (1-indexed). We start by writing 0 in the 1st row. Now in every subsequent row, we look at the previous row and replace each occurrence of 0 with 01, and each occurrence of 1 with 10. Given two integer n and k, return the kth (1-indexed) symbol in the nth row of a table of n rows.

```cpp
int solve (int n, int k){
    if(n==1) return 0;

    int len = pow(2, n-1);      // --> total bits in nth row of grammar

    if(k<len/2)
        return solve (n-1, k);
    else
        return !solve (n, k-len/2);
}


int kthGrammar(int n, int k) {
    return solve (n, k-1);     // --> Since question follows 1 based indexing
}
```

#### 21. Restore IP Addresses
Given a string s containing only digits, return all possible valid IP addresses that can be obtained from s. You can return them in any order. A valid IP address consists of exactly four integers, each integer is between 0 and 255, separated by single dots and cannot have leading zeros.

Input: s = "010010"

Output: ["0.10.0.10","0.100.1.0"]

```cpp
void solve (string s, int idx, vector<string> & res, string ip, int dots){
    if(dots<0) return;

    string part = "";

    for(int i=idx; i<s.length(); i++){
        part.push_back(s[i]);

        /* If only 0 appears then only possible way is to append . after it and process further string */

        if(part=="0"){

            /* If all three dots are used and we are on last index then append this ip and return */

            if(dots==0 and i==s.length()-1)
                res.push_back(ip+part);

            /* Else recurse */

            else
                solve (s, i+1, res, ip+part+".", dots-1);

            return;
        }

        /* Else if part<=255 then we will try all cases using backtracking */

        else if(stoi(part)<=255){

            /* If all three dots are used and we are on last index then append this ip and return */

            if(dots==0 and i==s.length()-1){
                res.push_back(ip+part);
                return;
            }

            solve (s, i+1, res, ip+part+".", dots-1);
        }

        else return;
    }
}    


vector<string> restoreIpAddresses(string s) {
    vector<string> res;
    if(s.length()>12) return res;

    solve (s, 0, res, "", 3);

    return res;
}
```

#### 22. Permutation Sequence
The set [1, 2, 3, ..., n] contains a total of n! unique permutations. Given n and k, return the kth permutation sequence.

```cpp
void get_Kth_permutation(int n, int k, string & s, vector<int> & v, vector<int> & fact){
    if(n==0) return;

    int block_size = fact[n-1];
    int idx = k/block_size;

    s.push_back('0'+v[idx]);
    v.erase(v.begin()+idx);

    k = k - idx*block_size;

    get_Kth_permutation(n-1, k, s, v, fact);
}


string getPermutation(int n, int k) {
    string ans = "";  

    vector<int> v;
    for(int i=1; i<=n; i++)
        v.push_back(i);

    vector<int> fact;
    fact.push_back(1);

    for(int i=1; i<=n; i++)
        fact.push_back(fact.back()*i);

    get_Kth_permutation (n, k-1, ans, v, fact);

    return ans;
}
```

#### 23. Largest number in K swaps
Given a number K and string str of digits denoting a positive integer, build the largest number possible by performing swap operations on the digits of str at most K times.

Input: K = 3, str = "3435335"

Output: 5543333

```cpp
/* get max element after starting index */

int get_max(string & s, int start, int mx){
    for(int i=start; i<s.length(); i++)
        mx = max(mx, s[i]-'0');
    return mx;
}


void solve (string s, int idx, int k, string & ans){
    if(idx==s.size() or k==0){
        if(s>ans) ans = s;
        return;
    }

    /*  find max element after idx */

    int mx = get_max(s, idx+1, s[idx]-'0');

    /* If there is no greater element then curr element then don't swap and recurse */

    if(mx==s[idx]-'0')
        solve(s, idx+1, k, ans);

    /* else one by one try to swap all max element indexes and recurse */

    else{
        for(int i=idx+1; i<s.size(); i++){
            if(s[i]-'0' != mx) continue;

            swap(s[i], s[idx]);
            solve(s, idx+1, k-1, ans);
            swap(s[i], s[idx]);     //--> swapped again to recover the string to original form
        }
    }
}


string findMaximumNum(string str, int k){
    string ans = str;
    solve (str, 0, k, ans);
    return ans;
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

#### 2. Rat in a Maze Problem - I 
Consider a rat placed at (0, 0). It has to reach the destination at (N - 1, N - 1). Find all possible paths that the rat can take to reach from source to destination. Value 0 at a cell in the matrix represents that it is blocked and rat cannot move to it while value 1 at a cell in the matrix represents that rat can be travel through it.

```cpp
bool isInside(int x, int y, int n){
    return x>=0 and y>=0 and x<n and y<n;
}

void dfs (vector<vector<int>> & grid, int x, int y, int n, vector<vector<bool>> & curr_path, vector<string> & res, string & s){
    if(x==n-1 and y==n-1){
        res.push_back(s);
        return;
    }

    curr_path[x][y] = true;

    int dx[] = {1, 0, 0, -1};
    int dy[] = {0, -1, 1, 0};

    vector<char> direction = {'D', 'L', 'R', 'U'};

    for(int i=0; i<4; i++){
        int xnew = x + dx[i];
        int ynew = y + dy[i];

        if(isInside(xnew, ynew, n) and grid[xnew][ynew]==1 and !curr_path[xnew][ynew]){
            s.push_back(direction[i]);
            dfs (grid, xnew, ynew, n, curr_path, res, s);
            s.pop_back();
        }    
    }

    curr_path[x][y] = false;
}


vector<string> findPath(vector<vector<int>> &m, int n) {
    vector<vector<bool>> curr_path (n, vector<bool> (n, false));
    vector<string> res;
    string s = "";

    if(m[0][0]==0) return res;    // --> Boundary case

    dfs (m, 0, 0, n, curr_path, res, s);
    return res;
}
```

#### 3. Unique Paths III
On a 2-dimensional grid, there are 4 types of squares:
1. 1 represents the starting square.  There is exactly one starting square.
2. 2 represents the ending square.  There is exactly one ending square.
3. 0 represents empty squares we can walk over.
4. -1 represents obstacles that we cannot walk over.

Return the number of 4-directional walks from the starting square to the ending square, that walk over every non-obstacle square exactly once.

```cpp
bool isInside (int x, int y, int row, int col){
    return x>=0 and y>=0 and x<row and y<col;
}


void dfs (vector<vector<int>> & grid, vector<vector<bool>> & curr_path, int x, int y, int & path_cnt, int cnt, int row, int col){
    if(grid[x][y]==2){
        if(cnt==-1) path_cnt++;
        return;
    }

    curr_path[x][y] = true;

    int dx[] = {1, 0, -1, 0};
    int dy[] = {0, 1, 0, -1};

    for(int i=0; i<4; i++){
        int xnew = x + dx[i];
        int ynew = y + dy[i];

        if(isInside(xnew, ynew, row, col) and grid[xnew][ynew]!=-1 and !curr_path[xnew][ynew])
            dfs(grid, curr_path, xnew, ynew, path_cnt, cnt-1, row, col);
    }

    curr_path[x][y] = false;
    return;
}


int uniquePathsIII(vector<vector<int>>& grid) {
    int row = grid.size(), col = grid[0].size();
    int src_x, src_y;
    int cnt = 0;
    int path_cnt = 0;

    vector<vector<bool>> curr_path (row, vector<bool> (col, false));

    for(int i=0; i<row; i++){
        for(int j=0; j<col; j++){
            if(grid[i][j]==0)
                cnt++;
            else if(grid[i][j]==1){
                src_x = i; src_y = j;
            }
        }
    }

    dfs (grid, curr_path, src_x, src_y, path_cnt, cnt, row, col);
    return path_cnt;
}
```

#### 4. N-Queens
Given an integer n, return the number of distinct solutions to the n-queens puzzle.

![img](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

Input: n = 4

Output: 2

```cpp
bool canPlace (vector<vector<bool>>& board, int n, int x, int y){

    /* checking same column */

    for(int i=0; i<x; i++)
        if(board[i][y]) return false;

    /* checking upper left diagonal */

    int i=x-1, j=y-1;
    while(i>=0 and j>=0){
        if(board[i][j]) return false;
        i--; j--;
    }

    /* checking upper right diagonal */

    i=x-1; j=y+1;
    while(i>=0 and j<n){
        if(board[i][j]) return false;
        i--; j++;
    }

    return true;
}


void backtrack (vector<vector<bool>> & board, int row, int n, int & sol){
    if(row==n){
        sol++;
        return;
    }

    for(int i=0; i<n; i++){
        if(canPlace(board, n, row, i)){
            board[row][i] = true;
            backtrack(board, row+1, n, sol);
            board[row][i] = false;
        }
    }
}


int totalNQueens(int n) {
    vector<vector<bool>> board (n, vector<bool> (n, false));
    int sol = 0;

    backtrack (board, 0, n, sol);
    return sol;
}
```

#### 5. N-Queens II
Given an integer n, return all distinct solutions to the n-queens puzzle. You may return the answer in any order.

Input: n = 4

Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]

```cpp
bool canPlace (vector<string>& board, int n, int x, int y){

    /* checking same column */

    for(int i=0; i<x; i++)
        if(board[i][y]=='Q') return false;

    /* checking upper left diagonal */

    int i=x-1, j=y-1;
    while(i>=0 and j>=0){
        if(board[i][j]=='Q') return false;
        i--; j--;
    }

    /* checking upper right diagonal */

    i=x-1; j=y+1;
    while(i>=0 and j<n){
        if(board[i][j]=='Q') return false;
        i--; j++;
    }

    return true;
}


void backtrack (vector<string> & board, int row, int n, vector<vector<string>> & res){
    if(row==n){
        res.push_back(board);
        return;
    }

    for(int i=0; i<n; i++){
        if(canPlace(board, n, row, i)){
            board[row][i] = 'Q';
            backtrack(board, row+1, n, res);
            board[row][i] = '.';
        }
    }
}


vector<vector<string>> solveNQueens(int n) {
    vector<string> board;
    vector<vector<string>> res;

    for(int i=0; i<n; i++){
        string s = "";
        for(int j=0; j<n; j++){
            s.push_back('.');
        }
        board.push_back(s);
    }

    backtrack (board, 0, n, res);
    return res;
}
```

#### 6. Sudoku Solver
Write a program to solve a Sudoku puzzle by filling the empty cells.

```cpp
bool canPlace (vector<vector<char>>& board, int x, int y, char ch){

    /* checking in same row */

    for(int i=0; i<9; i++)
        if(board[x][i]==ch) return false;

    /* checking same column */

    for(int i=0; i<9; i++)
        if(board[i][y]==ch) return false;

    /* checking in sub-box */

    int start_x = x - (x%3);
    int start_y = y - (y%3);

    for(int i=start_x; i<start_x+3; i++){
        for(int j=start_y; j<start_y+3; j++){
            if(board[i][j]==ch) return false;
        }
    }

    return true;
}


bool backtrack (vector<vector<char>>& board, int x, int y){
    if(x==9) return true;

    if(board[x][y]!='.'){
        if(y==8) return backtrack (board, x+1, 0);
        else return backtrack (board, x, y+1);
    }

    for(int choice=1; choice<=9; choice++){
        char ch = '0'+choice;

        if (canPlace(board, x, y, ch)){
            board[x][y] = ch;
            bool res = false;

            if(y==8) res = backtrack (board, x+1, 0);
            else res = backtrack (board, x, y+1);

            if(res) return true;
        }
    }

    board[x][y] = '.';
    return false;
}


void solveSudoku(vector<vector<char>>& board) {
    bool res = backtrack (board, 0, 0);
}
```
