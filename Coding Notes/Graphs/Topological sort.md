# Topological Sort

It is a linear ordering of vertices such that for every directed edge u --> v, vertex u comes before vertex v in the ordering.

#### 1. Course Schedule II
There are a total of numCourses courses labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai. Return the ordering of courses you should take to finish all courses. If it is impossible to finish all courses, return an empty array.

**Method 1: DFS + Stack**
```cpp
bool dfs (unordered_map<int, vector<int>>& graph, vector<bool>& vis, vector<bool>& curr_path, stack<int>& stk, int src){
    vis[src] = true;
    curr_path[src] = true;

    for(int nbr : graph[src]){

        if(curr_path[nbr]) return false;

        if(!vis[nbr]){
            bool res = dfs(graph, vis, curr_path, stk, nbr);
            if(!res) return false;
        }
    }

    curr_path[src] = false;
    stk.push(src);
    return true;
}

vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
    unordered_map<int, vector<int>> graph;
    vector<bool> vis (numCourses, false);
    vector<bool> curr_path (numCourses, false);

    stack<int> stk;
    vector<int> res;

    for(auto e : prerequisites)
        graph[e[1]].push_back(e[0]);        // ----> create edges u --> v such that u comes before v 

    for(int i=0; i<numCourses; i++){
        if(!vis[i]){
            bool order_exist = dfs(graph, vis, curr_path, stk, i);
            if(!order_exist) return res;
        }
    }

    while(!stk.empty()){
        res.push_back(stk.top());
        stk.pop();
    }

    return res;
}
```

**Method 2: BFS + indegre array (Kahns Algo)**

Approach: 
1. Push all vertices with indegre 0 to queue
2. Iterate till queue becomes empty
3. Pop one element from queues --> Relax all its nbrs (decrement all its nbr's indegree by 1) --> and add that popped element to order vector.
4. While doing relaxing, If indegre of any nbr becomes 0, push it in the queue

```cpp
int bfs (unordered_map<int, vector<int>>& graph, vector<int>& indegre, queue<int>& q, vector<int>& order){
    int vis_count = 0;          // ---> It keeps track for processed vertices

    while(!q.empty()){
        int node = q.front(); q.pop();

        for(int nbr : graph[node]){
            indegre[nbr]--;
            if(indegre[nbr]==0)
                q.push(nbr);
        }

        order.push_back(node);
        vis_count++;
    }

    return vis_count;
}

vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
    unordered_map<int, vector<int>> graph;
    vector<int> indegre (numCourses, 0);
    vector<int> order;

    for(auto e : prerequisites){
        graph[e[1]].push_back(e[0]);   // ----> creating edge  u-->v
        indegre[e[0]]++;               // ----> update indegre for v
    }

    queue<int> q;

    for(int i=0; i<numCourses; i++)
        if(indegre[i]==0)
            q.push(i);

    int vis_count = bfs (graph, indegre, q, order);

    if(vis_count<numCourses)
        return vector<int>();

    return order;
}
```

#### 2. Minimum time taken by each job to be completed given by a Directed Acyclic Graph

![img](https://media.geeksforgeeks.org/wp-content/uploads/20200804212533/Semester1.png)

Output: 6

Hint: Use the bfs level logic in topological sort (Use kahns algo)

```cpp
int topological_sort (unordered_map<int, vector<int>> & graph, vector<int> & indegre){
    int time = 1;
    queue<int> q;

    /* Filling queue initially with vertices having indegre zero */

    for(int i=0; i<indegre.size(); i++){
        if(indegre[i]==0)
            q.push(i);
    }

    q.push(-1);    // ----> will represent the end of one topological order level

    while(!q.empty()){
        int v = q.front(); q.pop();

        if(v==-1){
            if(q.empty()) break;

            q.push(-1);
            time++;
            continue;
        }

        for(int nbr : graph[v]){
            indegre[nbr]--;
            if(indegre[nbr]==0) q.push(nbr);
        }
    }

    return time;
}


int minTimeTakenByJob (unordered_map<int, vector<int>> & graph, int total_jobs){
    vector<int> indegre (total_jobs+1, 0);

    for(auto p : graph){
        for(int v : p.second)
            indegre[v]++;
    }

    int time = topological_sort (graph, indegre);
    return time;
}
```
