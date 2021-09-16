# Binary Search

## @ Easy

### 1. Peak Index in a Mountain Array

```cpp
int peakIndexInMountainArray(vector<int>& arr) {
    int start = 0, end = arr.size()-1;
    
    while(start<=end){
        int mid = start + (end - start)/2;
        
        if(mid==0) 
            start = mid + 1;
            
        else if(mid==n-1) 
            end = mid-1;
            
        else if(arr[mid]>arr[mid+1] && arr[mid]>arr[mid-1]) 
            return mid;
            
        else if(arr[mid]<arr[mid-1]) 
            end = mid-1;
            
        else start = mid+1;
    }
    return -1;
}
```

<br>
  
### 2. Find Smallest Letter Greater Than Target
  
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

<br>

### 3. Sqrt(x) using Binary Search
  
```cpp
int mySqrt(int x) {
    int start = 1, end = x;
    int ans = 0;
    
    while(start<=end){
        long int mid = start + (end-start)/2;
        long long unsigned int sqr = mid*mid;
        
        if(sqr==x) 
            return mid;
            
        else if(sqr>x) 
            end = mid-1;
            
        else{
            ans = mid; 
            start = mid+1;
        } 
    }
    
    return ans;
}
```

<br>

### 4. Search in Bitonic Array
A Bitonic Sequence is a sequence of numbers which is first strictly increasing then after a point strictly decreasing. Return Index of element.

Hint: First find peak using Binary Search. Then search in left part and right part.

```cpp
int findPeak(vector<int> &A, int B){
    int start = 0, end = A.size()-1;

    while(start<=end){
        int mid = start + (end-start)/2;
        
        if(A[mid]>max(A[mid-1], A[mid+1])) 
            return mid;

        else if(A[mid]<A[mid+1]) 
            start++;

        else end--;
    }

    return -1;
}


int binarySearch(vector<int> &A, int B, int start, int end, bool inc){
    while(start<=end){
        int mid = start + (end-start)/2;
        if(A[mid]==B) return mid;

        else if(A[mid]<B){
            if(inc) start++;
            else end--;
        } 
        else{
            if(inc) end--;
            else start++;
        }
    }
    return -1;
}


int Solution::solve(vector<int> &A, int B) {
    int peak = findPeak(A, B);
    if(A[peak]==B) 
        return peak;

    int left = binarySearch(A, B, 0, peak-1, true);
    if(left!=-1) return left;

    int right = binarySearch(A, B, peak+1, A.size()-1, false);
    return right;
}
```

<br>


## @ Medium

### 1. Find Minimum in Rotated Sorted Array 

Note: Similar to Number of times a sorted array is rotated

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

<br>

### 2. Find Minimum in Rotated Sorted Array II (Repeating values)

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

<br>

### 3. Search in Rotated Sorted Array (Values are distinct)

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

<br>

### 4. Search in Rotated Sorted Array II (Values can be repeating)

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

<br>

### 5. Longest Increasing Subsequence (NlogN approach)

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

<br>

### 6. Find First and Last Position of Element in Sorted Array (Lower Bound and Upper Bound)

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

<br>

### 7. Find the missing number in Arithmetic Progression
Given an array that represents elements of arithmetic progression in order. One element is missing in the progression, find the missing number.

Input: arr[]  = {2, 4, 8, 10, 12, 14}

Output: 6

```cpp
int findMissingUtil(int arr[], int low, int high, int diff){
    if (high <= low)
        return INT_MAX;
  
    int mid = low + (high - low) / 2;
  
    /* The element just after the middle element is missing*/
    
    if (arr[mid + 1] - arr[mid] != diff)
        return (arr[mid] + diff);
  
    /* The element just before mid is missing */
    
    if (mid > 0 && arr[mid] - arr[mid - 1] != diff)
        return (arr[mid - 1] + diff);

    if (arr[mid] == arr[0] + mid * diff)
        return findMissingUtil(arr, mid + 1, high, diff);
  
    return findMissingUtil(arr, low, mid - 1, diff);
}
  
int findMissing(int arr[], int n) {
    int diff = (arr[n - 1] - arr[0]) / n;
    return findMissingUtil(arr, 0, n - 1, diff);
}
```

