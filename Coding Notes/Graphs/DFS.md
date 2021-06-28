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
