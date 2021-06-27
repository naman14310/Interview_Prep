# BFS

## @ BFS on adjacency list

#### 1. Keys and Rooms
There are N rooms and you start in room 0. A key rooms[i][j] = v opens the room with number v. Return true if and only if you can enter every room.

Input: [[1],[2],[3],[]]

Output: true

```cpp
bool canVisitAllRooms(vector<vector<int>>& rooms) {
    int n = rooms.size();
    vector<bool> vis (n, false);
    queue<int> q;
    int count = 1;

    q.push(0);
    vis[0] = true;

    while(!q.empty()){
        int key = q.front(); q.pop();
        for(int nbr : rooms[key]){
            if(!vis[nbr]){
                q.push(nbr);
                count++;
                vis[nbr] = true;
            }
        }
    }
    return count==n;
}
```

#### 2. Time Needed to Inform All Employees
A company has n employees with a unique ID for each employee from 0 to n - 1. The head of the company is the one with headID.

Each employee has one direct manager given in the manager array, manager[headID] = -1. Also, it is guaranteed that the subordination relationships have a tree structure.

The head of the company wants to inform all the company employees of an urgent piece of news. He will inform his direct subordinates, and they will inform their subordinates, and so on until all employees know about the urgent news.

The i-th employee needs informTime[i] minutes to inform all of his direct subordinates (i.e., After informTime[i] minutes, all his direct subordinates can start spreading the news). Return the number of minutes needed to inform all the employees about the urgent news.

Input: n = 4, headID = 2, manager = [3,3,-1,2], informTime = [0,0,162,914]

Output: 1076

```cpp
int numOfMinutes(int n, int headID, vector<int>& manager, vector<int>& informTime) {
    unordered_map<int, vector<int>> graph;
    int time = informTime[headID];

    for(int i=0; i<n; i++)
        graph[manager[i]].push_back(i);

    queue<pair<int,int>> q;
    q.push({headID, time});

    while(!q.empty()){
        auto emp = q.front(); q.pop();
        int id = emp.first, t = emp.second;

        for(int nbr : graph[id]){
            q.push({nbr, t+informTime[nbr]});
            time = max(time, t+informTime[nbr]);
        }
    }

    return time;
}
```

## @ BFS on matrix

#### 1. Flood Fill
You are given an image and three integers sr, sc, and newColor. You should perform a flood fill on the image starting from the pixel image[sr][sc]. To perform a flood fill, consider the starting pixel, plus any pixels connected 4-directionally to the starting pixel of the same color as the starting pixel, plus any pixels connected 4-directionally to those pixels (also with the same color), and so on. Replace the color of all of the aforementioned pixels with newColor.

![img](https://assets.leetcode.com/uploads/2021/06/01/flood1-grid.jpg)

```cpp
bool isInside(int x, int y, int row, int col){
    return x>=0 and y>=0 and x<row and y<col;
}

vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
    queue<pair<int,int>> q;
    int row = image.size(), col = image[0].size();
    vector<vector<bool>> vis (row, vector<bool>(col, false));
    int color = image[sr][sc];

    int dx[] = {1, 0, -1, 0};
    int dy[] = {0, 1, 0, -1};

    q.push({sr, sc});
    image[sr][sc] = newColor;
    vis[sr][sc] = true;

    while(!q.empty()){
        auto p = q.front(); q.pop();

        for(int i=0; i<4; i++){
            int x = p.first + dx[i];
            int y = p.second + dy[i];

            if(isInside(x, y, row, col) and !vis[x][y] and image[x][y]==color){
                q.push({x,y});
                image[x][y] = newColor;
                vis[x][y] = true;
            }
        }
    }
    return image;
}
```
