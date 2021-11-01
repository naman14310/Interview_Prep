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
