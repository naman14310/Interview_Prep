# Euler Path

#### 1. Reconstruct Itinerary
You are given a list of airline tickets where tickets[i] = [fromi, toi]. Reconstruct the itinerary in order and return it. The itinerary must begin with "JFK". If there are multiple valid itineraries, you should return  the smallest lexical order when read as a single string. For example, the itinerary ["JFK", "LGA"] has a smaller lexical order than ["JFK", "LGB"]. You may assume all tickets form at least one valid itinerary. You must use all the tickets once and only once.

![img](https://assets.leetcode.com/uploads/2021/03/14/itinerary2-graph.jpg)

Input: tickets = [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]

Output: ["JFK","ATL","JFK","SFO","ATL","SFO"]

```cpp
void euler_path (unordered_map<string, multiset<string>>& graph, stack<string>& stk, string src, vector<string>& res){

    /* Push src node in stack */

    stk.push(src);

    /* Iterate till stack becomes empty */

    while(!stk.empty()){

        /* Pop one node from stack */

        string airport = stk.top();

        /* If node has no neighbours then we will push it in our answer vector */

        if(graph[airport].empty()){
            res.push_back(stk.top());
            stk.pop();
            continue;
        }

        /* Else take out the first node from nbrs multiset, push it in the stack and erase it */

        string lex_smallest = *graph[airport].begin();
        stk.push(lex_smallest);

        graph[airport].erase(graph[airport].begin());
    }
}

vector<string> findItinerary(vector<vector<string>>& tickets) {
    vector<string> res;

    /* We will use multiset to keep values in sorted order (values can be repeating) */

    unordered_map<string, multiset<string>> graph;
    stack<string> stk;

    for(auto e : tickets)
        graph[e[0]].insert(e[1]);

    euler_path (graph, stk, "JFK", res);

    /* reverse the array to get correct order*/

    reverse(res.begin(), res.end());

    return res;
}
```
