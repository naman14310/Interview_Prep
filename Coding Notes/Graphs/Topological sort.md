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

Similar Question : Parallel Courses I (Leetcode Premium)

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

#### 3. Alien Dictionary (Tricky)
Given a sorted dictionary of an alien language having N words and k starting alphabets of standard dictionary. Find the order of characters in the alien language.

Input: N = 5, K = 4, dict = {"baa","abcd","abca","cab","cad"}

Output: bdac

```cpp
/* General DFS based topological order template with cycle detection */

bool topological_sort (unordered_map<char, set<char>> & graph, stack<char> & stk, unordered_set<char> & vis, unordered_set<char> & curr_path, char src){
    vis.insert(src);
    curr_path.insert(src);

    for(auto nbr : graph[src]){
        if(curr_path.find(nbr)!=curr_path.end())
            return true;

        if(vis.find(nbr)==vis.end()){
            bool cycle =  topological_sort (graph, stk, vis, curr_path, nbr);
            if(cycle) return true;
        }
    }

    curr_path.erase(src);
    stk.push(src);
    return false;
}


string find_alphabetic_order (unordered_map<char, set<char>> & graph, int K){
    unordered_set<char> vis;
    unordered_set<char> curr_path;
    stack<char> stk;
    string order = "";

    /* Iterate for first K alphabets and do DFS over them */

    for(int i=0; i<K; i++){
        char src = 'a' + i;

        if(vis.find(src)==vis.end()){
            bool cycle = topological_sort (graph, stk, vis, curr_path, src);
            if(cycle) return "";
        }
    }

    while(!stk.empty()){
        order.push_back(stk.top());
        stk.pop();
    }

    return order;
}


string findOrder(string dict[], int N, int K) {
    unordered_map<char, set<char>> graph;

    /*  
        Main gist of this problem is to Create graph on the basis of given list of words

        Iterate for all words till n-1. For every two consecutive words
        Check for the first mismatched charachter from beginning

        Let the mismatched char of first word be u and mismatched char of second word be v
        then insert a directed edge from u to v in the graph.
    */

    for(int i=0; i<N-1; i++){
        for(int j=0; j<min(dict[i].length(), dict[i+1].length()); j++){

            if(dict[i][j] != dict[i+1][j]){
                char u = dict[i][j];
                char v = dict[i+1][j];

                graph[u].insert(v);
                break;
            }
        }
    }

    string order = find_alphabetic_order(graph, K);
    return order;
}
```