<br>

### 8. Search in a Infinite Sorted Array (IMP for interview)

```cpp
#include<bits/stdc++.h>
using namespace std;
int INFINITE = 100000;


int binarySearch(vector<int> & v, int start, int end, int val){
    while(start<=end){
        int mid = start + (end-start)/2;
        if(v[mid]==val) return mid;
        if(v[mid]>val) end = mid-1;
        else start = mid+1;
    }
    return -1;
}


pair<int,int> find_start_end(vector<int> & v, int val){
    int start=0, end=1;
    while(val>end){
        start = end;
        end *= 2;
    }
    return {start,end};
}


int main(){
    int val = 15;                    // ----> TO BE SEARCHED
    vector<int> v (INFINITE, 0);     // ---->  [1, 2, 3, 4, 5 . . . . . INFIITE]
    for(int i=0; i<INFINITE; i++)
        v[i] = i+1;

    auto p = find_start_end(v, val);
    int start = p.first, end = p.second;
    cout<<binarySearch(v, start, end, val);
}
```

<br>

### 9.  Minimum Difference element in sorted array

```cpp
int binarySearch(vector<int> & v, int val){
    int start = 0, end = v.size()-1;
    while(start<=end){
        int mid = start + (end-start)/2;
        if(v[mid]==val) return v[mid];
        if(v[mid]>val) end = mid-1;
        else start = mid+1;
    }

    if(end<0) return v[start];
    if(start>v.size()-1) return v[end];
    return abs(v[start]-val) < abs(v[end]-val) ? v[start] : v[end];
}
```

<br>

### 10. Find K Closest Elements
Given a sorted integer array arr, two integers k and x, return the k closest integers to x in the array. The result should also be sorted in ascending order. An integer a is closer to x than an integer b if:
1. |a - x| < |b - x|, or
2. |a - x| == |b - x| and a < b

```cpp
vector<int> findClosestElements(vector<int>& arr, int k, int x) {
    int n = arr.size();
    deque<int> dq;
    vector<int> res;

    int lb = lower_bound(arr.begin(), arr.end(), x) - arr.begin();
    int i = lb-1, j = lb;  

    /* Simply push smaller value to deque and move i and j accordingly */ 

    while(k--){

        if(i<0){
            dq.push_back(arr[j]);
            j++;
        }

        else if(j==n){
            dq.push_front(arr[i]);
            i--;
        }

        else{

            if(abs(arr[i]-x)<=abs(arr[j]-x)){
                dq.push_front(arr[i]);
                i--;
            }
            else{
                dq.push_back(arr[j]);
                j++;
            }
        }
    }


    while(!dq.empty()){
        res.push_back(dq.front());
        dq.pop_front();
    }

    return res;
}
```

<br>

## @ Binary Search on Answer (Tricky Questions)

### 1. Capacity To Ship Packages Within D Days 
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

<br>

### 2. Koko Eating Bananas 
There are n piles of bananas, the ith pile has piles[i] bananas. The guards have gone and will come back in h hours. Koko can decide her bananas-per-hour eating speed of k. Each hour, she chooses some pile of bananas and eats k bananas from that pile. If the pile has less than k bananas, she eats all of them instead and will not eat any more bananas during this hour. Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return. Return the minimum integer k such that she can eat all the bananas within h hours.

**Example**
Input: piles = [30,11,23,4,20], h = 5
Output: 30

```cpp
bool isvalid(vector<int> & piles, long long int mid, int h){
    long long int cnt = 0;
    for(int i=0; i<piles.size(); i++){
        if(piles[i]<=mid) cnt++;
        else cnt += (int) ceil ((double)piles[i]/(double)mid);
    }
    if(cnt>h) return false;
    return true;
}

int binarySearch(vector<int> & piles, long long int start, long long int end, int h){
    int ans = end;
    while(start<=end){
        long long int mid = start + (end-start)/2;
        if(isvalid(piles, mid, h)){
            ans = mid;
            end = mid-1;
        }
        else start = mid+1;
    }
    return ans;
}

int minEatingSpeed(vector<int>& piles, int h) {
    long long int low = 1, high = 0;
    for(int i : piles) high+=i;
    return binarySearch(piles, low, high, h);
}
```

