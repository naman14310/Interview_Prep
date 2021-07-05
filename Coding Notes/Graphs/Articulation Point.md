# Articulation Points (or Cut Vertex)

A node is defined as Articulation point if on removing it, no. of components in the graph increases.

**Application**
Find single point of failure in a network

### 1. Simple Approach (Brute Force)
For each node do, 
1. Take out the node and all passing edges.
2. Find if we have only one component.
3. If component==1 then Node is not AP else Node is AP.
  
Time Complexity : O(V*(V+E))

### 2. Using Tarjans Algo

**Conditions of AP**
1. If node U is root of DFS tree and has atleast 2 childrens (i.e. independent subgraphs)
2. If node U is not root and it has a child V such that no vertex in subgraph rooted with V has a backedge to one of the ancestors of U.

[Video Explaination](https://www.youtube.com/watch?v=64KK9K4RpKE)

```cpp
void tarjans_algo (unordered_map<int, vector<int>> & graph, vector<int> & discovery_time, vector<int> & low_time, vector<int> & parent, vector<bool> & AP, int src, int & time){

    int child = 0;
    discovery_time[src] = time; 
    low_time[src] = time;

    for(int nbr : graph[src]){

        /* If nbr is not vis */

        if(discovery_time[nbr]==-1){

            child++;
            time++;
            parent[nbr] = src;

            tarjans_algo (graph, discovery_time, low_time, parent, AP, nbr, time);
            low_time[src] = min (low_time[src], low_time[nbr]);
            
            /************************ Following are two conditions for articulation point *****************************/

            /* Cond 1 : if src node is root node and have two independent child subgraphs then its an articulation point */

            if (parent[src]==-1 and child >= 2)
                AP[src] = true;

            /* Cond 2 : Else If src node is not root node and has a subgraph with NO backedge to any of its ancestors */

            else if(parent[src]!=-1 and low_time[nbr] >= discovery_time[src])
                AP[src] = true;
                
            /**********************************************************************************************************/
        }

        /* Else if nbr is vis and not the parent that means the edge is backedge */

        else if (parent[src]!=nbr)
            low_time[src] = min (low_time[src], discovery_time[nbr]);

    }
}


vector<int> findArticulationPoints(unordered_map<int, vector<int>> & graph, int n){
    vector<int> discovery_time (n, -1);
    vector<int> low_time (n, -1);
    vector<int> parent (n, -1);
    vector<bool> AP (n, false);

    int time = 0;
    tarjans_algo(graph, discovery_time, low_time, parent, AP, 0, time);

    vector<int> res;
    for(int i=0; i<n; i++)
        if(AP[i]) res.push_back(i);
    
    return res;
}
```

# Bridges (or Cut Edges)

A bridge is an edge, removing which increases the number of components.

**Application**
Find critical connections in a network

### 1. Simple Approach (Brute Force)
For each edge repeat, 
1. Remove edge from the graph.
2. Find if we have only one component.
3. If component==1 then removed edge is not a bridge else it is a bridge.
  
Time Complexity : O(E*(V+E))

#### Critical Connections in a Network
There are n servers numbered from 0 to n - 1 connected by undirected server-to-server connections forming a network where connections[i] = [ai, bi] represents a connection between servers ai and bi. A critical connection is a connection that, if removed, will make some servers unable to reach some other server. Return all critical connections in the network in any order.

![img](https://assets.leetcode.com/uploads/2019/09/03/1537_ex1_2.png)

```cpp
void tarjans_algo (unordered_map<int, vector<int>>& graph, vector<int>& discovery_time, vector<int>& low_time, vector<int>& parent, vector<vector<int>>& bridges, int src, int & time){

    discovery_time[src] = time;
    low_time[src] = time;

    for(int nbr : graph[src]){

        /* if nbr is not vis */

        if(discovery_time[nbr]==-1){

            time++;
            parent[nbr] = src;

            tarjans_algo (graph, discovery_time, low_time, parent, bridges, nbr, time);

            low_time[src] = min (low_time[src], low_time[nbr]);


            /************************ Following is a condition for Bridges ***************************/

            if(low_time[nbr] > discovery_time[src]){
                vector<int> e = {src, nbr};
                bridges.push_back(e);
            }

            /****************************************************************************************/    

        }

        /* If nbr is vis and not the parent that means its a back edge */

        else if(parent[src]!=nbr)
            low_time[src] = min (low_time[src], discovery_time[nbr]);

    }
}


vector<vector<int>> findBridges(int n,  unordered_map<int, vector<int>>& graph){
    vector<int> discovery_time (n, -1);
    vector<int> low_time (n, -1);
    vector<int> parent (n, -1);

    int time = 0;
    vector<vector<int>> bridges;

    tarjans_algo(graph, discovery_time, low_time, parent, bridges, 0, time);

    return bridges;
}

vector<vector<int>> criticalConnections(int n, vector<vector<int>>& connections) {
    unordered_map<int, vector<int>> graph;

    for(auto e : connections){
        graph[e[0]].push_back(e[1]);
        graph[e[1]].push_back(e[0]);
    }

    return findBridges(n, graph);
}
```
