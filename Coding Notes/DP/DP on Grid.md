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
