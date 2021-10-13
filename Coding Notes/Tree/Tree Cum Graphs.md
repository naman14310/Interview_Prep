# Problems on Trees Represented as Graphs

Trees given in the form of edges or parent_arrays can be treated as graphs. DFS calls similar to graphs should be used!

<br>


### 1. Largest Distance between nodes of a Tree (Tricky)
Given an arbitrary unweighted rooted tree which consists of N nodes. The goal of the problem is to find largest distance between two nodes in a tree. The tree is given as an array A, there is an edge between nodes A[i] and i (0 <= i < N). Exactly one of the i's will have A[i] equal to -1, it will be root node.

Hint: Graph variation of max diameter of tree

```cpp

int dfs (unordered_map<int, vector<int>> &graph, int src, int &diameter){

    if(graph[src].size()==0) return 0;      // --> leaf node

    int mx1 = 0, mx2 = 0;                   // --> Will represent two maxlength of subtrees rooted at src

    for(int nbr : graph[src]){
        int depth = dfs (graph, nbr, diameter)+1;

        if(depth>=mx1){
            mx2 = mx1;
            mx1 = depth;
        }
        else if(depth>mx2)
            mx2 = depth;
    }

    diameter = max(diameter, mx1+mx2);
    return max(mx1, mx2);
}


int solve(vector<int> &A) {
    unordered_map<int, vector<int>> graph;
    int diameter = 0;
    int root = 0;

    for(int i=0; i<A.size(); i++){
        if(A[i]==-1)
            root = i;
        else
            graph[A[i]].push_back(i);
    }

    int res = dfs(graph, root, diameter);
    return diameter;
}

```

<br>

### 2. Maximum Edge Removal (Tricky)
Given an undirected tree with an even number of nodes. Consider each connection between a parent and child node to be an edge. You need to remove maximum number of these edges, such that the disconnected subtrees that remain each have an even number of nodes. Return the maximum number of edges you can remove.

```
Input: A = 6, B = [[1, 2], [1, 3], [1, 4], [3, 5]. [4, 6]]

Output: 2

Explanation:

      1
    / | \
   2  3  4
      |   \
      5    6
      
Maximum number of edges we can remove is 2, i.e (1, 3) and (1, 4)
```

Hint: Do DFS traversal initially starting from leaf node. Temp res (return from traversal of one nbr) will give answer for one branch.

```cpp

/* function will return uncutted nodes */

int dfs (unordered_map<int, vector<int>> &graph, int src, int &cuts){
    if(graph[src].size()==0) return 1;      // --> leaf node

    int nodes = 1;                          // --> 1 represent curr node

    /* Since uncutted nodes from each individual branch will always remain odd we will simply add them */

    for(int nbr : graph[src])
        nodes += dfs(graph, nbr, cuts);
    
    /* If total uncutted nodes in subtree rooted at src is even, then do one cut and return 0 */
    
    if(nodes%2==0){
        cuts++;
        return 0;
    }
    
    /* Else return odd number of uncutted nodes */

    return nodes;
}


int solve(int A, vector<vector<int> > &B) {
    int cuts = 0;

    unordered_map<int, vector<int>> graph;
    for(auto e : B)
        graph[e[0]].push_back(e[1]);

    dfs(graph, 1, cuts);
    return cuts-1;      // --> Since for making n seperate trees, we need to make n-1 cuts!
}
```

<br>

### 3. Delete Edge
Given a undirected tree with N nodes labeled from 1 to N. Each node has a certain weight assigned to it given by an integer array A of size N. You need to delete an edge in such a way that Product between sum of weight of nodes in one subtree with sum of weight of nodes in other subtree is maximized. Return this maximum possible product modulo 109 + 7.

```cpp
long long dfs (unordered_map<int, vector<int>> &graph, vector<int> &weight, int src, long long &ans, int prev){

    if(graph[src].size()==1)        // --> leaf node 
        return weight[src-1];

    long long totalSum = weight[src-1];
    vector<long long> subtreeSum;

    for(auto nbr : graph[src]){
        if(nbr==prev) continue;

        subtreeSum.push_back(dfs(graph, weight, nbr, ans, src));
        totalSum = totalSum + subtreeSum.back();
    }
        
    for(auto sum : subtreeSum){
        long long a = sum;
        long long b = (totalSum-sum);
        ans = max(ans, (a*b));
    }
    
    return totalSum;
}


int Solution::deleteEdge(vector<int> &A, vector<vector<int> > &B) {
    unordered_map<int, vector<int>> graph;

    for(auto e : B){
        graph[e[0]].push_back(e[1]);
        graph[e[1]].push_back(e[0]);
    }

    long long ans = 0;
    
    dfs(graph, A, 1, ans, 0);
    return ans % 1e+7;
}
```

<br>

### 4. Sum of Distances in Tree (Tricky)
There is an undirected connected tree with n nodes labeled from 0 to n - 1 and n - 1 edges. You are given the integer n and the array edges where edges[i] = [ai, bi] indicates that there is an edge between nodes ai and bi in the tree. Return an array answer of length n where answer[i] is the sum of the distances between the ith node in the tree and all other nodes.

![img](https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist1.jpg)

Input: n = 6, edges = [[0,1],[0,2],[2,3],[2,4],[2,5]]

Output: [8,12,6,10,10,10]

Approach: 
1. Consider 0 as a root. 
2. Compute total distances of all node from 0. Also compute total count of nodes in a subtree of every node using same dfs function.
3. Now again write a dfs2 function for computing dist of every node using preorder traversal and using formula:
    
    dist[src] = dist[parent] + (count[0] - count[src]) - count[src]


```cpp
/* This function will return {total_distance, node_cnt in subtree} */

pair<int,int> dfs (unordered_map<int, vector<int>> &graph, int src, vector<int> &count, int level, int parent){
    int dist = 0, cnt = 0;

    for(auto nbr : graph[src]){
        if(nbr==parent) continue;

        auto p = dfs(graph, nbr, count, level+1, src); 
        dist += p.first;
        cnt += p.second;
    }

    count[src] = cnt+1;
    return {dist+level, count[src]};
}


/* This function iterate tree (rooted at 0) in preorder and fill dist vector for all nodes */

void dfs2 (unordered_map<int, vector<int>> &graph, int src, vector<int> &count, vector<int> &dist, int parent){

    if(src!=0){
        int extra_dist = (count[0] - count[src]);
        int reduced_dist = count[src];

        dist[src] = dist[parent] + extra_dist - reduced_dist;  
    } 

    for(auto nbr : graph[src]){
        if(nbr==parent) continue;
        dfs2 (graph, nbr, count, dist, src);
    }
}


vector<int> sumOfDistancesInTree(int n, vector<vector<int>>& edges) {

    unordered_map<int, vector<int>> graph;
    for(auto e : edges){
        graph[e[0]].push_back(e[1]);
        graph[e[1]].push_back(e[0]);
    }

    vector<int> count (n, 0);
    vector<int> dist (n, 0);

    auto p = dfs (graph, 0, count, 0, -1);        
    int dist0 = p.first;

    dist[0] = dist0;
    dfs2 (graph, 0, count, dist, -1);

    return dist;
}
```
