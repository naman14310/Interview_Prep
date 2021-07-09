# Graph coloring

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

#### 3. M-Coloring Problem 
Given an undirected graph and an integer M. The task is to determine if the graph can be colored with at most M colors such that no two adjacent vertices of the graph are colored with the same color.

```cpp
bool dfs (unordered_map<int, vector<int>> & graph, vector<int> & color, int src, int m){
    bool change_color = false;
    
    /* Iterating for all m colours on src */ 
    
    for(int i=1; i<=m; i++){
        color[src] = i;
        
        /* After filling ith color, check for its nbrs */
        
        for(int nbr : graph[src]){
            
            /* If color of nbr is same as color of source, break the loop and try other color on source */
            
            if(color[nbr] == color[src]){
                change_color = true;
                break;
            }
            
            /* If nbr is uncoloured, make a dfs call on nbr */
            
            if(color[nbr] == 0){
                bool res = dfs (graph, color, nbr, m);
                
                /* If dfs call returns false, then break the loop and try other color on source */
                
                if(!res){
                    change_color = true;
                    break;
                }
            }
        }
        
        if(change_color) change_color = false;
        else return true;
        
    }
    
    color[src] = 0;
    return false;
}
```