<br>

### 3. Minimum Number of Days to Make m Bouquets 
Given an integer array bloomDay, an integer m and an integer k. We need to make m bouquets. To make a bouquet, you need to use k adjacent flowers from the garden. The garden consists of n flowers, the ith flower will bloom in the bloomDay[i] and then can be used in exactly one bouquet. Return the minimum number of days you need to wait to be able to make m bouquets from the garden. If it is impossible to make m bouquets return -1.

Input: bloomDay = [7,7,7,7,12,7,7], m = 2, k = 3

Output: 12

Explanation: We need 2 bouquets each should have 3 flowers.
Here's the garden after the 7 and 12 days:

After day 7: [x, x, x, x, _, x, x]

We can make one bouquet of the first three flowers that bloomed. We cannot make another bouquet from the last three flowers that bloomed because they are not adjacent.

After day 12: [x, x, x, x, x, x, x]

It is obvious that we can make two bouquets in different ways.

```cpp
bool isValid(vector<int> & bloomDay, int mid, int m, int k){
    int i=0, sz=0, cnt=0;
    while(i<bloomDay.size()){
        if(i==0 || bloomDay[i-1]>mid) sz=0;
        if(bloomDay[i]<=mid) sz++;
        if(sz==k){
            sz=0;
            cnt++;
        }
        i++;
    }
    if(cnt<m) return false;
    return true;        
}

int binarySearch(vector<int> & bloomDay, int start, int end, int m, int k){
    int ans = end;
    while(start<=end){
        int mid = start + (end-start)/2;
        if(isValid(bloomDay, mid, m, k)){
            ans = mid;
            end = mid-1;
        }
        else
            start = mid+1;           
    }
    return ans;
}

int minDays(vector<int>& bloomDay, int m, int k) {
    int n = bloomDay.size();
    if(m*k > n) return -1;

    int low = INT_MAX;
    int high = INT_MIN;

    for(int b : bloomDay){
        low = min(b, low);
        high = max(b, high);
    }
    return binarySearch(bloomDay, low, high, m, k);
}
```

<br>

### 4. Find the Smallest Divisor Given a Threshold
Given an array of integers nums and an integer threshold, we will choose a positive integer divisor, divide all the array by it, and sum the division's result. Find the smallest divisor such that the result mentioned above is less than or equal to threshold. Each result of the division is rounded to the nearest integer greater than or equal to that element. (For example: 7/3 = 3 and 10/2 = 5).

```cpp
bool isValid(vector<int> nums, int mid, int threshold){
    int sum=0;
    for(auto i : nums){
        double d = ceil((double)i/(double)mid);
        sum+= (int)d;
    }
    if(sum>threshold) return false;
    return true;
}

int binarySearch(vector<int>nums, int start, int end, int threshold){
    int ans = end;
    while(start<=end){
        int mid = start + (end-start)/2;
        if(isValid(nums, mid, threshold)){
            ans = mid;
            end = mid-1;
        }
        else
            start = mid+1;
    }
    return ans;
}

int smallestDivisor(vector<int>& nums, int threshold) {
    int low = 1;
    int high = INT_MIN;
    for(int i : nums)
        high = max(high, i);
    
    return binarySearch(nums, low, high, threshold);
}
```

<br>

### 5. Sum of Mutated Array Closest to Target
Given an integer array arr and a target value target, return the integer value such that when we change all the integers larger than value in the given array to be equal to value, the sum of the array gets as close as possible (in absolute difference) to target. In case of a tie, return the minimum such integer.

```cpp
int getSum(vector<int> & arr, int mid, int target){
    int sum=0;
    for(auto i : arr){
        if(i>mid) sum+=mid;
        else sum+=i;
    }
    return sum;
}


int binarySearch(vector<int> & arr, int start, int end, int target, int & mindiff){
    int ans = end;
    while(start<=end){
        int mid = start + (end-start)/2;

        int sum = getSum(arr, mid, target);
        int diff = abs(sum-target);

        if(diff<mindiff){
            ans = mid;
            mindiff = diff;
        }

        else if(diff==mindiff && mid<ans){
            ans=mid;
        }

        if(sum<target) start = mid+1;
        else end = mid-1;
    }
    return ans;     
}


int findBestValue(vector<int>& arr, int target) {
    int low = 0;
    int high = INT_MIN;
    for(int i : arr){
        high = max(high, i);
    }
    int mindiff = INT_MAX;
    return binarySearch(arr, low, high, target, mindiff);
}
```

