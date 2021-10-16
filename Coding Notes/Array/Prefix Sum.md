# Problems on Prefix Sum

<br>

### 1. Balance Array
Given an integer array A of size N. You need to count the number of special elements in the given array. A element is special if removal of that element make the array balanced. Array will be balanced if sum of even index element equal to sum of odd index element.

Input: A = [2, 1, 6, 4]

Output: 1

Hint: Maintain PrefixSum and SuffixSum for odd and even index seperately.

```cpp
int Solution::solve(vector<int> &A) {
    int n = A.size();
    int special_elements = 0;
    
    vector<int> leftEven (n, 0);
    vector<int> leftOdd (n, 0);
    vector<int> rightEven (n, 0);
    vector<int> rightOdd (n, 0);
    
    /* Computing prefixSum and suffixSum for both even and odd indexes */

    for(int i=1; i<n; i++){
        leftEven[i] += leftEven[i-1];
        leftOdd[i] += leftOdd[i-1];

        if(i%2==0)
            leftOdd[i] += A[i-1]; 
        else
            leftEven[i] += A[i-1];
    }

    for(int i=n-2; i>=0; i--){
        rightEven[i] += rightEven[i+1];
        rightOdd[i] += rightOdd[i+1];

        if(i%2==0)
            rightOdd[i] += A[i+1];
        else
            rightEven[i] += A[i+1];
    }

    /* Iterate for every element and check if it is special or not */

    for(int i=0; i<n; i++){

        int evenSum = leftEven[i] + rightOdd[i];
        int oddSum = leftOdd[i] + rightEven[i];

        if(evenSum==oddSum) 
            special_elements++;
    }

    return special_elements;
}
```

<br>

### 2. Sum of Absolute Differences in a Sorted Array
You are given an integer array nums sorted in non-decreasing order. Build and return an integer array result with the same length as nums such that result[i] is equal to the summation of absolute differences between nums[i] and all the other elements in the array.

In other words, result[i] is equal to sum(|nums[i]-nums[j]|) where 0 <= j < nums.length and j != i (0-indexed).

Input: nums = [2,3,5]

Output: [4,3,5]

```cpp
vector<int> getSumAbsoluteDifferences(vector<int>& nums) {
    int n = nums.size();
    vector<int> res(n, 0);
    vector<int> csum(n, 0);
    csum[0] = nums[0];

    for(int i=1; i<n; i++)
        csum[i] = csum[i-1] + nums[i];


    for(int i=0; i<n; i++){
        int left = 0, right = 0;

        left = csum[i];
        right = csum[n-1] - csum[i];
        res[i] = ((i+1)*nums[i] - left) + (right - (n-i-1)*nums[i]);   
    }
    return res;
}
```

<br>

### 3. Longest Well-Performing Interval
We are given a list of the number of hours worked per day for a given employee. A day is considered to be a tiring day if and only if the number of hours worked is (strictly) greater than 8. A well-performing interval is an interval of days for which the number of tiring days is strictly larger than the number of non-tiring days. Return the length of the longest well-performing interval.

Input: hours = [9,9,6,0,6,6,9]

Output: 3

Hint: Replace all tiring days with 1 and non tiring days with -1. Now cummulative sum of well performing interval should be positive.

**Approach 1 : Using Prefix Sum - O(n2)**

```cpp
int longestWPI(vector<int>& hours) {
    int n = hours.size();

    hours[0] = hours[0]>8 ? 1 : -1;

    /* Replacing values greater then 8 with 1 and others with -1 and compute their cummulative sum */ 

    for(int i=1; i<n; i++)
        hours[i] = hours[i-1] + (hours[i]>8 ? 1 : -1);

    int ans = 0;

    for(int i=0; i<n; i++){
        for(int j=i; j<n; j++){

            int tired_cnt = i>0 ? hours[j]-hours[i-1] : hours[j]; 
            if(tired_cnt>0)
                ans = max(ans, j-i+1);
        }
    }

    return ans;
}
```

