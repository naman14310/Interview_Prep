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