<br>

### 6. Allocate minimum number of pages
You are given N number of books. Every ith book has Ai number of pages.You have to allocate books to M number of students. The task is to find that particular permutation in which the maximum number of pages allocated to a student is minimum of those in all the other permutations and print this minimum value. Each book will be allocated to exactly one student. Each student has to be allocated at least one book.

Note: Return -1 if a valid assignment is not possible, and allotment should be in contiguous order

```cpp
bool isValid(int arr[], int mid, int m, int n){
    int i=0, sum=0, cnt=1;
    while(i<n){
        sum+=arr[i];

        if(sum>mid){
            cnt++;
            sum=arr[i];
        }
        i++;
    }
    if(cnt<=m) return true;
    else return false;
}

int binarySearch(int arr[], int start, int end, int m, int n){
    int ans = -1;
    while(start<=end){
        int mid = start + (end-start)/2;

        if(isValid(arr, mid, m, n)){
            ans = mid;
            end = mid-1;
        }
        else start = mid+1;
    }
    return ans;
}

int findPages(int arr[], int n, int m) 
{   if(n<m) return -1;

    int low = INT_MIN, high = 0;
    for(int i=0; i<n; i++){
        low = max(low, arr[i]);
        high+=arr[i];
    }

    int res = binarySearch(arr, low, high, m, n);
    
    int maxpage = 0, i=0, sum=0;
    while(i<n){
        sum+=arr[i];
        if(sum>res){
            maxpage = max(maxpage, sum-arr[i]);
            sum=arr[i];
        }
        i++;
    }
    return max(sum,maxpage);
}
```

<br>

### 7. Split Array Largest Sum
Given an array nums which consists of non-negative integers and an integer m, you can split the array into m non-empty continuous subarrays. Write an algorithm to minimize the largest sum among these m subarrays.

Input: nums = [7,2,5,10,8], m = 2

Output: 18

```cpp
bool isValid (vector<int> & nums, int mid, int m){
    int i=0, sum=0;
    int cnt = 1;

    while(i<nums.size()){
        if(sum+nums[i]>mid){
            sum=0;
            cnt++;
        }
        else{
            sum += nums[i];
            i++;
        }
    }

    return cnt<=m;
}


int binarySearch (vector<int> & nums, int start, int end, int m){
    int ans = end;

    while(start<=end){
        int mid = start + (end-start)/2;

        if(isValid(nums, mid, m)){
            ans = mid;
            end = mid-1;
        }
        else
            start = mid+1;
    }

    return ans;
}


int splitArray(vector<int>& nums, int m) {
    int start = *max_element(nums.begin(), nums.end());
    int end = accumulate(nums.begin(), nums.end(), 0);

    return binarySearch (nums, start, end, m);
}
```

<br>

### 8. WoodCutting Made Easy (IB)