**Approach 2 : Optimized prefix sum using Hashmap - O(n)**

```cpp
int longestWPI(vector<int>& hours) {
    int n = hours.size();

    unordered_map<int, int> mp;         // --> map of pairs {csum, index}
    mp[0] = -1;        

    int csum = 0;
    int ans = 0; 

    for(int i=0; i<n; i++){

        csum += hours[i]>8 ? 1 : -1;

        /* If till now csum > 0 then it means that subarr till this index is WPI */

        if(csum>0)
            ans = max(ans, i+1);

        else{
            int gain = csum-1;          // --> gain is csum-1 becoz we want a subarray of sum atleast 1

            if(mp.find(gain)!=mp.end())
                ans = max(ans, i-mp[gain]);
        }

        /* If curr csum does not exist in map then insert it */

        if(mp.find(csum)==mp.end())
            mp[csum] = i;
    }

    return ans;
}
```

<br>

### 4. Partition Array Into Three Parts With Equal Sum 
Given an array of integers arr, return true if we can partition the array into three non-empty parts with equal sums.

Hint: Compute Prefix sum, iterate it and find sum, (2 * sum) and (3 * sum)

```cpp
bool canThreePartsEqualSum(vector<int>& arr) {
    int sum = 0;
    for(int i : arr) sum+=i;

    if(sum%3!=0) return false;

    sum = sum/3;
    for(int i=1; i<arr.size(); i++)
        arr[i] += arr[i-1]; 

    bool firstfound=false, secondfound=false;
    for(int i=0; i<arr.size(); i++){
        if(!firstfound){
            if(arr[i]==sum) firstfound = true;
        }
        else if(!secondfound){
            if(arr[i]==sum*2) secondfound = true;
        }
        else{
            if(arr[i]==sum*3) return true;
        }
    }
    return false;
}
```

<br>

### 5. Ways to Split Array Into Three Subarrays
A split of an integer array is good if:
1. The array is split into three non-empty contiguous subarrays - named left, mid, right.
2. The sum of the elements in left is less than or equal to the sum of the elements in mid, and the sum of the elements in mid is less than or equal to the sum of the elements in right.

Return the number of good ways to split nums. As the number may be too large, return it modulo 10^9 + 7.

**Approach 1 : Simple Prefix Sum - O(n2)**

```cpp
int getSum (vector<int> & nums, int i, int j){
    if(i>0) return nums[j] - nums[i-1];
    else return nums[j];
}

int waysToSplit(vector<int>& nums) {
    int n = nums.size();
    int M = 1000000007; 

    for(int i=1; i<n; i++)
        nums[i] += nums[i-1]; 

    int ans = 0;

    for(int i=0; i<n-1; i++){
        for(int j=i+1; j<n; j++){

            int sum1 = getSum(nums, 0, i), sum2 = getSum(nums, i+1, j), sum3 = getSum(nums, j+1, n-1);

            if(sum1<=sum2 and sum2<=sum3) 
                ans = (ans%M + 1)%M;

            else if(sum1>=sum2+sum3) break;
        }
    }

    return ans;
}
```

**Approach 2 : Prefix Sum + Binary Search - O(nlogn)**

