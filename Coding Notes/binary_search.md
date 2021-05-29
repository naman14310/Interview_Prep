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

#### 1. Find Minimum in Rotated Sorted Array

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

#### 2. Find Minimum in Rotated Sorted Array II (Repeating values)

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

#### 3. Longest Increasing Subsequence (NlogN approach using Binary Search)

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

#### 4. Find First and Last Position of Element in Sorted Array (Lower Bound and Upper Bound)

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

#### 5. Search in Rotated Sorted Array (Values are distinct)

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

#### 6. Search in Rotated Sorted Array II (Values can be repeating)

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

#### 7. Pow(x, n) (Fast exponentiation using Binary Search)

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

## @ Binary Search on Answer (Tricky Questions)

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

#### 2. Koko Eating Bananas (Binary Search on Answer)
Koko loves to eat bananas. There are n piles of bananas, the ith pile has piles[i] bananas. The guards have gone and will come back in h hours. Koko can decide her bananas-per-hour eating speed of k. Each hour, she chooses some pile of bananas and eats k bananas from that pile. If the pile has less than k bananas, she eats all of them instead and will not eat any more bananas during this hour. Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return. Return the minimum integer k such that she can eat all the bananas within h hours.

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

#### 3. Minimum Number of Days to Make m Bouquets (Binary Search on Answer)
Given an integer array bloomDay, an integer m and an integer k. We need to make m bouquets. To make a bouquet, you need to use k adjacent flowers from the garden.
The garden consists of n flowers, the ith flower will bloom in the bloomDay[i] and then can be used in exactly one bouquet. Return the minimum number of days you need to wait to be able to make m bouquets from the garden. If it is impossible to make m bouquets return -1.

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

#### 4. Find the Smallest Divisor Given a Threshold
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

#### 5. Sum of Mutated Array Closest to Target
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

#### 6. Allocate minimum number of pages
You are given N number of books. Every ith book has Ai number of pages.You have to allocate books to M number of students. The task is to find that particular permutation in which the maximum number of pages allocated to a student is minimum of those in all the other permutations and print this minimum value. 

Each book will be allocated to exactly one student. Each student has to be allocated at least one book.

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


## @ Binary Search on Matrix

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

#### 3. Median in a row-wise sorted Matrix

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

#### 4. Row with max 1s (Best approach - O(m+n))
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

## Hard

#### 1. Median of Two Sorted Arrays (Log(m+n) approach)

[Video Solution](https://www.youtube.com/watch?v=ws98ud9uxl4)

Approach:
1. Let smaller array be nums1 and other one be nums2.
2. Run binary search over nums1 to calculate mid1. Let start = 0 and end = n. => then mid = start + (end-start)/2
3. Now calculate mid2 using this formula => mid2 = (n+m+1)/2 - mid1
4. Create four variable maxLeft1, maxLeft2, minRight1, minRight2 for endpoints. (Basically we are dividing elements into two logical sets)
5. Now if maxLeft1<=minRight2 && maxLeft2<=minRight1 => then return median.
6. Else if maxLeft1>minRight2 => shift to left => end = mid-1
7. Else shift to right => start = mid+1

```cpp
double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {    
    int n = nums1.size();
    int m = nums2.size();
    if(n>m) return findMedianSortedArrays(nums2, nums1);

    int start = 0, end = n;
    while(start<=end){
        int mid1 =  start + (end-start)/2;
        int mid2 = (n+m+1)/2 - mid1;

        int maxLeft1 = mid1==0 ? INT_MIN : nums1[mid1-1];
        int maxLeft2 = mid2==0 ? INT_MIN : nums2[mid2-1];
        int minRight1 = mid1==n ? INT_MAX : nums1[mid1];
        int minRight2 = mid2==m ? INT_MAX : nums2[mid2];

        if(maxLeft1<=minRight2 && maxLeft2<=minRight1){
            if((m+n)%2==0)
                return (double) (max(maxLeft1, maxLeft2) + min(minRight1, minRight2))/2.0;
            else
                return (double) max(maxLeft1, maxLeft2);
        }

        else if(maxLeft1>minRight2) end = mid1-1;
        else start = mid1+1;
    }
   return -1;
}
```
