# DFS 

<br>

## @ DFS on Adjacency List

### 1. All Paths From Source to Target
Given a directed acyclic graph (DAG) of n nodes labeled from 0 to n - 1, find all possible paths from node 0 to node n - 1, and return them in any order.

```cpp
void dfs(vector<vector<int>>& graph, vector<bool> & vis, vector<vector<int>> & res, vector<int> & path, int n, int src){
    path.push_back(src);

    if(src==n-1){
        res.push_back(path);
        path.pop_back();
        return;
    }

    vis[src] = true;

    for(int nbr : graph[src])
        dfs(graph, vis, res, path, n, nbr);

    vis[src] = false;
    path.pop_back();
}

vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
    int n = graph.size(); 
    vector<vector<int>> res; 

    vector<bool> vis (n, false);
    vector<int> path;

    dfs(graph, vis, res, path, n, 0);
    return res;
}
```

<br>

### 2. Find Eventual Safe States
A node is evetually safe if all path from the node ends at a terminal node. Return an array containing all the safe nodes of the graph.

![img](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/03/17/picture1.png)

Input: graph = [[1,2],[2,3],[5],[0],[5],[],[]]

Output: [2,4,5,6]

Hint: Find all nodes which is not involved in any cycle gambling

```cpp
bool dfs (vector<vector<int>> & graph, vector<bool> & curr_path, vector<bool> & vis, int src, unordered_set<int> & safe){
    vis[src] = true;
    curr_path[src] = true;

    bool unsafe = false;

    for(int nbr : graph[src]){

        /* Case 1:  If nbr lies in current path --> that means cycle is formed and curr vertex is unsafe */

        if(curr_path[nbr])
            unsafe = true;

        /* 
            Case 2: If nbr was visited earlier,          
                    A) If it was unsafe --> current vertex is also unsafe
                    B) If it was safe ----> cannot comment on curr vertex so do nothing

        */

        else if(vis[nbr]){
            if(safe.find(nbr)==safe.end())
                unsafe = true;
        }

        /* Case 3: If nbr is undiscovered --> do DFS and update the information */

        else{
            bool res = dfs(graph, curr_path, vis, nbr, safe);
            unsafe = unsafe or res;
        }
    }

    curr_path[src] = false;

    if(!unsafe) 
        safe.insert(src);

    return unsafe;
}

vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
    int n = graph.size();
    vector<bool> vis (n, false);
    unordered_set<int> safe;                 // ----> It will store all safe vertices

    for(int i=0; i<n; i++){
        if(!vis[i]){
            vector<bool> curr_path (n, false);
            dfs(graph, curr_path, vis, i, safe);
        }
    }

    vector<int> safe_list;

    for(int i=0; i<n; i++)
        if(safe.find(i)!=safe.end())
            safe_list.push_back(i);

    return safe_list;
}
```

<br>

### 3. Course Schedule
There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai. Return true if you can finish all courses. Otherwise, return false.

Hint: Do simple cycle detection 

```cpp
bool dfs (unordered_map<int, vector<int>>& graph, vector<bool> & vis, vector<bool> & curr_path, int src){
    vis[src] = true;
    curr_path[src] = true;

    for(int nbr : graph[src]){

        if(curr_path[nbr]) return false;

        if(!vis[nbr]){
            bool res = dfs(graph, vis, curr_path, nbr);
            if(!res) return false;
        }
    }

    curr_path[src] = false;
    return true;
}

bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
    unordered_map<int, vector<int>> graph;
    vector<bool> vis (numCourses, false);
    vector<bool> curr_path(numCourses, false);

    for(auto e : prerequisites)
        graph[e[0]].push_back(e[1]);        

    for(int i=0; i<numCourses; i++){
        if(!vis[i]){   
            int res = dfs(graph, vis, curr_path, i);
            if(!res) return false;
        }
    }

    return true;
}
```


## @ DFS on Matrix

