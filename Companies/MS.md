# Microsoft

<br>

### [Position of robot at each movement](https://leetcode.com/discuss/interview-question/1539963/Microsoft-Codility-OA-Hyderabad-India)

```cpp
bool isValid(int x, int y, int row, int col){
    return x>=0 and y>=0 and x<row and y<col;
}


vector<vector<int>> solve (vector<vector<int>> &grid, string &s, int row, int col){
    int x = 0, y = 0;
    vector<vector<int>> res;

    for(char ch : s){

        if(ch=='U' and isValid(x-1, y, row, col) and grid[x-1][y]==0)
            x--;

        else if(ch=='D' and isValid(x+1, y, row, col) and grid[x+1][y]==0)
            x++;

        else if(ch=='L' and isValid(x, y-1, row, col) and grid[x][y-1]==0)
            y--;

        else if(ch=='R' and isValid(x, y+1, row, col) and grid[x][y+1]==0)
            y++;

        res.push_back({x, y});
    }

    return res;
}
```

<br>


### Maximum possible value by inserting '5'
```
examples:
input: 1234 -> output: 51234
input: 7643 -> output 76543
input: 0 -> output 50
input: -661 -> output -5661
```

```cpp
int solve (int num){
    string s = to_string(num);

    if(num<0){
        for(int i=1; i<s.length(); i++){
            if(s[i]-'0'>5){
                s.insert(i, "5");
                return stoi(s);
            }
        }
        s.push_back(5);
        return stoi(s);
    }
    else{
        for(int i=0; i<s.length(); i++){
            if(s[i]-'0'<5){
                s.insert(i, "5");
                return stoi(s);
            }
        }
        s.push_back(5);
        return stoi(s);
    }
}
```

<br>

### [Longest Nice Substring](https://leetcode.com/problems/longest-nice-substring/)

```cpp
/* Checking if the curr string between i and j is nice or not */

bool nice(vector<int> &v1, vector<int> &v2){
    for(int i=0; i<26; i++){
        if((v1[i]==0 and v2[i]>0) or (v1[i]>0 and v2[i]==0)) return false;
    }

    return true;
}


/* Checking for [1-26] unique chars one by one */

string solve (string s, int cnt){
    vector<int> v1 (26, 0);
    vector<int> v2 (26, 0);

    int i=0, j=0;
    string ans = "";

    while(j<s.length()){        
        char ch = s[j];

        if(islower(ch)){
            int idx = ch-'a';
            v1[idx]++;
            if(v1[idx]==1 and v2[idx]==0) cnt--;
        } 
        else{
            int idx = ch-'A';
            v2[idx]++;
            if(v2[idx]==1 and v1[idx]==0) cnt--;
        }


        while(cnt<0){
            char ch2 = s[i];

            if(islower(ch2)){
                int idx2 = ch2-'a';
                v1[idx2]--;
                if(v1[idx2]==0 and v2[idx2]==0) cnt++;
            } 
            else{
                int idx2 = ch2-'A';
                v2[idx2]--;
                if(v2[idx2]==0 and v1[idx2]==0) cnt++;
            }

            i++;
        }


        if(j-i+1>ans.length() and nice(v1, v2))
            ans = s.substr(i, j-i+1);

        j++;
    }

    return ans;
}


string longestNiceSubstring(string s) {
    string ans = "";

    for(int i=1; i<=26; i++){
        string temp = solve(s, i);
        if(temp.length()>ans.length())
            ans = temp;
    }

    return ans;
}
```

<br>

### [Counter Example](https://leetcode.com/discuss/interview-question/555702/Microsoft-or-Online-Codility-Assessment-or-Counterexample-(Task-1)/486118)

```
a = Array.new(n) {Rand(1..9)}
return a if find_min(a) != a.min
```

<br>

### [Minimum Deletions to Make Character Frequencies Unique](https://leetcode.com/problems/minimum-deletions-to-make-character-frequencies-unique/)

```cpp
int minDeletions(string s) {

    vector<int> freq (26, 0);
    for(char ch : s)
        freq[ch-'a']++;

    unordered_set<int> vis;
    int deletions = 0;

    for(int i=0; i<26; i++){
        while(freq[i]>0 and vis.find(freq[i])!=vis.end()){
            freq[i]--;
            deletions++;
        }

        if(freq[i]>0) vis.insert(freq[i]);
    }

    return deletions;
}
```

<br>

### Min Moves
Given an array like A = [1,1,3,4,4,4] find the minimum number of moves required to make like [1,4,4,4,4] -> the number and its frequency should match. Here answer is 3 swaps.
(delete one 1, delete 3, and add 4 by taking1s position).
Ex: A=[10,10,10], we need ten 10s to make count and frequency match. But we have in total 3 positions only. So delete all 10s and make it an empty list. So min moves is 3.

```cpp
int solve(vector<int> &v){
    unordered_map<int, int> mp;
    for(int num : v)
        mp[num]++;

    int minMoves = 0;

    for(auto p : mp){
        if(p.second>=p.first)
            minMoves += p.second-p.first;
        else
            minMoves += min(p.first-p.second, p.second);
    }

    return minMoves;
}
```

<br>

### Min Split
Find the min number of substring that can be made such that the substring has unique characters.
```
Sample Test Case:
world -> 1, as the string has no letters that occur more than once.
dddd -> 4, as you can only create substring of each character.
abba -> 2, as you can make substrings of ab, ba.
cycle-> 2, you can create substrings of (cy, cle) or (c, ycle)
```

```cpp
int solve (string &s){
    vector<bool> vis (26, false);
    int split = 0;

    for(char ch : s){
        if(vis[ch-'a']){
            vis = vector<bool> (26, false);
            split++;
        }

        vis[ch-'a'] = true;
    }

    return split+1;
}
```

<br>

### [Count Good Nodes](https://leetcode.com/problems/count-good-nodes-in-binary-tree/)

```cpp
void solve(TreeNode* root, vector<int> &v, int &cnt){
    if(!root) return;

    if(!root->left and !root->right){
        if(v.empty() or root->val >= v.back()) cnt++;
        return;
    } 

    if(v.empty() or root->val >= v.back()){
        cnt++;
        v.push_back(root->val);
    }

    solve(root->left, v, cnt);
    solve(root->right, v, cnt);

    if(root->val >= v.back())
        v.pop_back();
}


int goodNodes(TreeNode* root) {
    int cnt = 0;
    vector<int> v;

    solve(root, v, cnt);
    return cnt;
}
```

<br>

### Minimum change we CAN NOT give using coins
You are given an array of integer representing coins please find out the minimum change you CAN NOT give using these coins
```
Examples:
Input:[8,2,1,1,5,8,7,14]
Output:47

Input:[4,7]
Output:1

Input:[1,2,5]
Output:4
```

```cpp
int solve (vector<int> &coins){
    int sum = accumulate(coins.begin(), coins.end(), 0);
    int n = coins.size();

    vector<vector<bool>> matrix (n+1, vector<bool> (sum+1, false));

    for(int i=0; i<=n; i++){
        for(int j=0; j<=sum; j++){

            if(j==0) matrix[i][j] = true;

            else if(i==0) matrix[i][j] = false;

            else if(coins[i-1]>j)
                matrix[i][j] = matrix[i-1][j];

            else matrix[i][j] = matrix[i-1][j] or matrix[i-1][j-coins[i-1]];
        }
    }

    for(int j=1; j<=sum; j++)
        if(!matrix[n][j]) return j;
    
    return sum+1;
}
```
