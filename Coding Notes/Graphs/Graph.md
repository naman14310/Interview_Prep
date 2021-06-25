# Basic Graph Problems

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
