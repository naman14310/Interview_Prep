# Binary Search

## Easy

#### 1. Peak Index in a Mountain Array

```cpp
int peakIndexInMountainArray(vector<int>& arr) {
    int n = arr.size();
    int start = 0, end = n-1;
    while(start<=end){
        int mid = start + (end - start)/2;
        if(mid==0) start = mid + 1;
        else if(mid==n-1) end = mid-1;
        else if(arr[mid]>arr[mid+1] && arr[mid]>arr[mid-1]) return mid;
        else if(arr[mid]<arr[mid-1]) end = mid-1;
        else start = mid+1;
    }
    return -1;
}
```
  
#### 2. Find Smallest Letter Greater Than Target
  
```cpp
char nextGreatestLetter(vector<char>& letters, char target) {
    int n = letters.size();
    int start = 0, end = n-1;
    char ans = '#';
    while(start<=end){
        int mid = start + (end-start)/2;
        if(letters[mid]>target){
            ans = letters[mid];
            end = mid-1;
        } 
        else start = mid+1;
    }
    return ans=='#' ? letters[0] : ans;
}
```
  
#### 3. Sqrt(x) using Binary Search
  
```cpp
int mySqrt(int x) {
    int start = 1;
    int end = x;
    int ans = 0;
    while(start<=end){
        long int mid = start + (end-start)/2;
        long long unsigned int sqr = mid*mid;
        if(sqr==x) return mid;
        else if(sqr>x) end = mid-1;
        else{
            ans = mid; 
            start = mid+1;
        } 
    }
    return ans;
}
```
## Medium

#### 1. Capacity To Ship Packages Within D Days (Binary Search on Answer)
A conveyor belt has packages that must be shipped from one port to another within D days. The ith package on the conveyor belt has a weight of weights[i]. Each day, we load the ship with packages on the conveyor belt (in the order given by weights). We may not load more weight than the maximum weight capacity of the ship. Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within D days.

```cpp
bool isValid(vector<int> & weights, int D, int capacity){
    int count = 1;
    int i = 0;
    int cap = 0;
    while(i<weights.size()){
        cap += weights[i];
        if(cap>capacity){
            count++;
            cap=0;
        }
        else
            i++;
    }
    if(count>D) return false;
    else return true;
}

int binarySearch(vector<int> & weights, int low, int high, int D){
    int ans = high;
    while(low<=high){
        int mid = low + (high-low)/2;
        if(isValid(weights, D, mid)){
            ans = mid;
            high = mid-1;
        }
        else low = mid+1;
    }
    return ans;
}


int shipWithinDays(vector<int>& weights, int D) {
    int low = 0;
    int high = 0;
    for(int i : weights){
        low = max(low, i);
        high+=i;
    }
    return binarySearch(weights, low, high, D);
}
```

#### 2. Find Minimum in Rotated Sorted Array

```cpp
int findMin(vector<int>& nums) {
    int n = nums.size();
    int start = 0, end = n-1;

    while(start<end){
        int mid = start + (end-start)/2;
        if(nums[start] < nums[end]) return nums[start];
        if(nums[start] <= nums[mid]) start = mid+1;
        else end = mid;
    }
    return nums[start];
}
```

#### 3. Find Minimum in Rotated Sorted Array II (Repeating values)

```cpp
int findMin(vector<int>& nums) {
    int n = nums.size();
    int start = 0, end = n-1; 

    while(start<=end){
        int mid = start + (end-start)/2;
        if(nums[mid]>nums[end]) start = mid+1;
        else if(nums[mid]<nums[end]) end = mid; 
        else end--;
    }   
    return nums[start];
}
```

#### 4. Longest Increasing Subsequence (NlogN approach using Binary Search)

PS: It will only give correct Length but not correct sequence.

