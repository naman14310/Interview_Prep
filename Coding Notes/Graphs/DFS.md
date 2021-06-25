# DFS

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