```cpp
/* 
    Let i and j be two pointers which splits the array into 3 parts. 

    Condition for splitting: nums[i] <= nums[j]-nums[i] <= nums.back() - nums[j];

    which can be simplified to -
    1. nums[j] >= 2*nums[i]                     --> (condition for left boundary of j)  
    2. nums[j] <= (nums.back() + nums[i]) / 2   --> (condition for right boundary of j)
*/


/* function to calculate left boundary for j */

int getLeftBoundary (vector<int> & nums, int start, int end, int i, int n){
    int bound = n;

    while(start<=end){
        int mid = start + (end-start)/2;

        if(nums[mid] < 2*nums[i])
            start = mid+1;

        else{
            bound = mid;
            end = mid-1;
        }
    }

    return bound;
}


/* Function to calculate right boundary for j */

int getRightBoundary (vector<int> & nums, int start, int end, int i, int n){
    int bound = i;

    while(start<=end){
        int mid = start + (end-start)/2;

        if(nums[mid] > (nums[n-1] + nums[i])/2)
            end = mid-1;

        else{
            bound = mid;
            start = mid+1;
        }
    }

    return bound;
}


int waysToSplit(vector<int>& nums) {
    int n = nums.size();
    int M = 1000000007; 

    /* Computing prefix sum */

    for(int i=1; i<n; i++)
        nums[i] += nums[i-1]; 

    int ans = 0;

    /* for every i, compute leftBoundary and rightBoundary of j such that it satisfies the conditions */

    for(int i=0; i<n-2; i++){

        int leftBoundary = getLeftBoundary(nums, i+1, n-2, i, n);
        int rightBoundary = getRightBoundary(nums, i+1, n-2, i, n);

        if(rightBoundary>=leftBoundary)
            ans = (ans%M + (rightBoundary - leftBoundary + 1)%M)%M;
    }

    return ans;
}
```

<br>

### 6. Maximum Sum of Two Non-Overlapping Subarrays
Given an array nums of non-negative integers, return the maximum sum of elements in two non-overlapping (contiguous) subarrays, which have lengths firstLen and secondLen. The firstLen-length subarray could occur before or after the secondLen-length subarray.

Input: nums = [0,6,5,2,2,5,1,9,4], firstLen = 1, secondLen = 2

Output: 20

Hint: Precompute max sum for all subarrays starting from right of every index i. Do it for both subarray of len1 and len2.

```cpp
void fill_maxSubarrSum (vector<int> & nums, vector<int> & maxSubarrSum, vector<int> & prefixSum, int len, int n){
    int i=n-len, j=n-1;
    int mx_so_far = 0;

    while(i>0){
        int sum = prefixSum[j] - prefixSum[i-1];
        mx_so_far = max(mx_so_far, sum);

        maxSubarrSum[i] = mx_so_far;
        i--; j--;
    }

    maxSubarrSum[i] = max(mx_so_far, prefixSum[j]);      // --> boundary case for 0th index
}


int maxSumTwoNoOverlap(vector<int>& nums, int firstLen, int secondLen) {
    int n = nums.size();
    int ans = 0;

    vector<int> maxSubarrSum1 (n, -1);      // --> contains max sum of every subarray of len1 starting from right of idx i
    vector<int> maxSubarrSum2 (n, -1);      // --> contains max sum of every subarray of len2 starting from right of idx i

    vector<int> prefixSum (n, 0);
    prefixSum[0] = nums[0];

    for(int i=1; i<n; i++) 
        prefixSum[i] = prefixSum[i-1] + nums[i];

    fill_maxSubarrSum (nums, maxSubarrSum1, prefixSum, firstLen, n);
    fill_maxSubarrSum (nums, maxSubarrSum2, prefixSum, secondLen, n);

    /* Iterating for every index and computing sum */

    for(int i=0; i<=n-(firstLen + secondLen); i++){

        /* Checking for subarr1 in left and subarr2 in right */

        int s1 = i==0 ? prefixSum[i+firstLen-1] : prefixSum[i+firstLen-1] - prefixSum[i-1];
        int s2 = maxSubarrSum2[i+firstLen];
        ans = max(ans, s1+s2);

        /* Checking for subarr2 in left and subarr1 in right */

        s1 = i==0 ? prefixSum[i+secondLen-1] : prefixSum[i+secondLen-1] - prefixSum[i-1];
        s2 = maxSubarrSum1[i+secondLen];
        ans = max(ans, s1+s2);
    }

    return ans;
}
```

<br>


## @ Range Queries

### 1. Corporate Flight Bookings
There are n flights that are labeled from 1 to n. You are given an array of flight bookings bookings, where bookings[i] = [firsti, lasti, seatsi] represents a booking for flights firsti through lasti (inclusive) with seatsi seats reserved for each flight in the range. Return an array answer of length n, where answer[i] is the total number of seats reserved for flight i.

