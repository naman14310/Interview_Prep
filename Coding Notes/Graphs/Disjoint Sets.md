# Disjoint Sets (Union-Find)
Two or more sets with nothing in common are called disjoint sets.

**Use of Disjoint sets**
1. Find operation: It Keeps track of the set that an element belongs to. Hence, It'll be easier to check whether they belong to same subset ot not.
2. Union operation: Used to merge two sets into one.

#### 1. Redundant Connection
You are given a graph that started as a tree with n nodes labeled from 1 to n, with one additional edge added. The graph is represented as an array edges of length n where edges[i] = [ai, bi] indicates that there is an edge between nodes ai and bi in the graph. Return an edge that can be removed so that the resulting graph is a tree of n nodes. If there are multiple answers, return the answer that occurs last in the input.

![img](https://assets.leetcode.com/uploads/2021/05/02/reduntant1-2-graph.jpg)

Input: edges = [[1,2],[2,3],[3,4],[1,4],[1,5]]

Output: [1,4]

```cpp
struct vt{
    int parent;
    int rank;

    vt(int p, int r){
        parent = p;
        rank = r;
    }
};

int ds_find(vector<vt> & ds, int v){

    /* find operation with path compression */

    if(ds[v].parent == -1) 
        return v;

    return ds[v].parent = ds_find(ds, ds[v].parent);    
}

void ds_union(vector<vt> & ds, int v1, int v2){

    /* union by rank */

    if(ds[v1].rank > ds[v2].rank)
        ds[v2].parent = v1;

    else if(ds[v1].rank < ds[v2].rank)
        ds[v1].parent = v2;

    else{
        ds[v1].parent = v2;
        ds[v2].rank++;
    } 
}

vector<int> findRedundantConnection(vector<vector<int>>& edges) {
    int n = edges.size();

    /* created a vector of type <struct vt> of size n+1 (as indexing starts from 1) */

    vector<vt> ds (n+1, vt(-1, 0));          

    for(auto e : edges){

        int root1 = ds_find(ds, e[0]);      
        int root2 = ds_find(ds, e[1]);

        if(root1==root2)       // ----> cycle detected
            return e;

        ds_union(ds, root1, root2);
    }

    return edges.back();
}
```

#### 2. Number of Provinces
There are n cities. Some of them are connected, while some are not. If city a is connected directly with city b, and city b is connected directly with city c, then city a is connected indirectly with city c. A province is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an n x n matrix isConnected where isConnected[i][j] = 1 if the ith city and the jth city are directly connected, and isConnected[i][j] = 0 otherwise. Return the total number of provinces.

```cpp
struct vt{
    int parent;
    int rank;
    vt(int p, int r){
        parent = p;
        rank = r;
    }
};

int ds_find(vector<vt> & ds, int v){
    if(ds[v].parent == -1)
        return v;

    return ds[v].parent = ds_find(ds, ds[v].parent);
}

void ds_union(vector<vt> & ds, int v1, int v2){
    if(ds[v1].rank < ds[v2].rank)
        ds[v1].parent = v2;

    else if(ds[v1].rank > ds[v2].rank)
        ds[v2].parent = v1;

    else{
        ds[v1].parent = v2;
        ds[v2].rank++;
    }        
}

int findCircleNum(vector<vector<int>>& isConnected) {
    int n = isConnected.size();
    vector<vt> ds (n, vt(-1, 0));

    for(int i=0; i<n; i++){
        for(int j=i+1; j<n; j++){

            if(isConnected[i][j]){
                int root1 = ds_find(ds, i);
                int root2 = ds_find(ds, j);

                if(root1!=root2)
                    ds_union(ds, root1, root2);
            }
        }
    }

    int province = 0;

    for(int i=0; i<n; i++)
        if(ds[i].parent==-1) province++;

    return province;
}
```
