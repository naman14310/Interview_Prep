# Minimum Cost Spanning Tree

Spanning Tree : cover all vertices with minimum possible edges

**Properties:**
1. Spanning Tree is a subset of graph.
2. Spanning Tree cannot be disconnected
3. Graph should be single component to find the spanning tree
4. Spanning tree is Maximally Acyclic

**Note:** In a graph of 'V' vertices, Number of Edges in ST = V-1

MinCost Spanning Tree : A Spanning tree with min cost in a weighted graph is called MST.

## Prims Algo

It is a greedy approach and very much similar to dijkstra.

**Difference between Prims and Dijkstra :** In Dijkstra we will relax the nbrs by computing new_edge_weight + cost_so_far whereas in Prims, we will relax the nbrs by using only new_edge_weight,

[Video Explaination](https://www.youtube.com/watch?v=Z_yuwfriWAw)

```cpp
int MST (unordered_map<int, vector<pair<int,int>>> & graph, int V){
    int min_MST_cost = 0;
    int vis_count = 0;

    vector<int> cost (V, INT_MAX);
    cost[0] = 0;

    set<pair<int,int>> s;    // ----> set of {cost, vertex}
    s.insert({0, 0});

    vector<bool> vis (V, false); 

    while(!s.empty() and vis_count < V){
        auto itr = s.begin();
        int vertex = itr->second, weight = itr->first; 
        s.erase(itr);

        min_MST_cost += weight;

        for(auto nbr : graph[vertex]){
            int nbr_v = nbr.first;

            if(vis[nbr_v]) continue;

            int old_cost = cost[nbr_v];
            int new_cost = nbr.second;      // ----> Just one line difference between dijkstra and prims algo 

            if(new_cost < old_cost){
                pair<int,int> ptr = {old_cost, nbr_v};

                if(s.find(ptr)!=s.end())
                    s.erase(ptr);

                s.insert({new_cost, nbr_v});
                cost[nbr_v] = new_cost;
            }
        }

        vis[vertex] = true;
        vis_count++;
    }

    return min_MST_cost;
}
```
