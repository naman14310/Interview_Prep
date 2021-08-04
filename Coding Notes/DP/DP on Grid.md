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

### 2. Count Square Submatrices with All Ones
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

### 3. Count Submatrices With All Ones (Rectangles are also allowed)
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

### 4. Largest 1-Bordered Square (Tricky)
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

### 3. Triangle
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

### 4. Unique Paths
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