[Question](https://www.interviewbit.com/problems/woodcutting-made-easy/)

```cpp
bool isValid(vector<int> &A, long long B, long long bladeHeight){
    long long sum=0;

    for(auto h : A)
        if(h>bladeHeight) sum += h-bladeHeight;
    
    return sum>=B;
}


int binarySearch (vector<int> &A, long long B, long long low, long long high){
    long long ans = -1;
    
    while(low<=high){
        long long mid = low + (high-low)/2;

        if(isValid(A, B, mid)){
            ans = mid;
            low = mid+1;
        }

        else high = mid-1;
    }

    return ans;
}


int Solution::solve(vector<int> &A, int B) {
    long long low = 0;
    long long high = *max_element(A.begin(), A.end());

    return binarySearch(A, B, low, high);
}
```

<br>


## @ Binary Search on Matrix

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

### 2. Kth Smallest Element in a Sorted Matrix (Binary Search on Answer)
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

```cpp
pair<int,int> count(vector<vector<int>> & matrix, int val, int row, int col){
    int leftCount = 0, rightCount = 0;
    for(int i=0; i<row; i++){
        int pos = upper_bound(matrix[i].begin(), matrix[i].end(), val) - matrix[i].begin();
        leftCount += pos;
        rightCount += col-pos;
    }
    return {leftCount, rightCount};
}

int median(vector<vector<int>> &matrix, int r, int c){
    int row = matrix.size(), col = matrix[0].size();
    int low = INT_MAX, high = INT_MIN;

    for(int i=0; i<row; i++){
        low = min(low, matrix[i][0]);
        high = max(high, matrix[i][col-1]);
    }

    while(low<=high){
        int mid = low + (high-low)/2;
        auto p = count(matrix, mid, row, col);

        int lc = p.first;
        int rc = p.second;

        if(lc-1<rc) low = mid+1;
        else high = mid-1;
    }
    return low;
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

<br>

## Hard

### 1. Median of Two Sorted Arrays (Log(m+n) approach)

[Video Solution](https://www.youtube.com/watch?v=ws98ud9uxl4)

Approach:
1. Let smaller array be nums1 and other one be nums2.
2. Run binary search over nums1 to calculate cut1. Let start = 0 and end = m. => then cut1 = start + (end-start)/2
3. Now calculate cut using this formula => cut2 = (m+n+1)/2 - cut1
4. Create four variable l1, l2, r1, r2 for endpoints. (Basically we are dividing elements into two logical sets)
5. Now if l1<=r2 && l2<=r1 => then return median.
6. Else if l1>r2 => shift to left => end = mid-1
7. Else shift to right => start = mid+1

```cpp
class Solution {
public:
    
    /*
    
                                                Approach
                                                --------
                                
                                           
    step1:  Initialise start=0 and end=m and find cut1 = start+(end-start)/2 
    
    step2:  Find cut2 for respective cut1 using, cut2 = (m+n+1)/2 - cut1
                                 
    step3:  Compute 4 coordinates i.e. l1, l2 (left nbr of cut1 and cut2) and r1, r2 (right nbr of cut1 and cut2)
    
    step4:  (a) If max(l1, l2) <= min(r1, r2) ==> we found the correct partitions, hence return median
    
            (b) Else if l1>r2 move end to cut1-1
            
            (c) Else move start to cut1+1
            
    
    --------------------------------------------------------------------------------------------------------------
             
                Iteration 1:
    
                                    arr1 (length m) :           1 4 | 5        
                                    arr2 (length n) :           2 | 3
                                    
                                    (1,4,2 belongs to left part and 5,3 belongs to right part)
                
                Iteration 2:
            
                                    arr1            :           1 | 4 5
                                    arr2            :           2 3 | INT_MAX
                                    
                                    (1,2,3 belongs to left part and 4,5 belongs to right part)

                                    Median = max(1,3) = 3
                                   
                                   
                PS: We need to perform iteration till all elements of left part becomes smaller then right part
    */
    
    
    
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(), n = nums2.size();
        
        /* Our code will work fine if size of first array is smaller */
        
        if(m>n) return findMedianSortedArrays(nums2, nums1);

        /* Apply binary search only on first array */
        
        int start = 0, end = m;        
        
        while(start<=end){
            
            int cut1 = start + (end-start)/2;
            int cut2 = (m+n+1)/2 - cut1;
                                    
            double l1 = cut1==0 ? INT_MIN : nums1[cut1-1];
            double l2 = cut2==0 ? INT_MIN : nums2[cut2-1];
            
            double r1 = cut1==m ? INT_MAX : nums1[cut1];
            double r2 = cut2==n ? INT_MAX : nums2[cut2];
            
            
            /* If left part is smaller then right part simply return median based on odd-even criteria */
                        
            if(l1<=r2 and l2<=r1){
                
                if((m+n)%2==0) 
                    return (max(l1,l2) + min(r1,r2))/2;
                else
                    return max(l1, l2);
            }
            
            /* Else if leftMax of nums1 > rightMin of nums2 --> shift cut1 to left side */
                
            else if(l1>r2)
                end = cut1-1;
            
            /* Else shift cut1 to right side */
            
            else start = cut1+1;
        }
        
        return -1;
    }
};
```
