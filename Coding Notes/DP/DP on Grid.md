# Problems on DP on GRID

### 1. Range Sum Query 2D (Prefix Sum 2D)
Given a 2D matrix matrix, handle multiple queries of Calculating the sum of the elements of matrix inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).

![img](https://assets.leetcode.com/uploads/2021/03/14/sum-grid.jpg)

Hint: Sum of any rectangle box = grid[row2][col2] - left_box_sum - top_box_sum + top_left_diag_box

```cpp
class NumMatrix {
public:
    
    vector<vector<int>> grid;
    
    NumMatrix(vector<vector<int>>& matrix) {
        int row = matrix.size(), col = matrix[0].size();
        grid = vector<vector<int>> (row, vector<int> (col, 0));
        
        /* ---------------- Filling 2D Prefix Sum Grid ----------------- */
        
        for(int i=0; i<row; i++){
            for(int j=0; j<col; j++){
                
                if(i==0 and j==0)
                    grid[i][j] = matrix[i][j];
                
                else if(i==0)
                    grid[i][j] = matrix[i][j] + grid[i][j-1];
                
                else if(j==0)
                    grid[i][j] = matrix[i][j] + grid[i-1][j];
                
                else
                    grid[i][j] = matrix[i][j] + grid[i-1][j] + grid[i][j-1] - grid[i-1][j-1];
            }
        }
    }
    
    int sumRegion(int row1, int col1, int row2, int col2) {
        
        int left_box_sum = col1>0 ? grid[row2][col1-1] : 0;
        int top_box_sum = row1>0 ? grid[row1-1][col2] : 0;
        int top_left_diag_box = row1>0 and col1>0 ? grid[row1-1][col1-1] : 0;
        
        return grid[row2][col2] - left_box_sum - top_box_sum + top_left_diag_box;
    }
};
```

### 2. Maximal Square
Given an m x n binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.

![img](https://assets.leetcode.com/uploads/2020/11/26/max1grid.jpg)

Output: 4

```cpp
int maximalSquare(vector<vector<char>>& matrix) {
    int row = matrix.size(), col = matrix[0].size();
    vector<vector<int>> dp (row, vector<int> (col, 0));

    int ans = 0; 

    for(int i=0; i<row; i++){
        for(int j=0; j<col; j++){

            if(matrix[i][j]=='0') continue;

            if(i==0 or j==0)
                dp[i][j] = 1;

            else
                dp[i][j] = min({dp[i-1][j], dp[i][j-1], dp[i-1][j-1]}) + 1;

            ans = max(ans, dp[i][j]);
        }
    }

    return ans*ans;
}
```

### 3. Count Square Submatrices with All Ones
Given a m * n matrix of ones and zeros, return how many square submatrices have all ones.

Time Complexity : O(n2)

```
Input: matrix =
[
  [0,1,1,1],
  [1,1,1,1],
  [0,1,1,1]
]

Output: 15
```

```cpp
int countSquares(vector<vector<int>>& matrix) {
    int row = matrix.size(), col = matrix[0].size();
    int total_sqr_cnt = 0;

    /* Iterate for every row and col except starting from 1 */

    for(int i=1; i<row; i++){
        for(int j=1; j<col; j++){

            /* if matrix[i][j]==1 update it with min(top, left, top-left) + 1 */

            if(matrix[i][j]==1)
                matrix[i][j] = min(min(matrix[i-1][j], matrix[i][j-1]), matrix[i-1][j-1]) + 1;
        }
    }

    /* Compute sum of the whole matrix and that will be our answer */
    /* becoz number inside every cell indicates the cnt of sqrs that can be drawn by keeping that cell at bottom right */

    for(int i=0; i<row; i++){
        for(int j=0; j<col; j++){

            total_sqr_cnt += matrix[i][j];
        }
    }

    return total_sqr_cnt;
}
```

### 4. Count Submatrices With All Ones (Rectangles are also allowed)
Given a rows * columns matrix mat of ones and zeros, return how many submatrices have all ones.

Time Complexity : O(n3)

```
Input: mat = 
[
    [0,1,1,0],
    [0,1,1,1],
    [1,1,1,0]
]

Output: 24
```

```cpp
int count_mat_start_from_cell (vector<vector<int>>& mat, vector<vector<int>>& rightOneCnt, int i, int j, int & row, int & col){
    int cnt = 0;
    int mn = col;

    while(i<row){
        mn = min (mn, rightOneCnt[i][j]);
        cnt += mn; 
        i++;
    }

    return cnt;
}


int numSubmat(vector<vector<int>>& mat) {
    int row = mat.size(), col = mat[0].size();
    vector<vector<int>> rightOneCnt (row, vector<int> (col, 0));

    for(int i=0; i<row; i++){
        for(int j=col-1; j>=0; j--){

            if(mat[i][j]==0) continue;

            if(j==col-1)
                rightOneCnt[i][j] = 1;
            else
                rightOneCnt[i][j] = rightOneCnt[i][j+1]+1;
        }
    }

    int total_submatrices = 0;

    /* Iterate over mat and count all matrices starting from cell i,j one by one using above function */

    for(int i=0; i<row; i++){
        for(int j=0; j<col; j++){
            if(mat[i][j]==1)
                total_submatrices += count_mat_start_from_cell (mat, rightOneCnt, i, j, row, col);
        }
    }

    return total_submatrices;
}
```

### 5. Largest 1-Bordered Square (Tricky)
Given a 2D grid of 0s and 1s, return the number of elements in the largest square subgrid that has all 1s on its border, or 0 if such a subgrid doesn't exist in the grid.
```
Input: grid = 
[
    [1,1,1],
    [1,0,1],
    [1,1,1]
]

Output: 9
```

Hint: Store left_one_cnt and up_one_cnt for every cell.

```cpp
int largest1BorderedSquare(vector<vector<int>>& grid) {
    int row = grid.size(), col = grid[0].size();
    int ans = 0;

    /* Every cell of dp table contains pair of {count_of_1_in_up, count_of _1_in_left} */

    vector<vector<pair<int,int>>> dp (row, vector<pair<int,int>> (col, {0,0}));

    for(int i=0; i<row; i++){
        for(int j=0; j<col; j++){

            if(grid[i][j]==0)
                continue;

            if(i==0 and j==0)
                dp[0][0] = {1, 1};

            else if(i==0)
                dp[i][j] = {1, dp[i][j-1].second+1};

            else if(j==0)
                dp[i][j] = {dp[i-1][j].first+1, 1};

            else
                dp[i][j] = {dp[i-1][j].first+1, dp[i][j-1].second+1};

            /* Treating cell i-j as bottom-right corner of square, check if it can form any sqr */

            int mn = min(dp[i][j].first, dp[i][j].second);

            while(mn>0){
                int x = i-mn+1, y = j-mn+1;

                if(dp[i][y].first >= mn and dp[x][j].second >= mn){
                    ans = max(ans, mn);
                    break;
                }

                mn--;
            }
        }
    }

    return ans*ans;
}
```

### 6.  Largest Plus Sign
Return the order of the largest axis-aligned plus sign of 1's contained in grid. If there is none, return 0.

![img](https://assets.leetcode.com/uploads/2021/06/13/plus1-grid.jpg)

Output: 2

```cpp
int orderOfLargestPlusSign(int n, vector<vector<int>>& mines) {
    if(mines.size()==n*n) return 0;

    vector<vector<int>> grid(n, vector<int> (n, 1));
    vector<vector<int>> up (n, vector<int> (n, 0));
    vector<vector<int>> down (n, vector<int> (n, 0));
    vector<vector<int>> left (n, vector<int> (n, 0));
    vector<vector<int>> right (n, vector<int> (n, 0));

    for(auto p : mines)
        grid[p[0]][p[1]] = 0;

    /* Filling up matrix */

    for(int i=0; i<n; i++){
        for(int j=0; j<n; j++){
            if(i==0)
                up[i][j] = grid[i][j];
            else
                up[i][j] = grid[i][j]==0 ? 0 : 1 + up[i-1][j];
        }
    }

    /* Filling down matrix */

    for(int i=n-1; i>=0; i--){
        for(int j=0; j<n; j++){
            if(i==n-1)
                down[i][j] = grid[i][j];
            else
                down[i][j] = grid[i][j]==0 ? 0 : 1 + down[i+1][j];
        }
    }

    /* Filling left matrix */

    for(int i=0; i<n; i++){
        for(int j=0; j<n; j++){
            if(j==0)
                left[i][j] = grid[i][j];
            else
                left[i][j] = grid[i][j]==0 ? 0 : 1 + left[i][j-1];
        }
    }

    /* Filling right matrix */

    for(int i=0; i<n; i++){
        for(int j=n-1; j>=0; j--){
            if(j==n-1)
                right[i][j] = grid[i][j];
            else
                right[i][j] = grid[i][j]==0 ? 0 : 1 + right[i][j+1];
        }
    }

    int ans = 1;

    for(int i=1; i<n-1; i++){
        for(int j=1; j<n-1; j++){
            int len = min({up[i][j], down[i][j], left[i][j], right[i][j]});
            ans = max(ans, len);
        }
    }

    return ans;
}
```


## @ Problems on Exploring Paths 

**How To Identify :** When there are chances to land on one cell repeatedly while exploring paths, then we can use DP. 


### 1. Minimum Path Sum
Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path. You can only move either down or right at any point in time.

![img](https://assets.leetcode.com/uploads/2020/11/05/minpath.jpg)

```cpp
bool isInside(int x, int y, int row, int col){
    return x<row and y<col;
}


int solve (vector<vector<int>> & grid, int x, int y, int row, int col, vector<vector<int>> & dp){
    if(x==row-1 and y==col-1) return grid[x][y]; 

    if(dp[x][y]!=-1) return dp[x][y]; 

    int ans = INT_MAX;

    int dx[] = {1, 0};
    int dy[] = {0, 1};

    for(int i=0; i<2; i++){
        int xnew = x + dx[i];
        int ynew = y + dy[i];

        if(isInside(xnew, ynew, row, col))
            ans = min(ans, solve(grid, xnew, ynew, row, col, dp));
    }

    ans += grid[x][y];
    return dp[x][y] = ans;
}


int minPathSum(vector<vector<int>>& grid) {
    int row = grid.size(), col = grid[0].size();
    vector<vector<int>> dp (row, vector<int> (col, -1));

    return solve (grid, 0, 0, row, col, dp);
}
```


### 2. Minimum Falling Path Sum
Given an n x n array of integers matrix, return the minimum sum of any falling path through matrix. A falling path starts at any element in the first row and chooses the element in the next row that is either directly below or diagonally left/right. 

Input: matrix = [[2,1,3],[6,5,4],[7,8,9]]

Output: 13

```cpp
bool isInside (int y, int col){
    return y>=0 and y<col;
}

int solve (vector<vector<int>> & matrix, vector<vector<int>> & dp, int x, int y, int row, int col){
    if(x==row-1) return matrix[x][y];

    if(dp[x][y]!=-1) return dp[x][y];

    int ans = INT_MAX;
    int dy[] = {-1, 0, 1};

    for(int i=0; i<3; i++){
        int xnew = x+1;
        int ynew = y+dy[i];

        if(isInside(ynew, col)){
            int temp = solve(matrix, dp, xnew, ynew, row, col);
            ans = min(ans, temp);
        }
    }

    ans += matrix[x][y];
    return dp[x][y] = ans;
}


int minFallingPathSum(vector<vector<int>>& matrix) {
    int row = matrix.size(), col = matrix[0].size();
    vector<vector<int>> dp (row, vector<int> (col, -1));

    int ans = INT_MAX;

    for(int i=0; i<col; i++)
        ans = min(ans, solve(matrix, dp, 0, i, row, col));

    return ans;
}
```

### 3. Maximum Number of Points with Cost
You are given an m x n integer matrix points (0-indexed). Starting with 0 points, you want to maximize the number of points you can get from the matrix. To gain points, you must pick one cell in each row. Picking the cell at coordinates (r, c) will add points[r][c] to your score. For every two adjacent rows r and r + 1 (where 0 <= r < m - 1), picking cells at coordinates (r, c1) and (r + 1, c2) will subtract abs(c1 - c2) from your score. Return the maximum number of points you can achieve.

![img](https://assets.leetcode.com/uploads/2021/07/12/screenshot-2021-07-12-at-13-40-26-diagram-drawio-diagrams-net.png)

Input: points = [[1,2,3],[1,5,1],[3,1,1]]

Output: 9

```cpp
long long solve (vector<vector<int>>& points, int r, int prev_c, int row, int col, vector<vector<int>> & dp){
    if(r==row) return 0;

    if(r>0 and dp[r][prev_c]!=-1) return dp[r][prev_c];

    long long ans = 0;

    for(int i=0; i<col; i++){
        long long temp = points[r][i] + solve (points, r+1, i, row, col, dp);
        if(r>0) temp -= abs(prev_c - i);

        ans = max(ans, temp);
    }

    return dp[r][prev_c] = ans;
}


long long maxPoints(vector<vector<int>>& points) {
    int row = points.size(), col = points[0].size();
    vector<vector<int>> dp (row, vector<int> (col, -1));

    return solve (points, 0, 0, row, col, dp);
}
```

### 4. Triangle
Given a triangle array, return the minimum path sum from top to bottom. For each step, you may move to an adjacent number of the row below. More formally, if you are on index i on the current row, you may move to either index i or index i + 1 on the next row.

```
Input: triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
Output: 11

Explanation: The triangle looks like:
   2
  3 4
 6 5 7
4 1 8 3
```

```cpp
int solve (vector<vector<int>> & triangle, int i, int j, int n, vector<vector<int>> & dp){
    if(i==n) return 0;

    if(dp[i][j]!=-1) return dp[i][j];

    int ans = 0;

    int res1 = solve(triangle, i+1, j, n, dp);
    int res2 = solve(triangle, i+1, j+1, n, dp);

    ans = min(res1, res2) + triangle[i][j];
    return dp[i][j] = ans;
}


int minimumTotal(vector<vector<int>>& triangle) {
    int n = triangle.size();
    vector<vector<int>> dp (n+1, vector<int> (n+1, -1));

    return solve (triangle, 0, 0, n, dp);
}
```

### 5. Unique Paths
A robot is located at the top-left corner of a m x n grid. The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid. How many possible unique paths are there?

```cpp
int uniquePaths(int m, int n) {
    vector<vector<int>> grid (m, vector<int> (n, 0));

    for(int i=0; i<m; i++){
        for(int j=0; j<n; j++){

            if(i==0 or j==0)
                grid[i][j] = 1;

            else
                grid[i][j] = grid[i-1][j] + grid[i][j-1];
        }
    }

    return grid[m-1][n-1];
}
```

### 6. Unique Paths II (with obstacles)

![img](https://assets.leetcode.com/uploads/2020/11/04/robot1.jpg)

Input: obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]

Output: 2

```cpp
int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
    int row = obstacleGrid.size(), col = obstacleGrid[0].size();
    
    if(obstacleGrid[0][0]==1 or obstacleGrid[row-1][col-1]==1) 
        return 0;

    vector<vector<int>> dp (row, vector<int> (col, 0));

    for(int i=0; i<row; i++){
        for(int j=0; j<col; j++){

            if(obstacleGrid[i][j]==1) continue;

            else if(i==0 and j==0)
                dp[i][j] = 1;

            else if(i==0)
                dp[i][j] = dp[i][j-1];

            else if(j==0)
                dp[i][j] = dp[i-1][j];

            else
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
        }
    }

    return dp[row-1][col-1];
}
```


### 7. Out of Boundary Paths
There is an m x n grid with a ball. The ball is initially at the position [startRow, startColumn]. You are allowed to move the ball to one of the four adjacent cells in the grid (possibly out of the grid crossing the grid boundary). You can apply at most maxMove moves to the ball. Given the five integers m, n, maxMove, startRow, startColumn, return the number of paths to move the ball out of the grid boundary. Since the answer can be very large, return it modulo 109 + 7.

![img](https://assets.leetcode.com/uploads/2021/04/28/out_of_boundary_paths_1.png)

Input: m = 2, n = 2, maxMove = 2, startRow = 0, startColumn = 0

Output: 6

```cpp
bool isOut(int x, int y, int m, int n){
    return x<0 or y<0 or x>=m or y>=n;    
}


int dfs (int x, int y, int m, int n, int moves, vector<vector<vector<int>>> & dp){

    if(isOut(x, y, m, n)) return 1;
    if(moves==0) return 0;

    if(dp[x][y][moves]!=-1) return dp[x][y][moves];

    int dx[] = {1, 0, -1, 0};
    int dy[] = {0, 1, 0, -1};

    int ans = 0;

    for(int i=0; i<4; i++){
        int xnew = x + dx[i];
        int ynew = y + dy[i];

        ans = ((ans % 1000000007) + (dfs (xnew, ynew, m, n, moves-1, dp) % 1000000007)) % 1000000007;
    }

    return dp[x][y][moves] = ans;
}


int findPaths(int m, int n, int maxMove, int startRow, int startColumn) {

    vector<vector<vector<int>>> dp (m+1, vector<vector<int>> (n+1, vector<int> (maxMove+1, -1)));
    return dfs (startRow, startColumn, m, n, maxMove, dp);
}
```

### 8. Cherry Pickup II
Given a rows x cols matrix grid representing a field of cherries. Each cell in grid represents the number of cherries that you can collect. You have two robots that can collect cherries for you, Robot #1 is located at the top-left corner (0,0) , and Robot #2 is located at the top-right corner (0, cols-1) of the grid.

Return the maximum number of cherries collection using both robots  by following the rules below:
1. From a cell (i,j), robots can move to cell (i+1, j-1) , (i+1, j) or (i+1, j+1).
2. When any robot is passing through a cell, It picks it up all cherries, and the cell becomes an empty cell (0).
3. When both robots stay on the same cell, only one of them takes the cherries.
4. Both robots cannot move outside of the grid at any moment.
5. Both robots should reach the bottom row in the grid.

![img](https://assets.leetcode.com/uploads/2020/04/29/sample_1_1802.png)

```cpp
bool isInside(int y, int col){
    return y>=0 and y<col;
}

/* 
    x   --> x pos of both robots will be same
    y1  --> y pos of robot 1
    y2  --> y pos of robot 2

*/

int solve (vector<vector<int>>& grid, int x, int y1, int y2, int row, int col, vector<vector<vector<int>>> & dp ){

    if(x==row-1){
        int ans = grid[x][y1] + grid[x][y2];
        if(y1==y2) ans -= grid[x][y1];
        return ans;
    }

    if(dp[x][y1][y2]!=-1) return dp[x][y1][y2];

    int ans = 0;
    int dy[] = {-1, 0, 1};

    for(int i=0; i<3; i++){
        int yn1 = y1 + dy[i]; 
        if(!isInside(yn1, col)) continue;

        for(int j=0; j<3; j++){
            int yn2 = y2 + dy[j]; 
            if(!isInside(yn2, col)) continue;

            int res = solve (grid, x+1, yn1, yn2, row, col, dp);
            ans = max(ans, res);
        }
    }

    ans += grid[x][y1] + grid[x][y2];
    if(y1==y2) ans -= grid[x][y1];      // --> If both robots comes to the same cell, then remove the extra overlapped cherry

    return dp[x][y1][y2] = ans;
}


int cherryPickup(vector<vector<int>>& grid) {
    int row = grid.size(), col = grid[0].size();
    vector<vector<vector<int>>> dp (row, vector<vector<int>> (col, vector<int> (col, -1)));

    return solve (grid, 0, 0, col-1, row, col, dp);
}
```
