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

<br>

### Minimum Changes To Make Alternating Binary String
You are given a string s consisting only of the characters '0' and '1'. In one operation, you can change any '0' to '1' or vice versa. Return the minimum number of operations needed to make s alternating.

```cpp
int minOperations(string s) {
    int cnt1=0, cnt2=0;

    /* Case1 : Start with 0 */

    bool zero = true;

    for(char ch : s){
        if(zero and ch=='1') cnt1++;
        else if(!zero and ch=='0') cnt1++;

        zero = !zero;
    }


    /* Case2 : Start with 1 */

    zero = false;

    for(char ch : s){
        if(zero and ch=='1') cnt2++;
        else if(!zero and ch=='0') cnt2++;

        zero = !zero;
    }

    return min(cnt1, cnt2);
}
```

<br>

### [Minimum Deletion Cost to Avoid Repeating Letters](https://leetcode.com/problems/minimum-deletion-cost-to-avoid-repeating-letters/)
Given a string s and an array of integers cost where cost[i] is the cost of deleting the ith character in s. Return the minimum cost of deletions such that there are no two identical letters next to each other.

Input: s = "aabaa", cost = [1,2,3,4,1]

Output: 2

```cpp
int minCost(string s, vector<int>& cost) {
    int ans = 0;

    char prev = ' ';
    int tempCost = 0;
    int mx = 0;

    for(int i=0; i<s.length(); i++){

        if(s[i]!=prev){
            ans += tempCost - mx;

            tempCost = cost[i];
            mx = cost[i];
            prev = s[i];
        }
        else{
            mx = max(mx, cost[i]);
            tempCost += cost[i];
        }

    }

    ans += tempCost - mx;
    return ans;
}
```

<br>

### Books in Boxes (Variation of House Robber)
Given N boxes containing different number of Books in each box(numBook[i]),take a minimum number of books from the boxes conditions are such that:
1. you must take either all or none of the books inside a given box.
2. you cannot skip taking books from boxes adjacent to each other. Box1 and 2 can not be skipped but you can skip box 1 and 3.
3. you must have a minimum number of books in your hand
for example ,if there are 6 boxes and the number of books in box are {7,2,13,12,9,1} then the minimum number of books u can take is 15(by skipping box 1,3,5).

```cpp
int solve (vector<int> &v){
    int n = v.size();
    if(n==1) return 0;
    if(n==2) return min(v[0], v[1]);

    vector<int> dp (n, 0);
    dp[0] = 0;
    dp[1] = min(v[0], v[1]);

    for(int i=2; i<n; i++){
        dp[i] = min(dp[i-1]+v[i], v[i-1]+dp[i-2]);
    }

    return dp.back();
}
```

<br>

### Number of ways to score runs
A player is only allowed to score 1, 2, 4 and 6 runs in a ball. How many ways the player can score N runs without hitting consecutive 4s?

```
for ex, to score 4 runs, the player can hit the runs in subsequent balls in the following ways:

1,1,1,1
1,1,2
1,2,1
2,1,1
2,2
4

Output=6
```

```cpp
int solve (vector<int> &v, int sum, int prev){
    if(sum==0) return 1;
    int ways = 0;
    
    for(int i=0; i<v.size(); i++){
        if(v[i]>sum) break;
        if(v[i]==4 and prev==4) continue;

        ways += solve(v, sum-v[i], v[i]);
    }
    
    return ways;
}


int main(){
    vector<int> v = {1, 2, 4, 6};
    int sum = 7;

    cout<<solve(v, sum, 0)<<endl;
    return 0;
}
```

<br>

### [Maximal Network Rank](https://leetcode.com/problems/maximal-network-rank/)

```cpp
int maximalNetworkRank(int n, vector<vector<int>>& roads) {
    vector<int> degre (n, 0);
    int ans = 0;

    unordered_set<string> vis;

    for(auto r : roads){
        degre[r[0]]++;
        degre[r[1]]++;

        vis.insert(to_string(r[0]) + " " + to_string(r[1]));
        vis.insert(to_string(r[1]) + " " + to_string(r[0]));
    }


    for(int i=0; i<n; i++){
        for(int j=i+1; j<n; j++){

            string key = to_string(i) + " " + to_string(j);

            int temp = degre[i] + degre[j];
            if(vis.find(key)!=vis.end()) temp--;

            ans = max(ans, temp);
        }    
    }

    return ans;
}
```

