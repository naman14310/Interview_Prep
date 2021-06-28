# Dijkstra Algo

Used for finding shortest path (or cheapest) in weightes graph.

## @ Dijkstra on Matrix

#### 1. Path With Minimum Effort
You are given a 2D array of size rows x columns, where heights[row][col] represents the height of cell (row, col). You are situated in the top-left cell, (0, 0), and you hope to travel to the bottom-right cell, (rows-1, columns-1) (i.e., 0-indexed). You can move up, down, left, or right. A route's effort is the maximum absolute difference in heights between two consecutive cells of the route. Return the minimum effort required to travel from the top-left cell to the bottom-right cell.

![img](https://assets.leetcode.com/uploads/2020/10/04/ex1.png)

Input: heights = [[1,2,2],[3,8,2],[5,3,5]]

Output: 2

```cpp
bool isInside(int x, int y, int row, int col){
    return x>=0 and y>=0 and x<row and y<col;
}

int dijkstra (vector<vector<int>>& heights, vector<vector<int>>& cost, int row, int col){
    vector<vector<bool>> vis (row, vector<bool> (col, false));
    
    int dx[] = {1, 0, -1, 0};
    int dy[] = {0, 1, 0, -1};

    set<pair<int, pair<int,int>>> s;      // ----> set <cost, <x,y>> 
    s.insert({0,{0,0}});

    vis[0][0] = true;

    while(!s.empty() and !vis[row-1][col-1]){

        /* Find min element from set */

        auto src = s.begin();                             
        int x = src->second.first, y = src->second.second, c = src->first;

        /* Erase min element from set */

        s.erase(src);                                  

        /* Now Iterate for all 4 nbr of above cell */

        for(int i=0; i<4; i++){
            int xnew = x + dx[i];
            int ynew = y + dy[i];                          

            /* If cell is valid and not vis */

            if(isInside(xnew, ynew, row, col) and !vis[xnew][ynew]){

                /* Find old_cost from cost matrix and compute new_cost by max (c, abs difference of cells) */

                int old_cost = cost[xnew][ynew];
                int new_cost = max(c, abs(heights[xnew][ynew] - heights[x][y]));

                /* If new_cost<old_cost ----> do Relaxation */

                if(new_cost<old_cost){

                    /* update value in cost_matrix */

                    cost[xnew][ynew] = new_cost;

                    /* find that nbr in set */

                    pair<int, pair<int, int>> nbr = {old_cost, {xnew, ynew}};
                    auto ptr = s.find(nbr);

                    /* If nbr exist ----> erase it so we can insert it again with updated values */

                    if(ptr != s.end())
                        s.erase(ptr);

                    /* Insert new or updated nbr into set */

                    s.insert({new_cost, {xnew, ynew}});
                }
            }
        }

        vis[x][y] = true;
    }

    return cost[row-1][col-1];
}


int minimumEffortPath(vector<vector<int>>& heights) {
    int row = heights.size(), col = heights[0].size();

    /* cost matrix is required for finding nbr cell (if exist) in the set */

    vector<vector<int>> cost (row, vector<int> (col, INT_MAX));
    cost[0][0] = 0;

    return dijkstra(heights, cost, row, col);
}
```
