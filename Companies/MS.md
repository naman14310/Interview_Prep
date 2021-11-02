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
