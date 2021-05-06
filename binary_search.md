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
A conveyor belt has packages that must be shipped from one port to another within D days. The ith package on the conveyor belt has a weight of weights[i]. Each day, we load the ship with packages on the conveyor belt (in the order given by weights). We may not load more weight than the maximum weight capacity of the ship. 
Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within D days.

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