Input: bookings = [[1,2,10],[2,3,20],[2,5,25]], n = 5

Output: [10,55,45,25,25]

```cpp
/* 
    Approach based on Rachit's Jain trick 

    For every range query, just add number of seat to v[left] 
    and subtract number of seats from v[right+1]

    After doing this for all queries, compute prefix sum and return the vector
*/

vector<int> corpFlightBookings(vector<vector<int>>& bookings, int n) {
    vector<int> v (n, 0);

    for(auto b : bookings){
        int start = b[0], end = b[1], seats = b[2];

        v[start-1] += seats;    // --> Since Indexing is 0 based
        if(end<n) v[end] -= seats;
    }

    for(int i=1; i<n; i++)
        v[i] += v[i-1];

    return v;
}
```

<br>

### 2. Hotel Bookings Possible
A hotel manager has to process N advance bookings of rooms for next season. His hotel has C rooms. Bookings contain an arrival date and a departure date. He wants to find out whether there are enough rooms in the hotel to satisfy the demand. 

Input: A = [1, 3, 5], B = [2, 6, 8], C = 1, where A is arrival time, B is departure time and C is total number of rooms

Output: False

**Approach 1 : Using range query with csum**

```cpp
Time Complexity : O(max(depart))

bool Solution::hotel(vector<int> &arrive, vector<int> &depart, int K) {
    int n = arrive.size();
    int days = *max_element(depart.begin(), depart.end());

    vector<int> rooms(days+1, 0);

    for(int i=0; i<n; i++){
        rooms[arrive[i]] += 1;
        if(depart[i]<days) rooms[depart[i]] -= 1;
    }

    for(int d=1; d<=days; d++){
        rooms[d] += rooms[d-1];
        if(rooms[d]>K) return false;
    }

    return true;
}
```

**Approach 2 : Using Minheap**

```cpp
Time Complexity : O(nlogn)

bool Solution::hotel(vector<int> &arrive, vector<int> &depart, int K){
    int n = arrive.size();
    vector<pair<int,int>> bookings;
    priority_queue<int, vector<int>, greater<int>> minheap;

    for(int i=0; i<n; i++)
        bookings.push_back({arrive[i], depart[i]});
    
    sort(bookings.begin(), bookings.end());

    for(int i=0; i<n; i++){
        int arv = bookings[i].first, dpr = bookings[i].second; 
        minheap.push(dpr);

        while(!minheap.empty() and minheap.top()<=arv)
            minheap.pop();

        if(minheap.size()>K) return false;
    }

    return true;
}
```

<br>

### 3. Describe the Painting

[Question](https://leetcode.com/problems/describe-the-painting/)

![img](https://assets.leetcode.com/uploads/2021/06/18/1.png)

Input: segments = [[1,4,5],[4,7,7],[1,7,9]]

Output: [[1,4,14],[4,7,16]]

```cpp
vector<vector<long long>> splitPainting(vector<vector<int>>& segments) {

    vector<long long> csum (100002, 0);
    vector<bool> endPoints (100002, false);         // --> used to mark endPoints of all segments (Since there are segmets which have same sum but different mix of colour  

    for(auto s : segments){
        int start = s[0], end = s[1], color = s[2];
        csum[start] += color;
        csum[end] -= color;

        endPoints[start] = endPoints[end] = true;
    }


    int start = 0;
    vector<vector<long long>> res;

    for(int i=1; i<csum.size(); i++){
        csum[i] += csum[i-1];

        if(csum[i]==csum[i-1]){

            if(endPoints[i]){
                res.push_back({start, i, csum[i]});
                start = i;
            }
        }

        else{

            if(csum[i-1]==0)
                start = i;
            else{
                res.push_back({start, i, csum[i-1]});
                start = i;
            }
        }
    }

    return res;
}
```



