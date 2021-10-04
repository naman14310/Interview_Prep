# Disjoint Sets (Union-Find)
Two or more sets with nothing in common are called disjoint sets.

<br>

**Use of Disjoint sets**
1. Cycle detection in undirected graph.
2. Finding number of components (or Strongly connected components) in an undirected graph.
3. For grouping items into multiple sets or groups.

<br>

**Operation of Disjoint Sets:**
1. Find operation: It returns the absolute root of set. Hence, It'll be easier to check whether they belong to same subset ot not.
2. Union operation: Used to merge two sets into one.

<br>

### 1. Redundant Connection
You are given a graph that started as a tree with n nodes labeled from 1 to n, with one additional edge added. The graph is represented as an array edges of length n where edges[i] = [ai, bi] indicates that there is an edge between nodes ai and bi in the graph. Return an edge that can be removed so that the resulting graph is a tree of n nodes. If there are multiple answers, return the answer that occurs last in the input.

![img](https://assets.leetcode.com/uploads/2021/05/02/reduntant1-2-graph.jpg)

Input: edges = [[1,2],[2,3],[3,4],[1,4],[1,5]]

Output: [1,4]

```cpp
struct vt{
    int parent, rank;
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

<br>

### 2. Number of Provinces
There are n cities. Some of them are connected, while some are not. If city a is connected directly with city b, and city b is connected directly with city c, then city a is connected indirectly with city c. A province is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an n x n matrix isConnected where isConnected[i][j] = 1 if the ith city and the jth city are directly connected, and isConnected[i][j] = 0 otherwise. Return the total number of provinces.

```cpp
struct vt{
    int parent, int rank;
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

<br>

### 3. Number of Operations to Make Network Connected
Given an initial computer network connections. You can extract certain cables between two directly connected computers, and place them between any pair of disconnected computers to make them directly connected. Return the minimum number of times you need to do this in order to make all the computers connected. If it's not possible, return -1. 

![img](https://assets.leetcode.com/uploads/2020/01/02/sample_1_1677.png)

Output: 1

Hint: Find total no. of groups after adding all edges one by one and total extra cables (when both pc already exist in same group, then cable between them is extra).

```cpp
struct vt{
    int parent, rank;
    vt (int p, int r){
        parent = p;
        rank = r;
    }
};

int ds_find(vector<vt> & ds, int v){
    if(ds[v].parent==-1)
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

int makeConnected(int n, vector<vector<int>>& connections) {
    vector<vt> ds (n, vt(-1, 0));
    int extra_cable = 0, groups = 0;

    for(auto e : connections){
        int root1 = ds_find(ds, e[0]);
        int root2 = ds_find(ds, e[1]);

        if(root1==root2) extra_cable++;
        
        else ds_union(ds, root1, root2);
    }

    for(int i=0; i<n; i++)
        if(ds[i].parent==-1) groups++;

    return groups-1>extra_cable ? -1 : groups-1;
}
```

<br>

### 4. Satisfiability of Equality Equations
Given an array equations, each string equations[i] has length 4 and takes one of two different forms: "a==b" or "a!=b".  Here, a and b are lowercase letters. Return true if and only if it is possible to assign integers to variable names so as to satisfy all the given equations.

Input: ["a==b","b!=c","c==a"]

Output: false

Hint: All "==" equations actually represent the connection in the graph. The connected nodes should be in the same union/set. Then we check all inequations. Two inequal nodes should be in the different union/set.

```cpp
struct vt{
    int parent, rank;
    vt(int p, int r){
        parent = p;
        rank = r;
    }
};

int ds_find(vector<vt>& ds, int v){
    if(ds[v].parent==-1)
        return v;

    return ds[v].parent = ds_find(ds, ds[v].parent);
}

void ds_union(vector<vt>& ds, int v1, int v2){
    if(ds[v1].rank < ds[v2].rank)
        ds[v1].parent = v2;

    else if(ds[v1].rank > ds[v2].rank)
        ds[v2].parent = v1;

    else{
        ds[v1].parent = v2;
        ds[v2].rank++;
    }
}


bool equationsPossible(vector<string>& equations) {
    int n = equations.size();
    vector<vt> ds (26, vt(-1, 0));             // ---> Since only lowercase letters will be there

    /* In 1st pass --> union all equalities */

    for(string s : equations){

        if(s[1]=='='){
            int v1 = s[0]-'a', v2 = s[3]-'a';

            int root1 = ds_find(ds, v1);
            int root2 = ds_find(ds, v2);

            if(root1!=root2)
                ds_union(ds, root1, root2);
        }
    }

    /* In 2nd pass --> verify all inequalities */

    for(string s : equations){

        if(s[1]=='!'){
            int v1 = s[0]-'a', v2 = s[3]-'a';

            int root1 = ds_find(ds, v1);
            int root2 = ds_find(ds, v2);

            if(root1==root2) return false;
        }
    }

    return true;
}
```

<br>

### 5. Largest Component Size by Common Factor
Given a non-empty array of unique positive integers nums, consider the following graph:
1. There are nums.length nodes, labelled nums[0] to nums[nums.length - 1].
2. There is an edge between nums[i] and nums[j] if and only if nums[i] and nums[j] share a common factor greater than 1.

Return the size of the largest connected component in the graph.

Input: nums = [2,3,6,7,4,12,21,39]

Output: 8

![img](https://assets.leetcode.com/uploads/2018/12/01/ex3.png)

Hint: Union each number with all its factor. Count the most frequent parent.

```cpp
/* --------------------------------- GENERAL UNION FIND TEMPLATE ----------------------------------- */

struct vt{
    int parent, rank;
    vt(int p, int r){
        parent = p;
        rank = r;
    }
};

int ds_find(vector<vt> & ds, int v){
    if(ds[v].parent==-1)
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

/* ------------------------------------------------------------------------------------------------- */

int largestComponentSize(vector<int>& nums) {
    int mx = *max_element(nums.begin(), nums.end());

    vector<vt> ds (mx+1, {-1, 0});

    /* Iterate array nums */

    for(int num : nums){
        int sqr_root = sqrt(num);

        /* For each element, find prime factors (by iterating from 2 to sqrt(num)) */

        for(int i=2; i<=sqr_root; i++){

            /* Do union operation of num with its every prime factor */

            if(num%i==0){

                /* We need to do find operation everytime because root may change anytime */

                int root1 = ds_find(ds, num);
                int root2 = ds_find(ds, i);

                if(root1!=root2)
                    ds_union(ds, root1, root2);

                root1 = ds_find(ds, num); 
                root2 = ds_find(ds, num/i);

                if(root1!=root2)
                    ds_union(ds, root1, root2);
            }
        }
    }

    unordered_map<int, int> mp;     // ----> will contains frequency of roots of each element
    int ans = 1;

    for(int num : nums){
        int root = ds_find(ds, num);
        mp[root]++;

        ans = max(ans, mp[root]);  // ----> Ans will be the max frequency of all roots
    }

    return ans;
}
```

<br>

### 6. Permutation Swaps
Rishabh has a permutation A of N integers 1, 2, ... N but he doesn't like it. Rishabh wants to get a permutation B. Also, Rishabh has some M good pairs given in a form of 2D matrix C of size M x 2  where (C[i][0], C[i][1]) denotes that two indexes of the permutation A. In one operation he can swap Ax and Ay only if (x, y) is a good pair. If the permutation B can be obtained return 1 else return 0.

Input: A = [1, 3, 2, 4], B = [1, 4, 2, 3], C = [[3, 4]]

Output: 0

Hint: Find if two indexes (that we want to swap) belongs to same group or not.

```cpp
struct ds{
    int parent, rank;
    ds(int p, int r){
        parent = p;
        rank = r;
    }
};


/* Simple ds Find operation with path compression */

int ds_find(vector<ds> &v, int vt){
    if(v[vt].parent==-1) return vt;
    return v[vt].parent = ds_find(v, v[vt].parent);
}


/* Simple ds Union operation by rank */

void ds_union(vector<ds> &v, int root1, int root2){

    if(v[root1].rank < v[root2].rank)
        v[root1].parent = root2;

    else if(v[root1].rank > v[root2].rank)
        v[root2].parent = root1;

    else{
        v[root1].parent = root2;
        v[root2].rank++;
    }
}


int solve(vector<int> &A, vector<int> &B, vector<vector<int> > &C) {
    int n = A.size();
    vector<ds> v(n+1, {-1, 0});

    /* We will create an unordered_map for mapping values with their indexes */

    unordered_map<int,int> mp;
    for(int i=0; i<n; i++)
        mp[A[i]] = i;

    /* Iterate one by one on all given pairs and apply Union operation on every pair */

    for(auto e : C){
        int vt1 = e[0]-1, vt2=e[1]-1;

        int root1 = ds_find(v, vt1);
        int root2 = ds_find(v, vt2);

        if(root1!=root2 or (root1==-1 and root2==-1))
            ds_union(v, root1, root2);
    }

    /* 
        After grouping indexes using union Find, 
        find if two indexes (that we want to swap)
        belongs to same group or not.
        If not then return false!
    */

    for(int i=0; i<n; i++){
        if(A[i]!=B[i]){

            int vt1 = i, vt2 = mp[B[i]];

            int root1 = ds_find(v, vt1);
            int root2 = ds_find(v, vt2);

            if(root1!=root2) return false;
        }
    }

    return true;
}
```

<br>

### 7. Journey to the Moon
The member states of the UN are planning to send 2 people to the moon. You will be given a list of pairs of astronaut ID's. Each pair is made of astronauts from the same country. Determine how many pairs of astronauts from different countries they can choose from.

```cpp
struct vt{
    int parent, rank;
    vt (int p, int r){
       parent = p;
       rank = r; 
    }
};

int ds_find (vector<vt> & ds, int v){
    if(ds[v].parent==-1)
        return v;
    return ds[v].parent = ds_find(ds, ds[v].parent);
}

void ds_union (vector<vt> & ds, int v1, int v2){
    if(ds[v1].rank < ds[v2].rank)
        ds[v1].parent = v2;
        
    else if(ds[v1].rank > ds[v2].rank)
        ds[v2].parent = v1;
        
    else{
        ds[v1].parent = v2;
        ds[v2].rank++;
    }
}

int journeyToMoon(int n, vector<vector<int>> astronaut) {
    vector<vt> ds (n, vt (-1, 0));
    
    for(auto e : astronaut){
        int a = e[0], b = e[1];
        int root1 = ds_find(ds, a);
        int root2 = ds_find(ds, b);
        
        if(root1!=root2)
            ds_union(ds, root1, root2);
    }
    
    unordered_map<int,int> mp;
    
    for(int i=0; i<n; i++){
        int root = ds_find(ds, i);
        mp[root]++;
    }
    
    long long total_ways = (n*(n-1))/2;
    long long same_ways = 0;
    
    for(auto p : mp){
        long long m = p.second;
        same_ways += (m*(m-1))/2; 
    }
    
    return total_ways - same_ways;
}
```