<br>

### [Maximum Length of a Concatenated String with Unique Characters](https://leetcode.com/problems/maximum-length-of-a-concatenated-string-with-unique-characters/)

```cpp
bool isValid (vector<bool> &v1, vector<bool> &v2){    
    for(int i=0; i<26; i++)
        if(v1[i] and v2[i]) return false;

    return true;
}


void combine(vector<bool> &v1, vector<bool>&v2){
    for(int i=0; i<26; i++)
        if(v2[i]) v1[i] = true;    
}


void detach(vector<bool> &v1, vector<bool>&v2){
    for(int i=0; i<26; i++)
        if(v2[i]) v1[i] = false;
}


int solve (vector<vector<bool>> &bitvector, int idx, vector<bool> &v, vector<int> &wordLen){

    if(idx==bitvector.size()) return 0;

    if(isValid(v, bitvector[idx])){

        combine(v, bitvector[idx]);
        int len1 = solve(bitvector, idx+1, v, wordLen) + wordLen[idx];

        detach(v, bitvector[idx]);
        int len2 = solve(bitvector, idx+1, v, wordLen);

        return max(len1, len2);
    }

    else{
        int len2 = solve(bitvector, idx+1, v, wordLen);
        return len2;
    }

}



int maxLength(vector<string>& arr) {
    vector<vector<bool>> bitvector;
    vector<int> wordLen;

    for(string s : arr){
        vector<bool> v (26, false);
        bool flag = false;

        for(char ch : s){
            if(v[ch-'a']){
                flag = true;
                break;
            }

            v[ch-'a'] = true;
        }

        if(!flag){
            bitvector.push_back(v);
            wordLen.push_back(s.length());
        } 
    }

    vector<bool> v (26, false);
    return solve (bitvector, 0, v, wordLen);
}
```

<br>

### [Crop Characters](https://imgur.com/a/e9PZUXR)

```cpp
string solve(string s, int k){
    if(k>=s.length()) return s;

    if(s[k]==' ')
        return s.substr(0, k);
    
    string res = s.substr(0, k);
    
    while(res.back()!=' ') res.pop_back();

    res.pop_back();
    return res;
}
```

<br>

### [String Comparision with one operation](https://imgur.com/a/e9PZUXR)

```cpp
string solve (string &s, string &t){
    int lens = s.length(), lent = t.length();
    string res = "";

    if(s==t) return "EQUAL";

    if(s==t.substr(1,lent-1)){
        res += "INSERT ";
        res.push_back(t[0]);
        return res;
    } 

    if(s.substr(0, lens-1)==t){
        res += "REMOVE ";
        res.push_back(s[lens-1]);
        return res;
    }

    for(int i=0; i<lens-1; i++){

        swap(s[i], s[i+1]);

        if(s==t){
            res += "SWAP ";
            res.push_back(s[i+1]);
            res += " ";
            res.push_back(s[i]);
            return res;
        }

        swap(s[i], s[i+1]);
    }

    return "IMPOSSIBLE";

} 
```

<br>

### [Max Chunks To Make Sorted](https://leetcode.com/problems/max-chunks-to-make-sorted/)

```cpp
int maxChunksToSorted(vector<int>& arr) {

    int range = 0;
    int chunk = 0;

    for(int i=0; i<arr.size(); i++){
        range = max(range, arr[i]);

        if(i==range) chunk++;
    }

    return chunk;
}
```

<br>

### Subarray with given Mean
You are given an array A of N integers and an integer S. Your task is to compute how many ways one can choose a contiguous fragment of A such that arithmetic mean is equal to S.

Approach: 
1. Decrease all the elements of the array by S
2. Now the problem reduces to finding a subarray whose sum = 0. This can be easily solved using prefix sum and a map.

```cpp
long long solve (vector<int> &v, int mean){

    for(int i=0; i<v.size(); i++)
        v[i]-=mean;
    
    /* Now question reduced to count number of subarrays having sum = 0 */

    unordered_map<int, int> mp;
    mp[0] = 1;

    long long ans = 0;
    long long csum = 0;


    for(int num : v){
        csum += num;

        if(mp.find(csum)!=mp.end())
            ans += mp[csum];
        
        mp[csum]++;
    }

    return ans;
}
```

