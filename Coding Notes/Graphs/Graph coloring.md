# Graph coloring

#### 1. 


#### 2. M-Coloring Problem 
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
