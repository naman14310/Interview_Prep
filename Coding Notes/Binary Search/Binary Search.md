# Binary Search

<br>

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

### 2. Search in Bitonic Array
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
  
### 3. Find Smallest Letter Greater Than Target
  
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

### 4. Sqrt(x) using Binary Search
  
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

### 5. Find First and Last Position of Element in Sorted Array (Lower Bound and Upper Bound)

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

### 6. Find the missing number in Arithmetic Progression
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

### 7. Search in a Infinite Sorted Array (IMP for interview)

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

### 8.  Minimum Difference element in sorted array

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

### 9. Find K Closest Elements
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

### 10. Most Beautiful Item for Each Query
You are given a 2D integer array items where items[i] = [pricei, beautyi] denotes the price and beauty of an item respectively. You are also given a 0-indexed integer array queries. For each queries[j], you want to determine the maximum beauty of an item whose price is less than or equal to queries[j]. If no such item exists, then the answer to this query is 0.

Input: items = [[1,2],[3,2],[2,4],[5,6],[3,5]], queries = [1,2,3,4,5,6]

Output: [2,4,5,5,6,6]

```
int binarySearch (vector<pair<int,int>> &v, int start, int end, int q){
    int ans = 0;

    while(start<=end){
        int mid = start + (end-start)/2;

        if(v[mid].first<=q){
            ans = v[mid].second;
            start = mid+1;
        }

        else end = mid-1;
    }

    return ans;
}


vector<int> maximumBeauty(vector<vector<int>>& items, vector<int>& queries) {
    int n = items.size();
    vector<pair<int,int>> v;
    sort(items.begin(), items.end());

    int i=0;

    while(i<n){
        int mx = items[i][1];

        while(i+1<n and items[i][0]==items[i+1][0]){
            i++;
            mx = max(mx, items[i][1]);
        }

        if(v.size()==0) v.push_back({items[i][0],mx});
        else v.push_back({items[i][0], max(v.back().second, mx)});            
        i++;
    }


    vector<int> res;

    for(int q : queries)
        res.push_back(binarySearch(v, 0, v.size()-1, q));

    return res;
}
```


<br>


## @ Hard

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

<br>

### 2. Minimum Number of Operations to Make Array Continuous 
You are given an integer array nums. In one operation, you can replace any element in nums with any integer. nums is considered continuous if both of the following conditions are fulfilled:
1. All elements in nums are unique.
2. The difference between the maximum element and the minimum element in nums equals nums.length - 1.

Return the minimum number of operations to make nums continuous.

Input: nums = [1,2,3,5,6]

Output: 1

[Explaination with pictures](https://leetcode.com/problems/minimum-number-of-operations-to-make-array-continuous/discuss/1470900/Python-Binary-Search-explanation-with-pictures.)

Hint: Traverse the list A, for the current number a, we need to find out how many unique numbers in the range [a, a + n - 1] (inclusively), present in A.

```cpp
int minOperations(vector<int>& nums) {
    int n = nums.size();
    int ans = n;

    sort(nums.begin(), nums.end());

    vector<int> unique (n+1, 0);          // --> It'll give us cnt of unique numbers in a given range in O(1)
    unique[0] = 1;

    for(int i=1; i<n; i++){

        unique[i] = unique[i-1];

        if(nums[i]!=nums[i-1])
            unique[i]++;
    }

    unique[n] = unique[n-1];

    /* 
        Iterating over sorted nums, and cnt unqiue elements present in array of range [start, end] 
        by making every element start in each iteration.
    */

    for(int i=0; i<n; i++){

        int start = nums[i];
        int end = start + n-1;

        int ub = upper_bound(nums.begin(), nums.end(), end) - nums.begin();
        int end_idx = ub-1;

        int unq_cnt = unique[end_idx]-unique[i]+1;
        ans = min(ans, n-unq_cnt);
    }

    return ans;

}
```
