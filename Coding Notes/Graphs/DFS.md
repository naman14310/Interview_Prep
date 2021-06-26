# DFS 

## @ DFS on Adjacency List

#### 1. All Paths From Source to Target
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

## @ DFS on Matrix

#### 1. Max Area of Island

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

#### 2. Number of Closed Islands
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