<br>

### Missing Rolls on Dice

We rolled a dice several times. We remembered N rolls which are described by array A. We forgot F rolls. The arithmatic mean of all rolls is M. What are the possible results of missing rolls.

Input: A=[3, 2, 4, 3], F=2, M=4

Output: [6, 6]

```cpp
void generateCombs (int start, int idx, int target, vector<vector<int>> &res, vector<int> &v, int f){
    if(target<0) return;

    if(idx==f){
        if(target==0) res.push_back(v);
        return;
    }

    for(int i=start; i<=6; i++){
        v.push_back(i);
        generateCombs (i, idx+1, target-i, res, v, f);
        v.pop_back();
    }
}

vector<vector<int>> solve (vector<int> arr, int f, int m){

    int arrSum = 0;
    for(int num : arr) arrSum += num;

    int totalSum = (arr.size() + f) * m;

    int target = totalSum - arrSum;

    vector<vector<int>> res;
    vector<int> v;

    generateCombs(1, 0, target, res, v, f);
    return res;
}
```

<br>

### Bomb in a Grid
Given a N * N matrix. Vector r and c represent respective row and col of bombs. Print the matrix with 'B' in place of bombs and count of nbr bombs in other cells (consider 8 nbrs)

```cpp
bool isInside(int x, int y, int row, int col){
    return x>=0 and y>=0 and x<row and y<col;
}


vector<vector<char>> solve (int n, vector<int> &r, vector<int> &c){

    vector<vector<char>> grid (n, vector<char> (n, '0'));

    for(int i=0; i<r.size(); i++){
        int x = r[i], y = c[i];
        grid[x][y]='B';
        
        int dx[] = {1, 0, -1, 0, 1, 1, -1, -1};
        int dy[] = {0, 1, 0, -1, 1, -1, 1, -1};

        for(int i=0; i<8; i++){
            int xnew = x + dx[i];
            int ynew = y + dy[i];

            if(isInside(xnew, ynew, n, n) and grid[xnew][ynew]!='B'){
                grid[xnew][ynew] += 1;
            }
        }

    }
    
    return grid;
}
```

<br>

### [Min Buckets to Fill Water](https://www.geeksforgeeks.org/microsoft-interview-experience-3/)
Given buckets of size 1..N, find the minimum number of buckets required to fill K liters of water.

```cpp
/* assuming we can take buckets multiple times */

int solve (int idx, int n, int k, vector<vector<int>> &dp){

    if(k==0) return 0;
    if(idx>n) return -1;

    if(dp[idx][k]!=INT_MIN) return dp[idx][k];

    if(idx<=k){

        int taken = solve(idx, n, k-idx, dp);
        int not_taken = solve(idx+1, n, k, dp);

        if(taken==-1 and not_taken==-1) return dp[idx][k] = -1;

        if(taken==-1) return dp[idx][k] = not_taken;

        if(not_taken==-1) return dp[idx][k] = taken+1;

        return dp[idx][k] = min(taken+1, not_taken);
    }

    else{
        int not_taken = solve(idx+1, n, k, dp);
        return dp[idx][k] = not_taken;
    }

}


int getMinBuckets(int n, int k){
    vector<vector<int>> dp (n+1, vector<int> (k+1, INT_MIN));
    return solve (1, n, k, dp);
}
```

<br>

### Min Removes to make palindrome

Input: s = "ervervige"

Output: 2

```cpp
int solve (string &s){
    unordered_map<char, int> mp;

    for(char ch : s){
        mp[ch]++;
    }

    int cnt = 0;

    for(auto p : mp){
        if(p.second&1) cnt++;
    }

    return cnt>0?cnt-1:0;
}
```

<br>

### Find Path without getting detected

