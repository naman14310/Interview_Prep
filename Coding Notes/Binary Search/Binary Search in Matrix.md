# Binary Search in Matrix

<br>

### 1. Search a 2D Matrix II

![matrix](https://assets.leetcode.com/uploads/2020/11/24/searchgrid2.jpg)

```cpp
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    int row = matrix.size(), col = matrix[0].size();
    int i = 0, j = col-1;
    while(i<row && j>=0){
        if(matrix[i][j]==target) return true;
        else if(target>matrix[i][j]) i++;
        else j--;
    }
    return false;
}
```

<br>

### 2. Kth Smallest Element in a Sorted Matrix 
Given an n x n matrix where each of the rows and columns are sorted in ascending order, return the kth smallest element in the matrix.

Approach: 
1. Use binary search on the range low to high, where low = matrix[0][0] and high = matrix[row-1][col-1]
2. Calculate mid and pass it to isValid() function.
3. Value mid is valid if count of elements lesser then mid is >= k.
4. If isValid returns true, save mid to answer and continue the search. (We will use lower bound technique here)
5. For calculation of count of elements lesser then mid => start from bottom left corner and use binary search concept.

```cpp
int isValid(vector<vector<int>> & matrix, int val, int row, int col, int k){
    int i=row-1, j=0;
    int count = 0;
    while(i>=0 && j<col){
        if(matrix[i][j]<=val){
            count += i+1;
            j++;
        }
        else i--;
    }

    if(count>=k) return true;
    else return false;
}

int kthSmallest(vector<vector<int>>& matrix, int k) {
    int row = matrix.size(), col = matrix[0].size();
    int low = matrix[0][0], high = matrix[row-1][col-1];

    int ans = -1; 
    while(low<=high){
        int mid = low + (high-low)/2;

        if(isValid(matrix, mid, row, col, k)){
            ans = mid;
            high = mid-1;
        }
        else low = mid+1;
    }
    return ans;
}
```

<br>

### 3. Median in a row-wise sorted Matrix

[Video Solution](https://www.youtube.com/watch?v=_4rxBuhyLXw)

Hint: Find minimum mid such that Left part (smaller then or equal to mid) must be greater then total/2

```cpp

/* It will return count of elements smaller then or equal to mid */

int small_count (vector<vector<int>> &A, int mid){
    int row = A.size(), col = A[0].size();  
    int cnt = 0;

    for(int i=0; i<row; i++){
        int idx = upper_bound(A[i].begin(), A[i].end(), mid) - A[i].begin();
        cnt += idx; 
    }

    return cnt;
}


int binarySearch (vector<vector<int>> &A, int low, int high, int total){
    int ans = high;

    /* 
        Divide all elements into left and right part.
        Left part will contain element smaller then or equal to mid.
        Right part will contain elemets greater then mid.

        Logic: Find minimum mid such that Left part must be greater then total/2
    */


    while(low<=high){
        int mid = low + (high-low)/2;
        int leftcnt = small_count(A, mid);

        /* 
            If cnt > total/2 : Then mid might be the ans, 
            so update ans and reduce our search space to left side. 
        */

        if(leftcnt>total/2){
            ans = mid;
            high = mid-1;
        }

        /* Else reduce search space to right side */ 

        else low = mid+1;
    }

    return ans;
}


int Solution::findMedian(vector<vector<int>> &A) {
    int row = A.size(), col = A[0].size();
    int low = INT_MAX, high = INT_MIN;

    for(int i=0; i<row; i++){
        low = min(low, A[i][0]);
        high = max(high, A[i][col-1]);
    }

    return binarySearch (A, low, high, row*col);
}
```

<br>

### 4. Row with max 1s (Best approach - O(m+n))
Given a boolean 2D array of n x m dimensions where each row is sorted. Find the 0-based index of the first row that has the maximum number of 1's.

Approach: Instead of doing binary search in every row, we first check whether the row has more 1s than max so far. If the row has more 1s, then only count 1s in the row. Also, to count 1s in a row, we don't do binary search in complete row, we do search in before the index of last max.

```cpp
int rowWithMax1s(vector<vector<int> > arr, int n, int m) {
   int ans = -1;
   int prev_pos = m;
   
   for(int i=0; i<n; i++){

       if(arr[i][prev_pos-1]==0) continue;

       int pos = lower_bound(arr[i].begin(), arr[i].begin()+prev_pos, 1) - arr[i].begin();
       prev_pos = pos;
       ans = i;

       if (prev_pos == 0) return ans;
   }
   return ans;
}
```
