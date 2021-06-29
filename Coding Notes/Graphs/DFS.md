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

#### 2. Find Eventual Safe States
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

#### 3. Course Schedule
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

#### 3. Number of Islands

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

#### 4. Pacific Atlantic Water Flow
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


## @ Graph coloring

#### 1. Is Graph Bipartite?

Hint: If graph can be coloured with only two colours such that no two vertices have same color then it is Bipartite.

```cpp
/*
    -1 --> uncoloured (unvisited)
    0 --> red
    1 --> blue  
*/

bool dfs (vector<vector<int>>& graph, vector<int> & color, int src){
    int node_color = color[src];

    for(int nbr : graph[src]){

        if(color[nbr]==-1){
            color[nbr] = !node_color;
            bool is_bipartite = dfs(graph, color, nbr);
            if(!is_bipartite) 
                return false;
        }

        else if(color[nbr]==node_color)
            return false;
    }

    return true;
}

bool isBipartite(vector<vector<int>>& graph) {
    int n = graph.size();
    vector<int> color (n, -1);

    for(int i=0; i<n; i++){
        if(color[i]==-1){
            color[i] = 0;
            bool res = dfs(graph, color, i);
            if(!res) return false;
        }
    }

    return true;
}
```

#### 2. Possible Bipartition
Given a set of n people (numbered 1, 2, ..., n), we would like to split everyone into two groups of any size. Formally, if dislikes[i] = [a, b], it means it is not allowed to put the people numbered a and b into the same group. Return true if and only if it is possible to split everyone into two groups in this way.

Hint: Check if graph is bipartite or not

```cpp
bool dfs (unordered_map<int, vector<int>>& graph, vector<int>& color, int src){
    int node_color = color[src];

    for(int nbr : graph[src]){

        /* If nbr is not visited */

        if(color[nbr]==-1){
            color[nbr] = !node_color;

            bool res = dfs(graph, color, nbr);
            if(!res) return false;
        }

        /* If nbr is vis but of same color */

        else if(color[nbr]==node_color)
            return false;
    }

    return true;
}


bool possibleBipartition(int n, vector<vector<int>>& dislikes) {
    vector<int> color (n+1, -1);
    unordered_map<int, vector<int>> graph;

    for(auto e : dislikes){
        graph[e[0]].push_back(e[1]);
        graph[e[1]].push_back(e[0]);
    }

    for(int i=1; i<=n; i++){
        if(color[i]==-1){
            bool res = dfs(graph, color, i);
            if(!res) return false;
        }
    }

    return true;
}
```

## @ Hard to Guess as Graphs

#### 1. Jump Game III
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

#### 2. Evaluate Division
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
