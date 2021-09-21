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

### 4. Count Number of Nice Subarrays (Tricky)
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

### 5. Subarray Sums Divisible by K
Given an array nums of integers, return the number of contiguous, non-empty subarrays that have a sum divisible by k.

Hint: Insert remainder instead of csum into hashmap. Also handle negative numbers.

```cpp
int subarraysDivByK(vector<int>& nums, int k) {
    int ans = 0;
    int csum = 0;

    unordered_map<int,int> mp;
    mp[0] = 1;

    for(int num : nums){

        /* 
            If numbers are negative then convert them to lowest positive num 
            which will also generate same remainder 
        */

        if(num<0)
            num = (num%k)+k;

        csum += num;
        int rem = csum % k;         // --> here we compute rem instead of gain

        if(mp.find(rem)!=mp.end())
            ans += mp[rem];

        mp[rem]+=1;                 // --> and we will insert rem instead of csum
    }

    return ans;
}
```

<br>



### 1. Flip (IB)

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
