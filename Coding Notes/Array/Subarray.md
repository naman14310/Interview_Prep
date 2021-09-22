# Subarrays

<br>

## @ Hashmap and Csum

### 1. Subarray Sum Equals K 
Given an array of integers nums and an integer k, return the total number of continuous subarrays whose sum equals to k.

Approach: Create hashmap and initialize with mp[0] = 1. Compute prefix sum. Iterate over the prefix sum and calculate gain (gain = cs[i]-k) for every iteration. If gain exist in hashmap, add mp[gain] to answer. Add prefix sum of each iteration to hashmap.

[Video Solution](https://www.youtube.com/watch?v=MHocw0bP1rA)

```cpp
int subarraySum(vector<int>& nums, int k) {
    int ans = 0;
    int csum = 0;
    
    unordered_map<int, int> mp;
    mp[0] = 1;
    
    for(int num : nums){
        csum += num;
        int gain = csum - k;
        
        if(mp.find(gain)!=mp.end()) 
            ans += mp[gain];
            
        mp[csum] += 1;
    }
    
    return ans;
}
```

<br>

### 2. Longest subarray with 0 sum
Given an array having both positive and negative integers. Compute the length of the Longest subarray with sum 0.

Input: N = 8, A[] = {15,-2,2,-8,1,7,10,23}

Output: 5

Hint: Cummulative sum repeates if sum of subarray becomes 0 (i.e. gain=0)

```cpp
int maxLen(int A[], int n){
    int ans = 0;
    int csum=0;
    
    unordered_map<int,int> mp;          // --> {csum, first occuring index}
    mp[0] = -1;                         // --> map initialization
        
    for(int i=0; i<n; i++){
        csum += A[i];
        
        /* Whenever we find csum in map, it means we found subarray of sum 0 */
        
        if(mp.find(csum)!=mp.end())
            ans = max(ans, i-mp[csum]);
        else
            mp[csum] = i;
    }
    
    return ans;
}
```

<br>

### 3. Contiguous Array (Tricky)
Given a binary array nums, return the maximum length of a contiguous subarray with an equal number of 0 and 1.

Input: nums = [0,1,1,0,1,1,1]

Output: 4

Hint: Variation of longest subarray with sum 0.

```cpp
int findMaxLength(vector<int>& nums) {
    int ans = 0, csum = 0;
    
    unordered_map<int,int> mp;
    mp[0] = -1;

    for(int i=0; i<nums.size(); i++){
        if(nums[i]==0) csum += -1;
        else csum += 1;

        if(mp.find(csum)!=mp.end())
            ans = max(ans, i-mp[csum]);
        else
            mp[csum] = i;
    }

    return ans;
}
```

<br>

### 4. Longest Subarray Length Having more OneCount
Given an integer array A of size N containing 0's and 1's only.  You need to find the length of the longest subarray having count of 1’s one more than count of 0’s.

Input: A = [0, 1, 1, 0, 0, 1]
 
Output: 5

Hint: Compute csum by incrementing it for 1 and decrementing it for 0. Now, to find longest subarray with more OneCount, find the longest subarray whose sum is 1. Hence problem reduced to subarray sum equals 1.

```cpp
int Solution::solve(vector<int> &A) {
    int ans = 0;
    int csum = 0;

    unordered_map<int, int> mp;
    mp[0] = -1;

    for(int i=0; i<A.size(); i++){
        if(A[i]==0) csum--;
        else csum++;

        int diff = csum-1;              

        if(mp.find(diff)!=mp.end())
            ans = max(ans, i-mp[diff]);
        
        if(mp.find(csum)==mp.end())
            mp[csum] = i;
    }

    return ans;
}
```

<br>

### 5. Count Number of Nice Subarrays (Tricky)
A continuous subarray is called nice if there are k odd numbers on it. Return the number of nice sub-arrays.

Hint: Convert all odd numbers to 1 and all even numbers to 0. Now problem is reduced to number of subarrays having sum equals k.

```cpp
int no_of_subarrays(vector<int> & v, int k){
    int count = 0;
    int csum = 0;

    unordered_map<int,int> mp;
    mp[0] = 1;

    for(int num : v){
        csum += num;

        if(mp.find(csum-k)!=mp.end())
            count+=mp[csum-k];

        mp[csum]++; 
    }

    return count;
}


int numberOfSubarrays(vector<int>& nums, int k) {
    for(int i=0; i<nums.size(); i++){
        if(nums[i]%2==0)
            nums[i]=0;
        else
            nums[i]=1;
    }

    /* Now problem reduced to no. of subarrays whose sum equals to k */

    return no_of_subarrays(nums, k);
}
```

<br>

### 6. Subarray Sums Divisible by K
Given an array nums of integers, return the number of contiguous, non-empty subarrays that have a sum divisible by k.

Hint: Insert remainder instead of csum into hashmap. Also handle negative remainders.

```cpp
int subarraysDivByK(vector<int>& nums, int k) {
    int ans = 0;
    int csum = 0;

    unordered_map<int,int> mp;
    mp[0] = 1;

    for(int num : nums){
        csum += num;

        int rem = csum%k;
        if(rem<0) rem = rem+k;      // --> If remainder becomes negative, convert it into +ve by adding k

        if(mp.find(rem)!=mp.end())
            ans += mp[rem];

        mp[rem]+=1;
    }

    return ans;
}
```

<br>

### 7. Subarray Sum Divisibility by K (Return true/false)
Return true if nums has a continuous subarray of size at least two whose elements sum up to a multiple of k, or false otherwise.

Input: nums = [23,2,6,4,7], k = 6

Output: true

```cpp
bool checkSubarraySum(vector<int>& nums, int k) {
    int n = nums.size();
    if(n<2) return false;

    unordered_map<int, int> mp;
    mp[0] = -1;

    int csum = 0;
    
    for(int i=0; i<n; i++){
        csum += nums[i];
        int rem = csum % k;

        if(mp.find(rem)!=mp.end()){
            if(i-mp[rem]>=2)
                return true;
        }
        
        else mp[rem] = i;
    }

    return false;
}
```

<br>

### 8. Make Sum Divisible by P (Tricky)
Given an array of positive integers nums, remove the smallest subarray (possibly empty) such that the sum of the remaining elements is divisible by p. It is not allowed to remove the whole array. Return the length of the smallest subarray that you need to remove, or -1 if it's impossible.

Input: nums = [6,3,5,2], p = 9

Output: 2

Hint: Find a subarray whose remainder equals to sum % p

```cpp
int minSubarray(vector<int>& nums, int p) {
    long long n = nums.size();

    /* Find total sum of all numbers */

    long long sum = 0;
    for(int num : nums)
        sum += num;

    /* 
        Case 1: If sum < p we need to remove whole array 
        which is not permissible, hence return -1 
    */

    if(sum<p) return -1;


    /* 
        Case 2: If required_rem==0 that means on removing 0 elements 
        we get the desired res, hence so return 0 
    */

    long long required_rem = sum % p;
    if(required_rem==0) return 0;


    /* Else We need to find a subarray whose remainder equals to required_rem (sum % p)  */

    unordered_map<long long,long long> mp;
    mp[0] = -1;

    long long csum = 0;
    long long ans = n;

    for(int i=0; i<n; i++){
        csum += nums[i];
        long long rem = csum % p;

        int target = rem - required_rem;
        if(target<0) target += p;               // --> converting -ve rem to +ve

        if(mp.find(target)!=mp.end())
            ans = min(ans, i-mp[target]);

        mp[rem] = i;
    }

    return ans==n ? -1 : ans;
}
```

<br>

### 9. Subarray with given XOR
Find the total number of subarrays having bitwise XOR of all elements equals to B.

```cpp

/*
            cxor
|<-----------><------------->|
       x              B

    hence, x ^ B = cxor 
        => x = cxor ^ B
*/

int Solution::solve(vector<int> &A, int B) {
    int ans = 0;
    int cxor = 0;

    unordered_map<int, int> mp;
    mp[0] = 1;

    for(int num : A){
        cxor = cxor ^ num;
        int target = cxor ^ B;

        if(mp.find(target)!=mp.end())
            ans += mp[target];

        mp[cxor]++;
    }

    return ans;
}
```

<br>



## @ Tricky Subarrays

### 1. Number of Subarrays with Bounded Maximum
Return the number of (contiguous, non-empty) subarrays such that the value of the maximum array element in that subarray is at least left and at most right.

Approach: Count all valid subarrays ending at an index i, where 0 <= i < n. For a  particular element we can have 3 cases:
1. ele > R --> We will mark this as the most recent invalid index.
2. ele < L --> It's answer will be equal to it's previous element's answer
3. L <= ele <= R --> subarrays ending at ele can be computed as i-last_invalid_index

```cpp
int numSubarrayBoundedMax(vector<int>& nums, int left, int right) {
    int last_invalid_index = -1;
    int ans = 0, prev = 0;

    for(int i=0; i<nums.size(); i++){
        if(nums[i]<left)
            ans += prev;

        else if(nums[i]>right){
            last_invalid_index = i;
            prev = 0;
        }

        else{
            prev = i-last_invalid_index;
            ans += prev;
        }
    }
    return ans;
}
```

<br>

### 2. Arithmetic Slices
An integer array is called arithmetic if it consists of at least three elements and if the difference between any two consecutive elements is the same. Given an integer array nums, return the number of arithmetic subarrays of nums.

Hint: Find count of equal common differences. If count is n then it gives n*(n-1)/2 subarrays.

```cpp
int numberOfArithmeticSlices(vector<int>& A) {
    int ans = 0;
    int i=1;
    
    while(i<A.size()){
        int item = A[i]-A[i-1];
        int count = 0;

        while(i<A.size() && A[i]-A[i-1]==item){
            count++;
            i++;
        }
        
        if(count>=2) 
            ans += (count*(count-1))/2;
    }
    
    return ans;
}
```

<br>

### 3. Shortest Unsorted Continuous Subarray
Given an integer array nums, you need to find one continuous subarray that if you only sort this subarray in ascending order, then the whole array will be sorted in ascending order.Return the shortest such subarray and output its length.

Approach:
1. Find leftmost flipped item and assign its index to left. If left = -1 => return 0
2. Find rightmost flipped item and assign its index to right
3. Find max and min inside the range left to right
4. Iterate from 0 to left => if any item is greater then min then reassign its index to left
5. Iterate from nums.size() to right => if any item is smaller then max then reassign its index to right
6. Return right-left+1

```cpp
int findUnsortedSubarray(vector<int>& nums) {
    int left=-1, right=-1;
    for(int i=0; i<nums.size()-1; i++){
        if(nums[i]>nums[i+1]){
            left = i;
            break;
        }
    }
    if(left==-1) return 0;

    for(int i=nums.size()-1; i>=1; i--){
        if(nums[i]<nums[i-1]){
            right = i;
            break;
        }
    }

    int mn = INT_MAX, mx = INT_MIN;
    for(int i=left; i<=right; i++){
        mn = min(mn, nums[i]);
        mx = max(mx, nums[i]);
    }

    for(int i=0; i<left; i++){
        if(nums[i]>mn) {
            left = i;
            break;
        }
    }
    
    for(int i=nums.size()-1; i>right; i--){
        if(nums[i]<mx){
            right = i;
            break;
        }
    }
    return right-left+1;
}
```

<br>

### 4. Flip (IB)
You are given a binary string A. In a single operation, you can choose two indices L and R and flip the characters between them. Perform ATMOST one operation such that in final string number of 1s is maximised. Return the lexicographically smallest such pair of L and R.

NOTE: Pair (a, b) is lexicographically smaller than pair (c, d) if a < c or, if a == c and b < d.

Input: A = "010"

Output: [1, 1]

Hint: Iterate only one time and maintain a cnt variable to track 1's and 0's cnt. Start new subaray if cnt becomes negative.

```cpp
vector<int> Solution::flip(string A) {
    int n = A.size();
    vector<int> ans;

    /* 
        zero will increment the cnt while 1 will decrement the cnt
        Now, our goal is to maximize this cnt
    */

    int cnt = 0, maxChange = 0;
    int start = 0;        

    for(int i=0; i<n; i++){
        if(A[i]=='1') cnt--;
        else cnt++;

        /* 
            If cnt<0, means we get more number of 1's so simple start
            new subarray from next index and reset cnt
        */

        if(cnt<0){
            start = i+1;
            cnt = 0;
        }

        /*
            If len of cnt is greater the maxChange so far
            then mark curr subarray till curr index as our answer
            and update maxChange var
        */

        else if(cnt>maxChange){
            ans = {start+1, i+1};       // --> follows 1 based indexing
            maxChange = cnt;
        }
    }

    return ans;
}
```

<br>

### 5. Shortest Subarray with Sum at Least K
Given an integer array nums and an integer k, return the length of the shortest non-empty subarray of nums with a sum of at least k. If there is no such subarray, return -1.

**Approach 1: Brute Force - O(n2)**

```cpp
/* Brute Force Solution - O(n2) */

int shortestSubarray(vector<int>& nums, int k) {

    /* Calculating prefixSum */

    for(int i=1; i<nums.size(); i++)
        nums[i]+=nums[i-1];

    int ans = INT_MAX;

    for(int j=0; j<nums.size(); j++){
        for(int i=0; i<=j; i++){

            int sum = i==0 ? nums[j] : nums[j] - nums[i-1];

            if(sum>=k and j-i+1 < ans)
                ans = j-i+1;
        }
    }

    return ans==INT_MAX ? -1 : ans;
}
```
