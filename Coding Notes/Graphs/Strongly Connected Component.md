# Strongly Connected Components in Directed Graph

If we can reach from every vertex to every other vertex in a component then it is called strongly connected component. A single node is always a strongly connected component.

Note: All the nodes in a component are always strongly connected in an undirected graph. Problems of SCC on undirected graph can be solved using `DS Union Find`.

**Graph Transpose Property**
1. On reversing all edges of the graph, the type of graph won't be change i.e. SCC will remain SCC. 
2. Direction between components will be reversed, Therefore this property can be used to detect SCC's.


### Kosaraju Algo
1. Perform DFS traversal of graph. Push node to stack before returning.
2. Find the transpose graph by reversing the edges.
3. Pop nodes one by one from stack and again do DFS on modified. Each successfull DFS will give 1 SCC.

```cpp
#include<bits/stdc++.h>
using namespace std;

class graph{

    unordered_map<int, vector<int>> mp, rev;          // ----> one for original graph and other for transposed graph (containing reversed edges)
    int V;
    stack<int> s; 
    
    public:

        graph(int v){
            V = v;
        }

        void addEdge(int u, int v){
            mp[u].push_back(v);
        }
        
        /* Step 1 : Doing DFS traversal and pushing nodes to stack at the end */

        void dfs1(vector<bool> &vis, int src){
            vis[src] = true;
            for(int nbr : mp[src]){
                if(!vis[nbr]){
                    dfs1(vis, nbr);
                }
            }
            s.push(src);
        }
        
        /* Step 2 : Creating transposed graph by reversing edges */

        void transpose(){
            for(auto v : mp){
                for(int nbr : v.second){
                    rev[nbr].push_back(v.first);
                }
            }
        }
        
        /* Step 3 : Doing DFS traversal on node poped from stack */

        void dfs2(vector<bool> &vis, int src){
            vis[src] = true;
            for(int nbr : rev[src]){
                if(!vis[nbr]){
                    dfs2(vis, nbr);
                }
            }
        }

        void kosaraju(){
            int components = 0;
            vector<bool> vis1 (V, false);
            vector<bool> vis2 (V, false);
      
            for(int i=0; i<V; i++){
                if(!vis1[i])
                dfs1(vis1, i);
            }

            transpose();
            
            /* Poping nodes one by one performing DFS traversals */
            
            while(!s.empty()){
                int src = s.top(); s.pop();
                if(!vis2[src]){
                    dfs2(vis2,src);
                    components++;
                }
            }

            cout<<"No of Stringly Connected Components are : "<<components<<endl;
        }

};

int main(){
    graph g(8);
    g.addEdge(0,1);
    g.addEdge(1,2);
    g.addEdge(2,0);
    g.addEdge(2,3);
    g.addEdge(3,4);
    g.addEdge(4,5);
    g.addEdge(4,7);
    g.addEdge(5,6);
    g.addEdge(6,4);
    g.addEdge(6,7);

    g.kosaraju();
    return 0;
}
```
