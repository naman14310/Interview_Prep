# Matrix

## Medium

#### 1. Sort the matrix Diagonally

![matrix](https://assets.leetcode.com/uploads/2020/01/21/1482_example_1_2.png)

Hint: First iterate from top-right mat[0][col] to top-left mat[0][0] and sort diagnolly. Then move from mat[row][0] to mat[0][0] and sort diagonally.

```cpp
vector<vector<int>> diagonalSort(vector<vector<int>>& mat) {
        int r = mat.size(), c = mat[0].size();
        
        for(int y = 0; y<c; y++){
            vector<int> v;
            int i=0, j=y, idx = 0;
            while(i<r && j<c){
                v.push_back(mat[i][j]);
                i++; j++;
            }
            
            sort(v.begin(), v.end());
            
            i=0, j=y;
            while(i<r && j<c){
                mat[i][j] = v[idx];
                i++; j++; idx++;
            }
        }
        
        for(int x = 1; x<r; x++){
            vector<int> v;
            int i=x, j=0, idx = 0;
            while(i<r && j<c){
                v.push_back(mat[i][j]);
                i++; j++;
            }
            
            sort(v.begin(), v.end());
            
            i=x, j=0;
            while(i<r && j<c){
                mat[i][j] = v[idx];
                i++; j++; idx++;
            }
        }
        return mat;
    }
```

#### 2. Rotate Image

```cpp
/*
 * clockwise rotate
 * first reverse up to down, then swap the symmetry 
 * 1 2 3     7 8 9     7 4 1
 * 4 5 6  => 4 5 6  => 8 5 2
 * 7 8 9     1 2 3     9 6 3
*/

void rotate(vector<vector<int> > &matrix) {
    reverse(matrix.begin(), matrix.end());
    for (int i = 0; i < matrix.size(); ++i) {
        for (int j = i + 1; j < matrix[i].size(); ++j)
            swap(matrix[i][j], matrix[j][i]);
    }
}

/*
 * anticlockwise rotate
 * first reverse left to right, then swap the symmetry
 * 1 2 3     3 2 1     3 6 9
 * 4 5 6  => 6 5 4  => 2 5 8
 * 7 8 9     9 8 7     1 4 7
*/

void anti_rotate(vector<vector<int> > &matrix) {
    for (auto vi : matrix) reverse(vi.begin(), vi.end());
    for (int i = 0; i < matrix.size(); ++i) {
        for (int j = i + 1; j < matrix[i].size(); ++j)
            swap(matrix[i][j], matrix[j][i]);
    }
}
```

#### 3. Spiral Matrix

![matrix](https://assets.leetcode.com/uploads/2020/11/13/spiral1.jpg)

```cpp
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> ans;
        int row = matrix.size(), col = matrix[0].size();
        int count = 1;
        int sqr = row*col;
        int i, j, endx = row, endy = col, startx = 0, starty = 0;
        while(count<=sqr){
 
            i = startx; j = starty;
            for(; j<endy; j++){
                if(count>sqr) break;
                ans.push_back(matrix[i][j]);
                count++;
            }
            
            i++; j--;
            
            for(; i<endx; i++){
                if(count>sqr) break;
                ans.push_back(matrix[i][j]);
                count++;
            }
            
            i--; j--;
            
            for(; j>=starty; j--){
                if(count>sqr) break;
                ans.push_back(matrix[i][j]);
                count++;
            }
            
            j++; i--;
            
            for(; i>startx; i--){
                if(count>sqr) break;
                ans.push_back(matrix[i][j]);
                count++;
            }
            
            starty++; startx++; endx--; endy--;
        }
        return ans;
    }
```

#### 4. Set Matrix Zeroes
Given an m x n matrix. If an element is 0, set its entire row and column to 0. Do it in-place.

![matrix](https://assets.leetcode.com/uploads/2020/08/17/mat2.jpg)

Hint: Use first row and first col as auxillary vectors to mark 0 for whole cols and rows respectively.

```cpp
/* OPTIMIZED SOLUTION WITH CONSTANT SPACE */
    
    void setZeroes(vector<vector<int>>& matrix) {
        int row = matrix.size();
        int col = matrix[0].size();
        bool firstRow = false;
        bool firstCol = false;
        
        for(int j=0; j<col; j++){
            if(matrix[0][j]==0) {
                firstRow = true;
                break;
            }
        }
        
        for(int i=0; i<row; i++){
            if(matrix[i][0]==0) {
                firstCol = true;
                break;
            }
        }
        
        for(int i=1; i<row; i++){
            for(int j=1; j<col; j++){
                if(matrix[i][j]==0){
                    matrix[0][j]=0;
                    matrix[i][0]=0;
                }
            }
        }
        
        for(int i=1; i<row; i++){
            for(int j=1; j<col; j++){
                if(matrix[0][j]==0 || matrix[i][0]==0){
                    matrix[i][j] = 0;
                }
            }
        }
        
        if(firstRow){
            for(int j=0; j<col; j++)
                matrix[0][j] = 0;         
        }
        
        if(firstCol){
            for(int i=0; i<row; i++)
                matrix[i][0] = 0;
        }
    }
```

#### 5. Rotating the Box
You are given an m x n matrix of characters box representing a side-view of a box. Each cell of the box is one of the following:

1. A stone '#'
2. A stationary obstacle '*'
3. Empty '.'

The box is rotated 90 degrees clockwise, causing some of the stones to fall due to gravity. Each stone falls down until it lands on an obstacle, another stone, or the bottom of the box. Gravity does not affect the obstacles' positions, and the inertia from the box's rotation does not affect the stones' horizontal positions. It is guaranteed that each stone in box rests on an obstacle, another stone, or the bottom of the box. Return an n x m matrix representing the box after the rotation described above.

![box](https://assets.leetcode.com/uploads/2021/04/08/rotatingtheboxleetcode3withstone.png)

```cpp
vector<vector<char>> rotateTheBox(vector<vector<char>>& box) {
        int row = box.size(), col = box[0].size();
        vector<vector<char>> res (col, vector<char> (row, '.'));

        for(int i=0; i<row; i++){
            int top = col-1;
            for(int j=col-1; j>=0; j--){

                if(box[i][j]=='*'){
                    res[j][row-i-1] = '*';
                    top = j-1;
                }
                else if(box[i][j]=='#'){
                    res[top][row-i-1] = '#';
                    top--;
                }
            }
        }
        return res;
}
```
