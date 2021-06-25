# Basic Graph Problems

## @ Questions based on Indegree and Outdegree

#### 1. Find the Town Judge
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

#### 2. Minimum Number of Vertices to Reach All Nodes
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