```cpp
int lengthOfLIS(vector<int>& nums) {
    vector<int> res;
    int len = 1;
    res.push_back(nums[0]);

    for(int i=1; i<nums.size(); i++){
        if(nums[i]>res[len-1]){
            res.push_back(nums[i]);
            len++;
        }
        else{
            auto lb = lower_bound(res.begin(), res.end(), nums[i]);
            int pos = lb-res.begin();
            res[pos] = nums[i];
        }
    }
    return len;
}
```

#### 5. Find First and Last Position of Element in Sorted Array (Lower Bound and Upper Bound)

```cpp
int lowerBound(vector<int> & nums, int target, int n){
    int start = 0, end = n-1;
    int ans = -1;
    while(start<=end){
        int mid = start + (end-start)/2;
        if(nums[mid]>=target){
            ans = mid;
            end = mid-1;
        }
        else start = mid + 1;
    }
    return ans;
}

int upperBound(vector<int> & nums, int target, int n){
    int start = 0, end = n-1;
    int ans = -1;
    while(start<=end){
        int mid = start + (end-start)/2;
        if(nums[mid]>target){
            ans = mid;
            end = mid-1;
        }
        else start = mid + 1;
    }
    return ans;
}

vector<int> searchRange(vector<int>& nums, int target) {
    vector<int> res(2, -1);
    int n = nums.size();
    if(n==0) return res;

    int lb = lowerBound(nums, target, n);
    if(lb==-1) return res;
    int ub = upperBound(nums, target, n);

    if(lb==ub) return res;
    res[0] = lb;
    res[1] = ub==-1 ? n-1 : ub-1;
    return res;
}
```

#### 6. Search in Rotated Sorted Array (Values are distinct)

```cpp
int findMinPos(vector<int> & nums){
    int n = nums.size();
    int start = 0, end = n-1;
    while(start<end){
        int mid = start + (end-start)/2;
        if(nums[start]<nums[end]) return start;
        else if(nums[start]<=nums[mid]) start = mid+1;
        else end = mid;
    }
    return start;
}

int binarySearch(vector<int> & arr, int start, int end, int target){
    while(start<=end){
        int mid = start + (end-start)/2;
        if(arr[mid]==target) return mid;
        else if (arr[mid]>target) end = mid-1;
        else start = mid+1;
    }
    return -1;
}

int search(vector<int>& nums, int target) {
    int n = nums.size();
    int pos = findMinPos(nums);
    int ans = binarySearch(nums, 0, pos-1, target);
    if(ans!=-1) return ans;
    else return binarySearch(nums, pos, n-1, target);
}
```

#### 7. Search in Rotated Sorted Array II (Values can be repeating)

```cpp
bool search(vector<int>& nums, int target) {
    int start = 0, end = nums.size()-1;
    int len = nums.size();

    while(start<=end){
        int mid = start + (end-start)/2;
        if(nums[mid]==target) return true;

        else if(nums[mid]>nums[start]){
            if(nums[mid]<target){
                start = mid+1;
                continue;
            }
            else if(target>=nums[start]){
                end = mid-1;
                continue;
            }
        }

        else if(nums[mid]<nums[end]){
            if(nums[end]<target){
                end = mid-1;
                continue;
            }
            else if(target<=nums[end] && target > nums[mid]){
                start = mid+1;
                continue;
            }
        }

        if(target == nums[start]) return true;
        else start++;

        if(target == nums[end]) return true;
        else end--;
    }
    return false;
}
```

#### 8. Pow(x, n) (Fast exponentiation using Binary Search)

```cpp
double myPow(double x, int n) {
    if(n==0) return 1;
    if(n>0){
        double res = 1;
        while(n>0){
            if(n&1) res*=x;
            x*=x;
            n>>=1;
        }
        return res;
    }
    else{
        n = abs(n);
        double res = 1;
        while(n>0){
            if(n&1) res*=1/x;
            x*=x;
            n>>=1;
        }
        return res;
    }
}
```

## @Binary Search on Matrix

#### 1. Search a 2D Matrix II

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

#### 2. Kth Smallest Element in a Sorted Matrix (Binary Search on Answer)
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
