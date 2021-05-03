# Matrix

## Medium

#### 1. Sort the matrix Diagonally

![matrix](https://assets.leetcode.com/uploads/2020/01/21/1482_example_1_2.png)


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