### 1. Max Area of Island

![img](https://assets.leetcode.com/uploads/2021/05/01/maxarea1-grid.jpg)

```cpp
bool isInside(int & x, int & y, int & row, int & col){
    return x>=0 and y>=0 and x<row and y<col;
}
    
int dfs(vector<vector<int>> & grid, int x, int y, int & row, int & col){
    int area = 1;
    grid[x][y] = 2;                // --> marked it as vis

    int dx[] = {1, 0, -1, 0};
    int dy[] = {0, 1, 0, -1};

    for(int i=0; i<4; i++){
        int xnew = x + dx[i];
        int ynew = y + dy[i];

        if(isInside(xnew, ynew, row, col) and grid[xnew][ynew]==1)
            area += dfs(grid, xnew, ynew, row, col);

    }

    return area;
}

int maxAreaOfIsland(vector<vector<int>>& grid) {
    int row = grid.size(), col = grid[0].size();
    int maxArea = 0;

    for(int i=0; i<row; i++){
        for(int j=0; j<col; j++){
            if(grid[i][j]==1){
                int area = dfs(grid, i, j, row, col);
                maxArea = max(area, maxArea);
            }
        }
    }
    return maxArea;
}
```

<br>

### 2. Number of Closed Islands
Given a 2D grid consists of 0s (land) and 1s (water).  An island is a maximal 4-directionally connected group of 0s and a closed island is an island totally (all left, top, right, bottom) surrounded by 1s. Return the number of closed islands.

![img](https://assets.leetcode.com/uploads/2019/10/31/sample_3_1610.png)

Hint: Pass one bool variable isClosed to dfs function, whenever you find any 0(land) on boundary, make isClosed false.

```cpp
bool isInside(int & x, int & y, int & row, int & col){
    return x>=0 and y>=0 and x<row and y<col;
}

void dfs(vector<vector<int>>& grid, int x, int y, int & row, int & col, bool & isClosed){
    grid[x][y] = 2;     

    if(x==0 or y==0 or x==row-1 or y==col-1)
        isClosed = false;

    int dx[] = {1, 0, -1, 0};
    int dy[] = {0, 1, 0, -1};

    for(int i=0; i<4; i++){
        int xnew = x + dx[i];
        int ynew = y + dy[i];

        if(isInside(xnew, ynew, row, col) and grid[xnew][ynew]==0)
            dfs(grid, xnew, ynew, row, col, isClosed);
    }
}


int closedIsland(vector<vector<int>>& grid) {
    int row = grid.size(), col = grid[0].size();
    int res = 0;

    for(int i=0; i<row; i++){
        for(int j=0; j<col; j++){
            if(grid[i][j]==0){
                bool isClosed = true;
                dfs(grid, i, j, row, col, isClosed);

                if(isClosed) res++;
            }
        }
    }
    return res;
}
```

<br>

### 3. Number of Islands

```cpp
bool isInside(int x, int y, int row, int col){
    return x>=0 and y>=0 and x<row and y<col;
}

void dfs(vector<vector<char>> & grid, int x, int y, int row, int col){
    grid[x][y] = '2';

    int dx[] = {1, 0, -1, 0};
    int dy[] = {0, 1, 0, -1};

    for(int i=0; i<4; i++){
        int xnew = x + dx[i];
        int ynew = y + dy[i];

        if(isInside(xnew, ynew, row, col) and grid[xnew][ynew]=='1')
            dfs(grid, xnew, ynew, row, col);
    }
}


int numIslands(vector<vector<char>>& grid) {
    int row = grid.size(), col = grid[0].size();
    int island = 0;

    for(int i=0; i<row; i++){
        for(int j=0; j<col; j++){
            if(grid[i][j]=='1'){
                dfs(grid, i, j, row, col);
                island++;
            }
        }
    }

    return island;
}
```

<br>

### 4. Surrounded Regions
Given an m x n matrix board containing 'X' and 'O', capture all regions surrounded by 'X'. A region is captured by flipping all 'O's into 'X's in that surrounded region.

![img](https://assets.leetcode.com/uploads/2021/02/19/xogrid.jpg)

```cpp
bool isInside(int x, int y, int row, int col){
    return x>=0 and y>=0 and x<row and y<col;
}

void dfs (vector<vector<char>>& board, int x, int y, int row, int col){
    board[x][y] = '.';

    int dx[] = {1, 0, -1, 0};
    int dy[] = {0, 1, 0, -1};

    for(int i=0; i<4; i++){
        int xnew = x + dx[i];
        int ynew = y + dy[i];

        if(isInside(xnew, ynew, row, col) and board[xnew][ynew]=='O')
            dfs(board, xnew, ynew, row, col);   
    }
}

void solve(vector<vector<char>>& board) {
    int row = board.size(), col = board[0].size();

    /* Iterate all four boundaries and convert all related O to . */

    for(int j=0; j<col; j++){
        if(board[0][j]=='O')
            dfs(board, 0, j, row, col);
    }

    for(int j=0; j<col; j++){
        if(board[row-1][j]=='O')
            dfs(board, row-1, j, row, col);
    }

    for(int i=1; i<row-1; i++){
        if(board[i][0]=='O')
            dfs(board, i, 0, row, col);
    }

    for(int i=1; i<row-1; i++){
        if(board[i][col-1]=='O')
            dfs(board, i, col-1, row, col);
    }

    /* Convert all remaining O to X and . to O again */

    for(int i=0; i<row; i++){
        for(int j=0; j<col; j++){
            if(board[i][j]=='O')
                board[i][j] = 'X';

            else if(board[i][j]=='.')
                board[i][j] = 'O';
        }
    }
}
```

<br>

### 5. Pacific Atlantic Water Flow
There is an m x n rectangular island that borders both the Pacific Ocean and Atlantic Ocean. The rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is less than or equal to the current cell's height. Return a 2D list of grid coordinates such that rain water can flow from cell (ri, ci) to both the Pacific and Atlantic oceans.

![img](https://assets.leetcode.com/uploads/2021/06/08/waterflow-grid.jpg)

Hint: Flow the water in reverse dir from ocean to land. First iterate the upper and left boundary to flow the pacific water. Then iterate right and bottom boundary to flow the atlantic water. If any cell contains both the water, add it in the answer vector.

```cpp
bool isInside(int x, int y, int row, int col){
    return x>=0 and y>=0 and x<row and y<col;
}

bool isVisited(vector<vector<pair<bool, bool>>>& water, bool pacific, int x, int y){
    if(pacific and water[x][y].first)
        return true;

    if(!pacific and water[x][y].second)
        return true;

    return false;
}

void dfs(vector<vector<int>>& heights, vector<vector<pair<bool, bool>>>& water, bool pacific, int x, int y, int row, int col, vector<vector<int>>& res){
    int dx[] = {1, 0, -1, 0};
    int dy[] = {0, 1, 0, -1};

    if(pacific) water[x][y].first = true;
    else{
        water[x][y].second = true;
        if(water[x][y].first){
            vector<int> temp;
            temp.push_back(x); temp.push_back(y);
            res.push_back(temp);
        }
    } 

    for(int i=0; i<4; i++){
        int xnew = x + dx[i];
        int ynew = y + dy[i];

        if(isInside(xnew, ynew, row, col) and !isVisited(water, pacific, xnew, ynew) and heights[xnew][ynew]>=heights[x][y])
            dfs(heights, water, pacific, xnew, ynew, row, col, res);
    }
}

vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
    int row = heights.size(), col = heights[0].size();
    vector<vector<int>> res;

    /* 
        water matrix will keep track of pacific and atlantic waters for every cell 
        and it will also act as a visited matrix
    */

    vector<vector<pair<bool, bool>>> water (row, vector<pair<bool, bool>> (col, {false, false}));

    /* Iterating upper boundary for pacific ocean */

    for(int j=0; j<col; j++){
        if(!water[0][j].first)
            dfs(heights, water, true, 0, j, row, col, res);
    }

    /* Iterating left boundary for pacific ocean */

    for(int i=0; i<row; i++){
        if(!water[i][0].first)
            dfs(heights, water, true, i, 0, row, col, res);
    }

    /* Iterating right boundary for atlantic ocean */

    for(int i=0; i<row; i++){
        if(!water[i][col-1].second)
            dfs(heights, water, false, i, col-1, row, col, res);
    }

    /* Iterating lower boundary for atlantic ocean */

    for(int j=0; j<col; j++){
        if(!water[row-1][j].second)
            dfs(heights, water, false, row-1, j, row, col, res);
    }

    return res;
}
```

<br>

### 6. Rat in a Maze Problem - I 
Consider a rat placed at (0, 0) in a square matrix of order N * N. It has to reach the destination at (N - 1, N - 1). Find all possible paths that the rat can take to reach from source to destination. The directions in which the rat can move are 'U'(up), 'D'(down), 'L' (left), 'R' (right). Value 0 at a cell in the matrix represents that it is blocked and rat cannot move to it while value 1 at a cell in the matrix represents that rat can be travel through it. In a path, no cell can be visited more than one time.

```
Input: N = 4
m[][] = {{1, 0, 0, 0},
         {1, 1, 0, 1}, 
         {1, 1, 0, 0},
         {0, 1, 1, 1}}
         
Output: DDRDRR DRDDRR
```

Hint: Use pass by reference

```cpp
bool isInside(int x, int y, int row, int col){
    return x>=0 and y>=0 and x<row and y<col;
}

void dfs (vector<vector<int>> & grid, vector<vector<bool>> & vis, int n, int x, int y, string & path, vector<string> & res){
    if(x==n-1 and y==n-1){
        res.push_back(path);
        return;
    }

    vis[x][y] = true;

    /* Following order gives sorted order */

    int dx[] = {1, 0, 0, -1};
    int dy[] = {0, -1, 1, 0};

    for(int i=0; i<4; i++){
        int xnew = x + dx[i];
        int ynew = y + dy[i];

        if(isInside(xnew, ynew, n, n) and !vis[xnew][ynew] and grid[xnew][ynew]==1){

            if(i==0) path.push_back('D');
            else if(i==1) path.push_back('L');
            else if(i==2) path.push_back('R');
            else path.push_back('U');

            dfs(grid, vis, n, xnew, ynew, path, res);

            path.pop_back();
        }
    }

    vis[x][y] = false;
}


vector<string> findPath(vector<vector<int>> &m, int n) {
    vector<string> res;
    if(m[0][0]==0) return res;

    string path = "";
    vector<vector<bool>> vis (n, vector<bool> (n, false));

    dfs (m, vis, n, 0, 0, path, res);
    return res;
}
```

<br>

### 7. Word Search
Given an m x n grid of characters board and a string word, return true if word exists in the grid. The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

![img](https://assets.leetcode.com/uploads/2020/11/04/word2.jpg)

Input: word = "ABCCED"

Output: true

```cpp
bool isInside(int x, int y, int row, int col){
    return x>=0 and y>=0 and x<row and y<col;
}


bool dfs (vector<vector<char>>& board, int x, int y, string& word, int idx, vector<vector<bool>>& vis, int& row, int& col){
    if(idx==word.size()) return true;

    vis[x][y] = true;

    int dx[] = {1, 0, -1, 0};
    int dy[] = {0, 1, 0, -1};

    for(int i=0; i<4; i++){
        int xnew = x + dx[i];
        int ynew = y + dy[i];

        if(isInside(xnew, ynew, row, col) and board[xnew][ynew]==word[idx] and !vis[xnew][ynew]){
            bool found = dfs(board, xnew, ynew, word, idx+1, vis, row, col);
            if(found) return true;
        }
    }

    vis[x][y] = false;
    return false;
}


bool exist(vector<vector<char>>& board, string word) {
    int row = board.size(), col = board[0].size();
    vector<vector<bool>> vis (row, vector<bool> (col, false));

    for(int i=0; i<row; i++){
        for(int j=0; j<col; j++){

            if(board[i][j]==word[0]){
                bool found = dfs (board, i, j, word, 1, vis, row, col);
                if(found) return true;
            }
        }
    }

    return false;
}
```

<br>

### 8. Word Search II (DFS + Trie)
Given an m x n board of characters and a list of strings words, return all words on the board. Each word must be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

Hint: Insert all words in trie, and explore all words simultaneously as move further inside trie.

```cpp
struct TrieNode{
    vector<TrieNode*> children;
    bool isTerminal;

    TrieNode(){
        children.assign(26, NULL);
        isTerminal = false;
    }
};

TrieNode* root;


void insert(string word){
    TrieNode* temp = root;

    for(char ch : word){
        int idx = ch-'a';

        if(!temp->children[idx])
            temp->children[idx] = new TrieNode();

        temp = temp->children[idx];
    }

    temp->isTerminal = true;
}


bool isInside(int x, int y, int row, int col){
    return x>=0 and y>=0 and x<row and y<col;
}


void dfs (vector<vector<char>>& board, vector<string> & res, vector<vector<bool>> & vis, TrieNode* root, int x, int y, int row, int col, string & s){

    vis[x][y] = true;

    /* if we reached any terminal node, that means we found the word, so insert it in res */

    if(root->isTerminal){
        res.push_back(s);
        root->isTerminal = false;     // --> Marking word to be found by removing terminal bit (for preventing duplicates in res)
        vis[x][y] = false;
    }


    /* do DFS for all childrens of trie */

    for(int i=0; i<26; i++){

        if(root->children[i]){
            char ch = 'a'+i;
            s.push_back(ch);

            int dx[] = {1, 0, -1, 0};
            int dy[] = {0, 1, 0, -1};

            for(int dir = 0; dir<4; dir++){
                int xnew = x + dx[dir];
                int ynew = y + dy[dir];

                /* If any option of trie lies in any 4 nbr cell, call the same function recursively */

                if(isInside(xnew, ynew, row, col) and board[xnew][ynew]==ch and !vis[xnew][ynew])
                    dfs (board, res, vis, root->children[i], xnew, ynew, row, col, s);
            }

            s.pop_back();
        }    
    }

    vis[x][y] = false;
}


vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
    root = new TrieNode();

    /* Insert all words in trie */

    for(string word : words)
        insert(word);

    int row = board.size(), col = board[0].size();
    vector<vector<bool>> vis(row, vector<bool> (col, false));
    vector<string> res;

    for(int i=0; i<row; i++){
        for(int j=0; j<col; j++){

            /* For each cell on board check if it is an option in trie */

            for(int c=0; c<26; c++){

                /* If it is an option then do DFS on that cell (both on board and trie simultaneously) */

                if(root->children[c] and board[i][j]=='a'+c){
                    string s = "";
                    s.push_back('a'+c);
                    dfs(board, res, vis, root->children[c], i, j, row, col, s);
                }
            }
        }
    }

    return res;
}
```

<br>

### 9. Detect Cycles in 2D Grid
A cycle is a path of length 4 or more in the grid that starts and ends at the same cell. From a given cell, you can move in four directions (up, down, left, or right), if it has the same value of the current cell. Also, you cannot move to the cell that you visited in your last move. Return true if any cycle of the same value exists in grid, otherwise, return false.

![img](https://assets.leetcode.com/uploads/2020/07/15/2.png)

```cpp
class Solution {
public:
    
    bool isInside (int x, int y, int row, int col){
        return x>=0 and x<row and y>=0 and y<col;
    }
    
    
    bool dfs(vector<vector<char>> &grid, int x, int y, int row, int col, vector<vector<bool>> &vis, vector<vector<bool>> &currPath, int prevX, int prevY){
        
        int dx[] = {1, 0, -1, 0};
        int dy[] = {0, 1, 0, -1};
        
        currPath[x][y] = true;
        vis[x][y] = true;
        
        for(int i=0; i<4; i++){
            int xnew = x + dx[i];
            int ynew = y + dy[i];
            
            if(isInside(xnew, ynew, row, col)){
                
                if(!(xnew==prevX and ynew==prevY) and currPath[xnew][ynew]) return true;
                
                if(!currPath[xnew][ynew] and grid[xnew][ynew] == grid[x][y]){
                    bool res = dfs(grid, xnew, ynew, row, col, vis, currPath, x, y);
                    if(res) return true;
                }
            }
        }
        
        currPath[x][y] = false;
        return false;
    }
        
    
    bool containsCycle(vector<vector<char>>& grid) {
        int row = grid.size(), col = grid[0].size();
        
        vector<vector<bool>> vis (row, vector<bool> (col, false));
        vector<vector<bool>> currPath (row, vector<bool> (col, false));
        
        for(int i=0; i<row; i++){
            for(int j=0; j<col; j++){
                
                if(!vis[i][j]){
                    bool cycle = dfs(grid, i, j, row, col, vis, currPath, -1, -1);
                    if(cycle) return true;
                }
            }
        }
        
        return false;
    }
};
```

<br>



## @ DFS with pruning

### 1. Cheapest Flights Within K Stops
There are n cities connected by some number of flights. You are given an array flights where flights[i] = [fromi, toi, pricei]. You are also given three integers src, dst, and k, return the cheapest price from src to dst with at most k stops. If there is no such route, return -1.

Note: This is simple approach using dfs. Optimized approach is using dijkstra.

```cpp
void dfs (unordered_map<int, vector<pair<int, int>>> & graph, vector<bool> & curr_path, int curr_cost, int & min_cost, int k, int src, int dst){
    if(k<-1 or curr_cost >= min_cost) return;    // ----> prune condition

    if(k>=-1 and src==dst){
        min_cost = min(min_cost, curr_cost);     // ----> if curr path cost is lesser then update min_cost
        return;
    }

    curr_path[src] = true;

    for(auto nbr : graph[src]){
        int nbr_node = nbr.first;
        int nbr_cost = nbr.second;

        if(!curr_path[nbr.first])
            dfs(graph, curr_path, curr_cost+nbr_cost, min_cost, k-1, nbr_node, dst);
    }

    curr_path[src] = false;
}


int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
    unordered_map<int, vector<pair<int, int>>> graph;
    vector<bool> curr_path (n, false);

    for(auto e : flights)
        graph[e[0]].push_back({e[1], e[2]});

    int min_cost = INT_MAX;

    dfs(graph, curr_path, 0, min_cost, k, src, dst);

    if(min_cost==INT_MAX) return -1;
    else return min_cost;
}
```

<br>

### 2. Path with good nodes
Given a tree with N nodes labelled from 1 to N. Each node is either good or bad denoted by binary array A of size N where if A[i] is 1 then ithnode is good else if A[i] is 0 then ith node is bad. Also the given tree is rooted at node 1 and you need to tell the number of root to leaf paths in the tree that contain not more than C good nodes.

Input: A = [0, 1, 0, 1, 1, 1],  B = [[1, 2], [1, 5], [1, 6], [2, 3], [2, 4]],  C = 1

Output: 3

```cpp
int dfs (unordered_map<int,vector<int>> &graph, vector<int> &good, int src, int C, vector<bool> &currPath){
    C -= good[src-1];

    if(C<0) return 0;           // --> If path contains more then C good nodes then return 0

    if(graph[src].size()==1)    // --> If currNode is leaf and path contains less then C node then return 1
        return 1;
    
    currPath[src-1] = true;
    int res = 0;

    for(int nbr : graph[src]){

        if(!currPath[nbr-1])
            res += dfs(graph, good, nbr, C, currPath);
    }

    currPath[src-1] = true;
    return res;
}


int Solution::solve(vector<int> &A, vector<vector<int> > &B, int C) {
    unordered_map<int,vector<int>> graph;
    vector<bool> currPath(A.size(), false);
    
    for(auto e : B){
        graph[e[0]].push_back(e[1]);
        graph[e[1]].push_back(e[0]);
    }

    return dfs (graph, A, 1, C, currPath);
}
```

<br>



## @ Hard to Guess as Graphs

### 1. Jump Game III
Given an array of non-negative integers arr, you are initially positioned at start index of the array. When you are at index i, you can jump to i + arr[i] or i - arr[i], check if you can reach to any index with value 0.

Input: arr = [4,2,3,0,3,1,2], start = 5

Output: true

```cpp
void dfs(vector<int> & arr, int idx, bool & found){
    if(arr[idx]==0){
        found = true;
        return;
    }

    int left_nbr = idx-arr[idx];
    int right_nbr = idx+arr[idx];

    arr[idx] = -1;    // --> marking index vis (after storing left and right nbr)

    if(left_nbr>=0 and arr[left_nbr]!=-1)
        dfs(arr, left_nbr , found);            // --> dfs call to left (i-arr[i])
        
    if(!found and right_nbr<arr.size() and arr[right_nbr]!=-1) 
        dfs(arr, right_nbr, found);            // --> dfs call to right (i+arr[i])
}

bool canReach(vector<int>& arr, int start) {
    bool found = false;
    dfs(arr, start, found);
    return found;
}
```

<br>

### 2. Evaluate Division
You are given an array of variables equations and an array of real numbers values, where equations[i] = [Ai, Bi] and values[i] represent the equation Ai / Bi = values[i]. Each Ai or Bi is a string that represents a single variable.

You are also given some queries, where queries[j] = [Cj, Dj] represents the jth query where you must find the answer for Cj / Dj = ?. Return the answers to all queries. If a single answer cannot be determined, return -1.0.

Input: equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]

Output: [6.00000,0.50000,-1.00000,1.00000,-1.00000]

[Video Explaination](https://www.youtube.com/watch?v=UcDZM6Ap5P4)

Hint: Represent all variables as vertices of graph and values as edge weights. Then perform dfs to find a path from vertice 1 to vertice 2.

```cpp
void dfs (unordered_map<string, vector<pair<string, double>>> & graph, unordered_set<string> & vis, string src, string end, double tempres, double & res){
    vis.insert(src);     // ---> mark src vertex visited

    if(src == end){
        res = tempres;
        return;
    }

    for(auto nbr_vt : graph[src]){
        string nbr = nbr_vt.first;
        double val = nbr_vt.second;

        if(vis.find(nbr)==vis.end() and res<0)
            dfs(graph, vis, nbr, end, tempres*val, res);
    }        

    vis.erase(src);
}

vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {

    unordered_map<string, vector<pair<string, double>> > graph;

    for(int i=0; i<equations.size(); i++){
        graph[equations[i][0]].push_back({equations[i][1], values[i]});
        graph[equations[i][1]].push_back({equations[i][0], 1.0/values[i]});
    }

    vector<double> ans;

    for(auto q : queries){
        string v1 = q[0], v2 = q[1];

        if(graph.find(v1)==graph.end() or graph.find(v2)==graph.end())
            ans.push_back(-1);

        else if (v1==v2)
            ans.push_back(1);

        else{
            unordered_set<string> vis;
            double res = -1;

            dfs(graph, vis, v1, v2, 1, res);
            ans.push_back(res);
        }
    }

    return ans;
}
```
