# Basic Graph Problems

## @ Questions based on Indegree and Outdegree

<br>

### 1. Find the Town Judge
In a town, there are n people labelled from 1 to n. One of these people is secretly the town judge. If the town judge exists, then:
1. The town judge trusts nobody.
2. Everybody (except for the town judge) trusts the town judge.
3. There is exactly one person that satisfies properties 1 and 2.

You are given trust, an array of pairs trust[i] = [a, b] representing that the person labelled a trusts the person labelled b. If the town judge exists and can be identified, return the label of the town judge.  Otherwise, return -1.

Hint: Consider trust as a graph, all pairs are directed edge. The point with in-degree - out-degree = N - 1 become the judge.

```cpp
/*
   do -- for outdegre
   and ++ for indegre
*/

int findJudge(int n, vector<vector<int>>& trust) {
    vector<int> degre (n+1, 0);

    for(auto p : trust){
        degre[p[0]]--;
        degre[p[1]]++;
    }

    for(int i=1; i<=n; i++)
        if(degre[i]==n-1) return i;

    return -1;
}
```

<br>

### 2. Minimum Number of Vertices to Reach All Nodes
Given a directed acyclic graph, with n vertices numbered from 0 to n-1, and an array edges where edges[i] = [from_i, to_i] represents a directed edge from node from_i to node to_i. Find the smallest set of vertices from which all nodes in the graph are reachable. It's guaranteed that a unique solution exists.

Input: n = 6, edges = [[0,1],[0,2],[2,5],[3,4],[4,2]]

Output: [0,3]

Hint: Find all nodes with 0 indegree

```cpp
 vector<int> findSmallestSetOfVertices(int n, vector<vector<int>>& edges) {
     vector<int> res;
     vector<int> indegre (n, 0);

     for(auto e : edges)
         indegre[e[1]]++;

     for(int i=0; i<n; i++)
         if(indegre[i]==0) res.push_back(i);

     return res;
 }
```

<br>

### 3. Clone graph
Given a reference of a node in a connected undirected graph. Return a deep copy (clone) of the graph.

![img](https://assets.leetcode.com/uploads/2019/11/04/133_clone_graph_question.png)

Method 1: DFS

```cpp
 Node* dfs (Node* node, unordered_map<Node*, Node*> & mp){

     /* Create a new clone of node and insert a mapping to map */

     Node* cloned_node = new Node(node->val);
     mp[node] = cloned_node;

     for(int i=0; i<node->neighbors.size(); i++){
         Node* nbr = node->neighbors[i];

         /* If nbr is not vis, make a dfs call and pushback returned node to nbrs_vector of cloned node */ 

         if(mp.find(nbr)==mp.end()){
             Node* cloned_nbr = dfs(nbr, mp);
             cloned_node->neighbors.push_back(cloned_nbr);
         }

         /* Else simply push_back nbr to nbrs_vector of cloned node */

         else
             cloned_node->neighbors.push_back(mp[nbr]);
     }

     return cloned_node;
 }

 Node* cloneGraph(Node* node) {
     if(!node) return NULL;

     unordered_map<Node*, Node*> mp;
     return dfs(node, mp);   
 }
```

Method 2: BFS

```cpp
 Node* bfs (Node* node, unordered_map<Node*, Node*>& mp){

     /* Creating a clone for initial node and insert mapping in mp */

     mp[node] = new Node(node->val);

     queue<Node*> q;
     q.push(node);

     while(!q.empty()){

         /* pop one node from q and name it vt */

         Node* vt = q.front(); q.pop();

         for(int i=0; i<vt->neighbors.size(); i++){                
             Node* nbr = vt->neighbors[i];

             /* If nbr is not visited then create a clone of nbr and pushback it in mp[vt]->neighbors */

             if(mp.find(nbr)==mp.end()){
                 mp[nbr] = new Node(nbr->val);
                 mp[vt]->neighbors.push_back(mp[nbr]);
                 q.push(nbr);
             }

             /* Else simply insert mp[nbr] in mp[vt]->neighbours */

             else
                 mp[vt]->neighbors.push_back(mp[nbr]);   
         }
     }

     return mp[node];
 }

 Node* cloneGraph(Node* node) {
     if(!node) return NULL;

     unordered_map<Node*, Node*> mp;
     return bfs (node, mp);
 }
```

<br>

### 4. Minimum Height Trees
Given a tree of n nodes labelled from 0 to n - 1, and an array of n - 1 edges where edges[i] = [ai, bi] indicates an undirected edge, you can choose any node of the tree as the root. When you select a node x as the root, the result tree has height h. Among all possible rooted trees, those with minimum height (i.e. min(h))  are called minimum height trees (MHTs). Return a list of all MHTs' root labels.

Input: n = 6, edges = [[3,0],[3,1],[3,2],[3,4],[5,4]]

Output: [3,4]

![img](https://assets.leetcode.com/uploads/2020/09/01/e2.jpg)

Hint: The idea is to eat up all the leaves at the same time, until one/two leaves are left.

```cpp
 vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
     if(n==1) return vector<int> (1, 0);

     unordered_map<int, unordered_set<int>> graph;
     queue<int> q;

     for(auto e : edges){
         graph[e[0]].insert(e[1]);
         graph[e[1]].insert(e[0]);
     }

     for(int i=0; i<n; i++)
         if(graph[i].size()==1)
             q.push(i);

     while(n>2){            
         int len = q.size();

         for(int i=0; i<len; i++){
             int node = q.front(); q.pop();

             for(int nbr : graph[node]){   
                 graph[nbr].erase(node);

                 if(graph[nbr].size()==1)
                     q.push(nbr);
             }
         }

         n -= len;
     }

     vector<int> res;

     while(!q.empty()){
         res.push_back(q.front());
         q.pop();
     }

     return res;
 }
```
