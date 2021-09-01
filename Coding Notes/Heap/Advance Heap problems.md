# Some Really Hard Problems on Heap

<br>

### 1. Trapping Rain Water II (Damn Hard!)
Given an m x n integer matrix heightMap representing the height of each unit cell in a 2D elevation map, return the volume of water it can trap after raining.

```
Input: 

heightMap = 
[[1,4,3,1,3,2],
[3,2,1,3,2,4],
[2,3,3,2,3,1]]

Output: 4
```

![img](https://assets.leetcode.com/uploads/2021/04/08/trap1-3d.jpg)

[Explaination with Visualization](https://leetcode.com/problems/trapping-rain-water-ii/discuss/1138028/Python3Visualization-BFS-Solution-With-Explanation)

**Approach**
1. Create a minheap of cell (height, x, y). Also create one vis matrix to keep track of already visited cells.
2. Initially insert all four boundary cells to min heap.
3. Run a loop till minheap becomes empty.
4. Pop cell of min height
5. Update level to max(level, curr_cell height)
6. For all 4 nbrs of curr_cell, if any nbr is not vis then :
7. If height of nbr is less then level --> water += level - height of nbr
8. Push unvisited nbrs to minheap and mark them vis.

```cpp
struct cell{
    int height, x, y;
    cell(int h, int x, int y){
        height = h;
        this->x = x;
        this->y = y;
    }
};


struct comp{

    /* REMEMBER : use opp sign in customized comparators of heap and set | Here we use '>' for minheap */

    bool operator()(cell c1, cell c2){
        return c1.height > c2.height;
    }  
};


bool isInside(int x, int y, int row, int col){
    return x>=0 and y>=0 and x<row and y<col;
}


int trapRainWater(vector<vector<int>>& heightMap) {
    int row = heightMap.size(), col = heightMap[0].size();

    priority_queue<cell, vector<cell>, comp> minheap;               // --> minheap of cell (height, x, y)
    vector<vector<bool>> vis (row, vector<bool> (col, false));      // --> vis matrix
    

    /* Initially insert all four boundary cells to min heap */

    for(int j=0; j<col; j++){
        minheap.push(cell(heightMap[0][j], 0, j));
        vis[0][j] = true;

        minheap.push(cell(heightMap[row-1][j], row-1, j));
        vis[row-1][j] = true;
    }

    for(int i=1; i<row-1; i++){
        minheap.push(cell(heightMap[i][0], i, 0));
        vis[i][0] = true;

        minheap.push(cell(heightMap[i][col-1], i, col-1));
        vis[i][col-1] = true;
    }

    /* ------------------------------------------------------ */

    int water = 0;
    int level = 0;      // --> it will store the max level upto water can be filled in curr cell 

    int dx[] = {0, 1, 0, -1};
    int dy[] = {1, 0, -1, 0};

    /* Iterate till heap becomes empty and pop cell with min height one by one */

    while(!minheap.empty()){
        cell c = minheap.top(); minheap.pop();
        level = max(level, c.height);               // --> update level after popping each element

        for(int i=0; i<4; i++){
            int xnew = c.x + dx[i];
            int ynew = c.y + dy[i];

            /* If nbr cell is inside and not vis and its height is less then level then water can be trapped here */

            if(isInside(xnew, ynew, row, col) and !vis[xnew][ynew]){
                if(heightMap[xnew][ynew]<level)
                    water += level - heightMap[xnew][ynew];

                /* Push nbr to the minheap and mark it as vis */

                minheap.push(cell(heightMap[xnew][ynew], xnew, ynew));
                vis[xnew][ynew] = true;
            }
        }
    }

    return water;
}
```

<br>

### 2. The Skyline Problem

![img](https://assets.leetcode.com/uploads/2020/12/01/merged.jpg)

Input: buildings = [[2,9,10],[3,7,15],[5,12,12],[15,20,10],[19,24,8]]

Output: [[2,10],[3,15],[7,12],[12,0],[15,10],[20,8],[24,0]]

1. Figure A shows the buildings of the input.
2. Figure B shows the skyline formed by those buildings. The red points in figure B represent the key points in the output list.

[Video Explaination](https://www.youtube.com/watch?v=GSBLe8cKu0s)

**Approach**
1. Create a vector of points for each building.
2. Sort that vector on the basis of pt.dist. If dist for two or more points are same then sort them on the basis of following three clashes.
3. Iterate for all points and do insert/erase in multiset, according to the point type. 
4. If curr_max != prev_max (before insertion/deletion) then push {pt.dist, curr_max} to res. 

```cpp
/*                  3 Boundary Cases   
                    ----------------

   start-start          end-end            start-end
     clash               clash               clash 

    x______             x______           x_____
    |      |            |      |          |     |                   x --> represents the
    |      |            |   ___|          |     x_____                    skyline points  
    |____  |            |  |   |          |     |     |
    |    | |            |  |   |          |     |     |
    |    | |            |  |   |          |     |     |
           x                   x                      x

  push point with     push point with     push start before 
  larger height       smaller height      before then end  
  before smaller      before larger       point  

*/


struct point{
    int dist, height, type;  
    point(int d, int h, int t){
        dist = d;
        height = h;
        type = t;
    }
};


static bool comp (point p1, point p2){

    if(p1.dist < p2.dist) return true;          // --> no clash

    else if(p1.dist==p2.dist){

        /* start-start clash */

        if(p1.type==0 and p2.type==0)
            return p1.height > p2.height;

        /* end-end clash */

        else if(p1.type==1 and p2.type==1)
            return p1.height < p2.height;       

        /* start-end clash */

        else return p1.type == 0;
    }

    else return false;
}  


vector<vector<int>> getSkyline(vector<vector<int>>& buildings) {
    vector<vector<int>> res;
    vector<point> p;

    /* Here we need to perform removal operations hence we are using multiset instead of priority_queue */

    multiset<int> s;        // --> we will insert p.heights in multiset (insert 0 initially)
    s.insert(0);

    for(auto b : buildings){
        p.push_back(point(b[0], b[2], 0));       // --> start of building b (startDist, height, 0 denotes start)
        p.push_back(point(b[1], b[2], 1));       // --> end of building b (endDist, height, 1 denotes end)
    }

    sort (p.begin(), p.end(), comp);

    for(auto pt : p){
        int prev_max = *s.rbegin();

        if(pt.type==0)
            s.insert(pt.height);
        else
            s.erase(s.find(pt.height));         // --> This will erase only one instance from multiset 
                                                // --> (Simple erase will remove all instances)

        int curr_max = *s.rbegin();

        if(curr_max!=prev_max)
            res.push_back({pt.dist, curr_max});
    }

    return res;
}
```

<br>

### 3. Sliding Window Median
You are given an integer array nums and an integer k. There is a sliding window of size k which is moving from the very left of the array to the very right.Each time the sliding window moves right by one position. Return the median array for each window in the original array.

```
Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [1.00000,-1.00000,-1.00000,3.00000,5.00000,6.00000]

Explanation: 
Window position                Median
---------------                -----
[1  3  -1] -3  5  3  6  7        1
 1 [3  -1  -3] 5  3  6  7       -1
 1  3 [-1  -3  5] 3  6  7       -1
 1  3  -1 [-3  5  3] 6  7        3
 1  3  -1  -3 [5  3  6] 7        5
 1  3  -1  -3  5 [3  6  7]       6
```

Hint: Use median of data stream concept and use two heaps to store running median. 

```cpp
void balance_heaps (priority_queue<int> & maxheap, priority_queue<int, vector<int>, greater<int>> & minheap){

    if(maxheap.size() > minheap.size()+1){
        minheap.push(maxheap.top());
        maxheap.pop();
    }
    else if(minheap.size() > maxheap.size()+1){
        maxheap.push(minheap.top());
        minheap.pop(); 
    }
}


void insert (priority_queue<int> & maxheap, priority_queue<int, vector<int>, greater<int>> & minheap, int val){

    if(maxheap.empty() and minheap.empty())
        maxheap.push(val);

    else if(maxheap.empty()) 
        minheap.push(val);

    else if(minheap.empty()) 
        maxheap.push(val);

    else if(val<=maxheap.top())
        maxheap.push(val);
        
    else
        minheap.push(val);

    balance_heaps (maxheap, minheap);
}


void remove (priority_queue<int> & maxheap, priority_queue<int, vector<int>, greater<int>> & minheap, int val){

    if(!maxheap.empty() and val<=maxheap.top()){
        vector<int> temp;
        while(!maxheap.empty()){

            if(maxheap.top()==val){
                maxheap.pop();
                break;
            }

            temp.push_back(maxheap.top());
            maxheap.pop();
        }

        for(int t : temp) 
            maxheap.push(t);
    }
    else{
        vector<int> temp;
        while(!minheap.empty()){

            if(minheap.top()==val){
                minheap.pop();
                break;
            }

            temp.push_back(minheap.top());
            minheap.pop();
        }

        for(int t : temp) 
            minheap.push(t);
    }

    balance_heaps (maxheap, minheap);
}


double get_median (priority_queue<int> & maxheap, priority_queue<int, vector<int>, greater<int>> & minheap){
    if (maxheap.size() > minheap.size()) return maxheap.top();
    else if (maxheap.size() < minheap.size()) return minheap.top();
    else{
        long long a = maxheap.top();
        long long b = minheap.top();
        return (a+b)/2.0;
    } 
}


vector<double> medianSlidingWindow(vector<int>& nums, int k) {

    vector<double> res;
    priority_queue<int> maxheap;
    priority_queue<int, vector<int>, greater<int>> minheap;

    int i=0;

    for( ; i<k; i++)
        insert(maxheap, minheap, nums[i]);

    res.push_back(get_median(maxheap, minheap));

    for( ; i<nums.size(); i++){
        remove(maxheap, minheap, nums[i-k]);
        insert(maxheap, minheap, nums[i]);
        res.push_back(get_median(maxheap, minheap));
    }

    return res;
}
```