```cpp
bool isInside (int x, int y, int row, int col){
    return x>=0 and y>=0 and x<row and y<col;
}


bool dfs (vector<string> & grid, int x, int y, int row, int col){
    if(x==row-1 and y==col-1) return true;

    int dx[] = {1, 0, -1, 0};
    int dy[] = {0, 1 ,0, -1};

    grid[x][y] = '-';

    for(int i=0; i<4; i++){
        int xnew = x + dx[i];
        int ynew = y + dy[i];

        if(isInside(xnew, ynew, row, col) and grid[xnew][ynew]=='.'){
            bool res = dfs (grid, xnew, ynew, row, col);
            if(res) return true;
        }

    }

    return false;
}


void move(int &x, int &y, char dir){
    if(dir=='>') y++;
    else if(dir=='<') y--;
    else if(dir=='^') x--;
    else x++;
}


bool fillVision(vector<string> & grid, int x, int y, char dir, int row, int col){
    move(x, y, dir);

    while(isInside(x, y, row, col)){
        if(grid[x][y]=='A') return false;
        if(grid[x][y]!='.' and grid[x][y]!='*') return true;

        grid[x][y] = '*';
        move(x, y, dir);
    }

    return true;
}


bool solve (vector<string> &grid){
    int row = grid.size(), col = grid[0].size();

    int src_x = 0, src_y = 0;

    for(int i=0; i<row; i++){
        for(int j=0; j<col; j++){
            
            if(grid[i][j]=='>' or grid[i][j]=='<' or grid[i][j]=='^' or grid[i][j]=='v'){
                bool fv = fillVision(grid, i, j, grid[i][j], row, col);
                if(!fv) return false;
            }

            if(grid[i][j]=='A'){
                src_x = i;
                src_y = j;
            }
        }
    }

    bool isPathExist = dfs (grid, src_x, src_y, row, col);
    return isPathExist;
}
```

<br>

### [Distinct Books in Shelf](https://leetcode.com/discuss/interview-question/1521473/microsoft-india-fte-engage-codility-oa-october-2021)
A storeroom is used to organize items stored in it on N shelves. Shelves are numbered from 0 to N-1. The K-th shelf is dedicated items of only one type, denoted by a positive integer A[K]. We need to free R consecutive shelves. What is the maximum number of types of items which still can be stored in the storeroom after freeing R consecutive shelves?

```cpp
int findDistinctItems(vector<int> &v, int k){
    int n = v.size();
    int ans = 0; 
    unordered_map<int, int> mp;

    for(int i : v)
        mp[i]++;

    int cnt = mp.size();

    for(int i=0; i<min(k, n); i++){
        mp[v[i]]--;
        if(mp[v[i]] == 0) cnt--;
    }

    ans = max(ans, cnt);

    for(int i=0; i+k<n; i++){
        mp[v[i]]++;
        if(mp[v[i]]==1) cnt++;

        mp[v[i+k]]--;
        if(mp[v[i+k]]==0) cnt--;

        ans = max(ans, cnt);
    }

    return ans;
}
```

<br>

### Minimum swap to make string palindrome
Given a string s, the task is to find out the minimum no of adjacent swaps required to make string s palindrome. If not possible, return -1.

Input: aabcb

Output: 3 

```cpp
int minSwaps (string &s){
    int n = s.length();
    int i=0, j=n-1;
    int moves = 0;

    while(i<=n/2){

        if(s[i]==s[j]){
            i++; j--;
            continue;
        }

        int k=j-1;
        while(k>i and s[k]!=s[i]) k--;

        if(k==i) return -1;

        while(k<j){
            swap(s[k], s[k+1]);
            moves++;
            k++;
        }

        i++; j--;
    }

    return moves;
}
```

<br>

### [Min Steps to make Pile equal heights](https://leetcode.com/discuss/interview-question/364618/)

```cpp
int solve (vector<int> &piles){
    sort(piles.begin(), piles.end(), greater<int>());

    int prev_cnt = 0, cnt = 0;
    int ans = 0;

    for(int i=0; i<piles.size(); i++){
        if(i==0 or piles[i]!=piles[i-1]){
            ans += prev_cnt + cnt;
            prev_cnt += cnt;
            cnt=1;
        }
        else
            cnt++;
    }

    return ans;
}
```

<br>

### [Min Deletions To Obtain String in Right Format](https://algo.monster/problems/min_deletions_to_obtain_string_in_right_format)

```cpp

int solve (string s){
    int n = s.length();
    int cntx = 0;
    for(char ch : s) 
        if(ch=='X') cntx++;
    
    int ans = cntx;
    int clx = 0, cly = 0;

    for(int i=0; i<s.length(); i++){

        if(s[i]=='X') clx++;
        else cly++;
        
        ans = min(ans, cly+(cntx-clx));
    }

    return ans;
}
```

