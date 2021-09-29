# Arrays

<br>

## @ Easy

### 1. Minimum Moves to Equal Array Elements II
Given an integer array nums of size n, return the minimum number of moves required to make all array elements equal.

Hint: Convert each no. to median

```cpp
int minMoves2(vector<int>& nums) {
    int n = nums.size();
    sort(nums.begin(), nums.end());

    int median = nums[n/2];
    int ans = 0;
    for(int i : nums)
        ans += abs(i-median);

    return ans;
}
```

<br>

### 2. Find Pivot Index (Left sum == Right sum)
Given an array of integers nums, calculate the pivot index of this array. The pivot index is the index where the sum of all the numbers strictly to the left of the index is equal to the sum of all the numbers strictly to the index's right.

Input: nums = [2,1,-1]

Output: 0

Hint: Find totalSum and then start iterating from left to right computing lsum on the go. For each lsum compute rsum using (sum-lsum-nums[i]). If both lsum and rsum are equal return i.

```cpp
int pivotIndex(vector<int>& nums) {
	int sum = 0;
	for(int i : nums)
		sum += i;

	int lsum = 0;

	for(int i=0; i<nums.size(); i++){
		int rsum = sum-lsum-nums[i];

		if(lsum==rsum) return i;

		lsum += nums[i];
	}

	return -1;
}
```

<br>

### 3. Global and Local Inversions
You are given an integer array nums of length n which represents a permutation of all the integers in the range [0, n - 1]. 
1. The number of global inversions is the number of the different pairs (i, j) where: 0 <= i < j < n and nums[i] > nums[j]
2. The number of local inversions is the number of indices i where: 0 <= i < n - 1 and nums[i] > nums[i + 1]

Return true if the number of global inversions is equal to the number of local inversions.

```cpp
bool isIdealPermutation(vector<int>& A) {
    bool flag = false;
	
    for(int i=0; i<A.size(); i++){
        if(abs(A[i]-i)>1) 
			return false;
    }
  
    return true;
}
```

<br>

### 4. Max Min (In Minimum Comparisions)
Given an array A of size N. You need to find the sum of Maximum and Minimum element in the given array. You should make minimum number of comparisons.

Hint: Use Divide and Conquer and recurse for left and right half of array.

```cpp

pair<int,int> findMinMax (vector<int> & A, int start, int end){
    if(start==end)
        return {A[start], A[start]}; 

    int mid = start+(end-start)/2;

    auto left = findMinMax(A, start, mid);
    auto right = findMinMax(A, mid+1, end);

    int mn = min(left.first, right.first);
    int mx = max(left.second, right.second);

    return {mn, mx};
}


int Solution::solve(vector<int> &A) {
    int n = A.size();
    auto res = findMinMax(A, 0, n-1);
    return res.first + res.second;
}
```

<br>

### 5. Rotate Array (Awesome 3 line reverse logic)

```cpp
/* ROTATE RIGHT */

void rotate_right(vector<int>& nums, int k) {
    k = k%nums.size();
    reverse(nums.begin(), nums.end());
    reverse(nums.begin(), nums.begin()+k);
    reverse(nums.begin()+k, nums.end());
}

/* ROTATE LEFT */

void rotate_left(vector<int>& nums, int k) {
    k = k%nums.size();
    k = nums.size()-k;
    reverse(nums.begin(), nums.end());
    reverse(nums.begin(), nums.begin()+k);
    reverse(nums.begin()+k, nums.end());
}

```

<br>

### 6. Increasing Triplet Subsequence
Given an integer array nums, return true if there exists a triple of indices (i, j, k) such that i < j < k and nums[i] < nums[j] < nums[k]. If no such indices exists, return false.

Hint: Maintain two variables for storing smallest and second smallest elements so far.

```cpp
bool increasingTriplet(vector<int>& nums) {
    int len = nums.size();
    if(len<3) return false;

    int n1 = nums[0], n2 = INT_MAX;
    for(int i=1; i<len; i++){
        if(nums[i]<=n1) 
            n1 = nums[i];
        else if(nums[i]<=n2) 
            n2 = nums[i];
        else 
            return true;
    }
    return false;
}
```

<br>

### 7. Highest Product (IB)
Given an array A, of N integers. Return the highest product possible by multiplying 3 numbers from the array.

```cpp
int Solution::maxp3(vector<int> &A) {
    int n = A.size();
    sort(A.begin(), A.end());

    if(A[n-1]<=0) 
        return A[n-1]*A[n-2]*A[n-3];
    else
        return A[n-1] * max(A[0]*A[1], A[n-2]*A[n-3]);
}
```

<br>

### 8. Count Number of Teams
There are n soldiers standing in a line. Each soldier is assigned a unique rating value. Choose 3 soldiers with index (i, j, k) with rating (rating[i], rating[j], rating[k]). A team is valid if for all i, j, k (0 <= i < j < k < n): 
1. (rating[i] < rating[j] < rating[k]) or, 
2. (rating[i] > rating[j] > rating[k]) 

Return the number of teams you can form given the conditions. (soldiers can be part of multiple teams).

Input: rating = [2,5,3,4,1]

Output: 3

Hint: For each soldier, count how many soldiers on the left and right have less and greater ratings. This soldier can form less[left] * greater[right] + greater[left] * less[right] teams.

```cpp
int numTeams(vector<int>& rating) {
	int n = rating.size();
	int ans = 0;

	for(int j=1; j<n-1; j++){

		int ls=0, lg=0, rs=0, rg=0;

		for(int i=0; i<j; i++){
			if(rating[i]<rating[j]) ls++;
			else if(rating[i]>rating[j]) lg++;
		}

		for(int k=j+1; k<n; k++){
			if(rating[k]<rating[j]) rs++;
			else if(rating[k]>rating[j]) rg++;
		}

		ans += (ls*rg) + (lg*rs);
	}

	return ans;
}
```

<br>



## @ Medium

### 1. Least Greater Element on Right
Replace every element with the least greater element on its right

Hint: Iterate from right to left and use Set data structure

```cpp
vector<int> leastGreaterElement(vector<int> &arr) {
    int n = arr.size();
    set<int> s;
    s.insert(arr[n-1]);
    arr[n-1] = -1;

    for(int i=n-2; i>=0; i--){
        int val = arr[i];
        auto ub = s.upper_bound(arr[i]);
        
        if(ub==s.end())
            arr[i] = -1;
        else
            arr[i] = *ub;
        
        s.insert(val);
    }
    
    return arr;
}
```

<br>

### 2. Maximum Sum Triplet
Given an array A containing N integers. You need to find the maximum sum of triplet ( Ai + Aj + Ak ) such that 0 <= i < j < k < N and Ai < Aj < Ak. If no such triplet exist return 0.

Input: A = [2, 5, 3, 1, 4, 9]

Output: 16

Hint: Assume each element as middle element of triplet and try to maximize the sum using some pre-computation of max values on right side and binary search on left side.

```cpp
#include<bits/stdc++.h>
using namespace std;

int Solution::solve(vector<int> &A) {
    int n = A.size();
    int ans = 0;
    
    /* precomputing rightMax for every index */

    vector<int> rmax (n, -1);
    int mx = A[n-1];

    for(int i=n-2; i>=0; i--){
        mx = max(mx, A[i]);
        if(mx>A[i]) rmax[i] = mx;
    }

    /* Set will be used to find greatest number smaller then A[i] in left side */
    
    set<int> s;
    s.insert(A[0]);

    for(int i=1; i<n-1; i++){
        auto lb = s.lower_bound(A[i]);

        if(lb!=s.begin() and rmax[i]!=-1){
            int l = *prev(lb);
            int temp = l+A[i]+rmax[i];
            ans = max(ans, temp);
        }

        s.insert(A[i]);
    }
    
    return ans;
}
```

<br>

### 3. Product of Array Except Self
Given an integer array nums, return an array answer such that answer[i] is equal to the product of all the elements of nums except nums[i].

Hint: Create left and right array to store cummulative product of all left elements and right elements in array.

```cpp
vector<int> productExceptSelf(vector<int>& nums) {
    int len = nums.size();
    vector<int> left (len, 1);
    vector<int> right (len,1);

    for(int i=1; i<len; i++)
        left[i] = left[i-1] * nums[i-1];
    
    for(int i=len-2; i>=0; i--)
        right[i] = right[i+1] * nums[i+1];
    
    for(int i=0; i<len; i++)
        nums[i] = left[i] * right[i];
    
    return nums;
}
```

<br>

### 4. Sort Colors (Dutch National Flag algo - 3 pointer solution)
Given an array nums with n objects colored red, white, or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

Hint: Settle 0 at the left end, 2 at the right end => 1 will automatically settle at middle.

```cpp
void sortColors(vector<int>& nums) {
	int low=0, mid=0, high=nums.size()-1;

	while(mid<=high){

		if(nums[mid]==0 and mid!=low){
			swap(nums[low], nums[mid]);
			low++;
		}
		else if(nums[mid]==2){
			swap(nums[high], nums[mid]);
			high--; 
		}
		else
			mid++;
	}
}
```

<br>

### 5. Next Permutation

Approach:
1. Iterate from right to left and find rightmost flipped bit (i.e. nums[i]>nums[i-1]) and assign its index to pos
2. If pos==0 => reverse whole array and return
3. Else Iterate from n-1 to pos and find the item (say nums[i]) just greater then nums[pos-1]. Then swap nums[i], nums[pos] and break
4. Reverse array from nums.begin()+pos to nums.end()

```cpp
void nextPermutation(vector<int>& nums) {
    int n = nums.size();
    int pos = n-1;
    while(pos>=1){
        if(nums[pos]>nums[pos-1]) break;
        pos--;
    }

    if(pos==0){
        reverse(nums.begin(), nums.end());
        return;
    }

    for(int i=n-1; i>=pos; i--){
        if(nums[i]>nums[pos-1]){
            swap(nums[i], nums[pos-1]);
            break;
        }
    }
    reverse(nums.begin()+pos, nums.end());    
}
```

<br>

### 6. Find Permutation
Given a positive integer n and a string s consisting only of letters D or I, you have to find any permutation of first n positive integer that satisfy the given input string. D means the next number is smaller, while I means the next number is greater.

Input 1: n = 3, s = ID

Return: [1, 3, 2]

Hint: If next number in greater ('I') then full current smallest possible and if next number is smaller ('D') then fill current largest possible.

```cpp
vector<int> Solution::findPerm(const string A, int B) {
    int curr_smallest = 1;
    int curr_largest = B;
    vector<int> ans;
	
    for(int i=0; i<A.size(); i++){
        
        if(A[i]=='I'){
            ans.push_back(curr_smallest);
            curr_smallest++;
        }
        else{
            ans.push_back(curr_largest);
            curr_largest--;
        }
    }
    
    ans.push_back(curr_smallest); // here curr_smallest == curr_largest
    return ans;
}
```

<br>

### 7. Best Sightseeing Pair - O(n)|O(1)
You are given an integer array values where values[i] represents the value of the ith sightseeing spot. The score of a pair (i < j) of sightseeing spots is values[i] + values[j] + i - j. Return the maximum score of a pair of sightseeing spots.

Input: values = [8,1,5,2,6]

Output: 11

Hint: Use variable max_score_on_right for storing max value of value[j]-j on right side for every index. 

```cpp
int maxScoreSightseeingPair(vector<int>& values) {
	int n = values.size();
	int max_score_on_right = values.back()-(n-1);
	int ans = 0;

	for(int i=n-2; i>=0; i--){
		ans = max(ans, values[i]+i+max_score_on_right);
		max_score_on_right = max(max_score_on_right, values[i]-i);
	}

	return ans;
}
```

<br>

### 8. Max Distance
Given an array A of integers, find the maximum of j - i subjected to the constraint of A[i] <= A[j].

Input: A = [3, 5, 4, 2]

Output: 2

Hint: Create new vector of pair {value, index}. Sort this vector and iterate from right to left. Compute right max index in every iteration and update ans.

```cpp
int Solution::maximumGap(const vector<int> &A) {
    int n = A.size();
    vector<pair<int,int>> v;

    for(int i=0; i<n; i++)
        v.push_back({A[i], i});
    
    sort(v.begin(), v.end());

    int maxRightIdx = v.back().second;
    int ans = 0;

    for(int i=n-2; i>=0; i--){

        if(maxRightIdx>v[i].second)
            ans = max(maxRightIdx - v[i].second, ans);
        
        maxRightIdx = max(maxRightIdx, v[i].second);
    }

    return ans;
}
```

<br>

### 9. Flip String to Monotone Increasing
A binary string is monotone increasing if it consists of some number of 0's (possibly none), followed by some number of 1's (also possibly none). You are given a binary string s. You can flip s[i] changing it from 0 to 1 or from 1 to 0. Return the minimum number of flips to make s monotone increasing.

Input: s = "00011000"

Output: 2

**Approach 1 : O(n) | O(n)**

Precompute one cnt on left side of every index and zero cnt on right side of every index (including curr idx). Then number of flips on each index = cnt of one on left side of idx + cnt of zero on right side of idx (including idx)


**Approach 2 : O(n) | O(1)**

Precompute zeroCount of hole array. Then iterate from left to right and increment/decrement zeroCount and oneCount in each iteration. Update flips in every iteration.

```cpp
int minFlipsMonoIncr(string & s) {
    int n = s.length();
    int zero_cnt = count(s.begin(), s.end(), '0');

    int one_cnt = 0;
    int flips = zero_cnt;

    for(int i=0; i<n; i++){

        if(s[i]=='1')
            one_cnt++;
        else
            zero_cnt--;

        flips = min(flips, one_cnt+zero_cnt);
    }

    return flips;
}
```

<br>

### 10. Array Nesting
You are given an integer array nums of length n where nums is a permutation of the numbers in the range [0, n - 1]. You should build a set s[k] = {nums[k], nums[nums[k]], nums[nums[nums[k]]], ... } subjected to the following rule:
1. The first element in s[k] starts with the selection of the element nums[k] of index = k.
2. The next element in s[k] should be nums[nums[k]], and then nums[nums[nums[k]]], and so on.
3. We stop adding right before a duplicate element occurs in s[k].

Return the longest length of a set s[k].

Input: nums = [5,4,0,3,1,6,2]

Hint: Update vis index in nums to -1 and avoid using extra array for vis purpose.

Output: 4

```cpp
int traverse (vector<int> & nums, int val){
	if(nums[val]==-1) return 0;

	int next = nums[val];
	nums[val]=-1;

	return 1 + traverse(nums, next);
}

int arrayNesting (vector<int>& nums){
	int ans = 1;

	for(int i=0; i<nums.size(); i++){
		if(nums[i]!=-1)
			ans = max(ans, traverse(nums, i));
	}

	return ans;
}
```

<br>

### 11. Partition Array into Disjoint Intervals
Given an array nums, partition it into two (contiguous) subarrays left and right so that:

1. Every element in left is less than or equal to every element in right.
2. left and right are non-empty.
3. left has the smallest possible size.

Return the length of left after such a partitioning.  It is guaranteed that such a partitioning exists.

Hint: Use two vectors for storing max_left_so_far and min_right_so_far. Iterate over array and find index where max_left <= min_right.

```cpp
int partitionDisjoint(vector<int>& nums) {
	int n = nums.size();
	vector<int> mn (n, INT_MAX);
	vector<int> mx (n, INT_MIN);

	mx[0] = nums[0];
	mn[n-1] = nums[n-1];

	for(int i=1; i<n; i++) 
	    mx[i] = max(mx[i-1], nums[i]);

	for(int i=n-2; i>=0; i--)
	    mn[i] = min(nums[i], mn[i+1]);

	for(int i=0; i<n-1; i++)
	    if(mx[i]<=mn[i+1])  return i+1;

	return -1;
}
```

<br>

### 12. 4Sum II
Given four integer arrays nums1, nums2, nums3, and nums4 all of length n, return the number of tuples (i, j, k, l) such that: 0 <= i, j, k, l < n and nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0

Hint: Use two hashmaps for storing sum of A[i]+B[i] and C[i]+D[i]

```cpp
int fourSumCount(vector<int>& A, vector<int>& B, vector<int>& C, vector<int>& D) {
    int len = A.size();
    int count = 0; 
    unordered_map<int, int> mp1;
    unordered_map<int, int> mp2;

    for(int i=0; i<len; i++){
        for(int j=0; j<len; j++){
			mp1[A[i]+B[j]]++;
        }
    }
    for(int i=0; i<len; i++){
        for(int j=0; j<len; j++){
			mp2[C[i]+D[j]]++;
		}
    }
    for(auto p1 : mp1){
        int nsum = -1*p1.first;
        int freq = p1.second;
        if(mp2.find(nsum)!=mp2.end())
            count+= freq*mp2[nsum];       
    }
    return count;
}
```

<br>

### 13. Pairs of Songs With Total Durations Divisible by 60
You are given a list of songs where the ith song has a duration of time[i] seconds. Return the number of pairs of songs for which their total duration in seconds is divisible by 60. Formally, we want the number of indices i, j such that i < j with (time[i] + time[j]) % 60 == 0.

Hint: Play with remainders

```cpp
int numPairsDivisibleBy60(vector<int>& time) {
    vector<int> rem(60, 0);
    int ans = 0;
    for(int i=0; i<time.size(); i++){
        int tm = time[i];
        ans+= rem[(60-tm%60)%60];
        rem[tm%60]++;
    }
    return ans;
}
```

<br>

### 14. Maximum Swap
You are given an integer num. You can swap two digits at most once to get the maximum valued number. Return the maximum valued number you can get.

Input: num = 2736

Output: 7236

Hint: At each digit, if there is a larger digit that occurs later, we want the swap it with the largest such digit that occurs farthest.

```cpp
    int maximumSwap(int num) {
        string nums = to_string(num);
        int n = nums.size(); 
        
	/* It will store index of the greatest element on right */
	
        vector<int> mx (n, n-1);        
        
        for(int i=n-2; i>=0; i--){
            
            if(nums[i]>nums[mx[i+1]])
                mx[i] = i;
            else
                mx[i] = mx[i+1];
        }
        
        
        for(int i=0; i<n; i++){
            
            if(nums[i]<nums[mx[i]]){
                swap(nums[i], nums[mx[i]]);
                return stoi(nums);
            }
        }
        
        return stoi(nums);
    }
```

<br>

### 15. Beautiful Arrangement II
Given two integers n and k, construct a list answer that contains n different positive integers ranging from 1 to n and obeys the following requirement: Suppose this list is answer = [a1, a2, a3, ... , an], then the list [|a1 - a2|, |a2 - a3|, |a3 - a4|, ... , |an-1 - an|] has exactly k distinct integers.

Hint: Fill in cyclic pattern

```cpp
vector<int> constructArray(int n, int k) {
    vector<int> res;
    int i=1, j=i+k;

    while(i<=j){
        res.push_back(i);
        i++;
        if(i>j) break;
        res.push_back(j);
        j--;
    }

    for(int itr=k+2; itr<=n; itr++)
        res.push_back(itr);
   
    return res;
}
```

<br>

### 16. Minimum Numbers of Function Calls to Make Target Array (Reverse Logic)
Your task is to form an integer array nums from an initial array of zeros arr that is the same size as nums. Either we can increment any element by 1 or we can multiply any element by 2.

Hint: Run a while loop till we get all zeros. decrement all odd numbers by one, now all numbers becomes even, divide all by 2. Repeat these steps to get all zeros in min steps.

```cpp
bool terminate(vector<int> v){
    for(auto i : v)
        if(i!=0)
            return false;

    return true;
}

int minOperations(vector<int>& nums) {
    int ans = 0;
    int n = nums.size();

    while(!terminate(nums)){
        for(int i=0; i<n; i++){
            if(nums[i]%2==1){
                nums[i]--;
                ans++;
            }
        }

        if(terminate(nums)) break;

        for(int i=0; i<n; i++)
            nums[i]/=2;

        ans++;
    }

    return ans;
}
```

<br>

### 17. Maximum Absolute Difference
You are given an array of N integers. Return maximum value of f(i, j) for all 1 ≤ i, j ≤ N. f(i, j) is defined as |A[i] - A[j]| + |i - j|, where |x| denotes absolute value of x.

Input: A = [1, 3, -1]

Output: 5

```cpp
int Solution::maxArr(vector<int> &A) {
    int ans = 0;

    int max1 = INT_MIN,  max2 = INT_MIN, max3 = INT_MIN, max4 = INT_MIN;
    int min1 = INT_MAX, min2 = INT_MAX, min3 = INT_MAX, min4 = INT_MAX;
    
    for(int i=0; i<A.size(); i++){
        
        /* mod can be opened in 4 ways */

        max1 = max(max1, A[i] + i);
        max2 = max(max2, A[i] - i);
        max3 = max(max3, -A[i] + i);
        max4 = max(max4, -A[i] - i);
        
        min1 = min(min1, A[i] + i);
        min2 = min(min2, A[i] - i);
        min3 = min(min3, -A[i] + i);
        min4 = min(min4, -A[i] - i);
    }
    
    ans = max(ans, max1-min1);
    ans = max(ans, max2-min2);
    ans = max(ans, max3-min3);
    ans = max(ans, max4-min4);
    
    return ans;
}
```

<br>

### 18. Maximum Length of Subarray With Positive Product
Given an array of integers nums, find the maximum length of a subarray where the product of all its elements is positive. Return the maximum length of a subarray with positive product (Zero not considered).

Input: nums = [-1,-2,-3,0,1]

Output: 2

Hint: If negative signs are even, then whole subarray will give positive product. Else we have two choices, Either take subarray after first neg sign, or Or take subarray before last neg sign.

```cpp
    int getTempAns (int first_neg_idx, int last_neg_idx, int neg_cnt, int prev, int i){
        
        /* If negative sign are even, then we can take whole subarray */
        
        if(neg_cnt%2==0) return i-prev;
        
        /* Else we have two choices */
        
        int after_first_neg = i-first_neg_idx-1;        // --> Either take subarray after first neg sign
        int before_last_neg = last_neg_idx-prev;        // --> Or take subarray before last neg sign
        
        return max(after_first_neg, before_last_neg);
    }
    
    
    int getMaxLen(vector<int>& nums) {
        int n = nums.size();
        
        int i=0;
        int ans = 0;

        int first_neg_idx = -1;
        int last_neg_idx = -1;
        int neg_cnt = 0;
        int prev = 0;
        
        /* Iterate for all numbers */
        
        while(i<n){
            
            /* If curr_num is neg then update first and last neg index and increment neg_cnt */
            
            if(nums[i]<0){
                
                if(first_neg_idx==-1)
                    first_neg_idx = i;
                
                last_neg_idx = i;
                neg_cnt++;
            }
            
            /* Else if curr_cum is zero, process prev subarray and reset variables for next subarray */
            
            else if(nums[i]==0){
                ans = max(ans, getTempAns(first_neg_idx, last_neg_idx, neg_cnt, prev, i));
                
                first_neg_idx = -1;
                last_neg_idx = -1;
                neg_cnt = 0;
                prev = i+1;
            }
            
            i++;
        }
        
        ans = max(ans, getTempAns(first_neg_idx, last_neg_idx, neg_cnt, prev, i));

        return ans;
    }
```

<br>


## @ Hard

### 1. Trapping Rain Water
Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

![TRP](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)

Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]

Output: 6

Approach:
1. Create two arrays maxL, maxR
2. maxL will contains max height in left so far. maxR will contain max height in right so far
3. Water stored at index i can be computed as max(0, min(maxL[i], maxR[i])-height[i])
4. Add water stored at all indexes and return their sum

```cpp
int trap(vector<int>& height) {
    int len = height.size();
    if(len<3) return 0; 
    vector<int> maxL (len, 0); maxL[0] = height[0];
    vector<int> maxR(len, 0); maxR[len-1] = height[len-1];

    for(int i=1; i<len; i++){
        maxL[i] = max(maxL[i-1], height[i]);
    }

    for(int i=len-2; i>=0; i--){
        maxR[i] = max(maxR[i+1], height[i]);
    }

    int water = 0;
    for(int i=0; i<len; i++){
        water+= max(0,min(maxL[i], maxR[i]) - height[i]);
    }
    return water;
}
```

<br>

### 2. Longest Consecutive Sequence (Tricky)
Given an unsorted array of integers nums, return the length of the longest consecutive elements sequence.

Hint: Create hashmap and find all those nums whose nums-1 doesn't exist in the hashmap. 

```cpp
int longestConsecutive(vector<int>& nums) {
    if(nums.size()<=1) return nums.size();
    unordered_map<int, bool> mp;

    for(int i : nums) mp[i] = true;

    for(auto p : mp){
        int num = p.first;
        if(mp.find(num-1)!=mp.end()) mp[num] = false;
    }

    int ans = 1;
    for(auto p : mp){
        if(p.second){
            int num = p.first;
            int count=1;
            while(mp.find(num+1)!=mp.end()){
                count++;
                num+=1;
            }
            ans = max(ans, count);
        }
    }
    return ans;
}
```

<br>

### 3. Maximum Gap (Buckets and Pigeon Hole)
Given an integer array nums, return the maximum difference between two successive elements in its sorted form. If the array contains less than two elements, return 0. You must write an algorithm that runs in linear time and uses linear extra space.

Input: nums = [3,6,9,1]

Output: 3

Approach:
```
Eg:  3 .... 6 .... 9 .... 1

Let n elements --> (n-1) gaps

Step 1: Find avg_gap_size

    avg_gap = range/total_gaps = ceil of [(max-min)/total_gaps]

Step 2: Create (n-1) buckets and put all elements in buckets by using following formula

    bucket = (num-min)/avg_gap

Step 3: Iterate over all buckets and find diff between min-max of two consecutive buckets 
```

```cpp
int maximumGap(vector<int>& nums) {
	int n = nums.size();
	if(n<2) return 0;
	if(n==2) return abs(nums[0]-nums[1]);

	int total_gaps = n-1;

	int mn = *min_element(nums.begin(), nums.end());
	int mx = *max_element(nums.begin(), nums.end());

	if(mx==mn) return 0;

	int avg_gap = ceil(((double)mx-(double)mn)/(double)total_gaps);


	/* Create buckets for storing min and max number of that bucket */

	vector<pair<int,int>> bucket (n-1, {INT_MAX, INT_MIN});

	for(int num : nums){
		int idx = (num-mn)/avg_gap;
		if(idx==n-1) idx--;                     // --> Corner case (100, 3, 2, 1) 

		bucket[idx].first = min(bucket[idx].first, num);
		bucket[idx].second = max(bucket[idx].second, num);
	}


	/* Iterate over all buckets and find max gap between two consecutive buckets */ 

	int prev_max = bucket[0].second;
	int maxGap = 0;

	for(int i=1; i<bucket.size(); i++){

		if(bucket[i].first==INT_MAX) continue;

		maxGap = max(maxGap, bucket[i].first - prev_max);
		prev_max = bucket[i].second;            
	}

	return maxGap;
}
```

<br>

### 4. Candy
There are n children standing in a line. Each child is assigned a rating value given in the integer array ratings. You are giving candies to these children subjected to the following requirements:

1. Each child must have at least one candy.
2. Children with a higher rating get more candies than their neighbors.

Return the minimum number of candies you need to have to distribute the candies to the children.

Approach:
1. Create a vector candies and initialize it with 1.
2. Iterate from 1 to n-1 --> If ratings[i] > ratings[i-1] fill it with candies[i-1]+1
3. Iterate from n-2 to 0 --> It ratings[i] > ratings[i+1], fill it with max(candies[i], candies[i+1]+1)
4. Return sum of candies array

```cpp
int candy(vector<int>& ratings) {
	int n = ratings.size();
	if(n==1) return 1;

	vector<int> candies (n, 1);

	for(int i=1; i<n; i++)    
		if(ratings[i]>ratings[i-1])
			candies[i] = candies[i-1]+1;

	for(int i=n-2; i>=0; i--)
		if(ratings[i]>ratings[i+1])
			candies[i] = max(candies[i], candies[i+1]+1);

	int sum = 0;
	for(int c : candies)
		sum += c;

	return sum;
}
```

<br>

### 5. Longest Mountain in Array
An array arr is a mountain array if and only if arr.length >= 3 and, There exists some index i (0-indexed) with 0 < i < arr.length - 1 such that:
1. arr[0] < arr[1] < ... < arr[i - 1] < arr[i]
2. arr[i] > arr[i + 1] > ... > arr[arr.length - 1]

Given an integer array arr, return the length of the longest subarray, which is a mountain. Return 0 if there is no mountain subarray.

Input: arr = [2,1,4,7,3,2,5]

Output: 5

```cpp
int longestMountain(vector<int>& arr) {
	int n = arr.size();
	if(n<3) return 0;

	int ans = 0;

	vector<int> inc (n, 1);
	vector<int> dec (n, 1);

	for(int i=1; i<n; i++){   
		if(arr[i]>arr[i-1]) 
			inc[i] = 1+inc[i-1];
	}    

	for(int i=n-2; i>=0; i--){
		if(arr[i]>arr[i+1]) 
			dec[i] = 1+dec[i+1];
	}

	for(int i=0; i<n; i++){
		if(inc[i]>1 and dec[i]>1)           // --> Will only consider as a mountain when it has its both arms
			ans = max(ans, inc[i]+dec[i]-1);
	}

	return ans;
}
```

<br>

### 6. Smallest Range - O(nlogn)
Given an array A of integers, for each integer A[i] we need to choose either x = -K or x = K, and add x to A[i] (only once). After this process, we have some array B. Return the smallest possible difference between the maximum value of B and the minimum value of B.

Input: nums = [1,3,6], k = 3

Output: 3

[Explaination](https://leetcode.com/problems/smallest-range-ii/discuss/173377/C%2B%2BJavaPython-Add-0-or-2-*-K)

```cpp
int smallestRangeII(vector<int>& nums, int k) {
	int n = nums.size();
	sort(nums.begin(), nums.end());

	/* Instead of adding +k and -k to elements, we will add 0 and 2*k to elements */

	int ans = nums[n-1] - nums[0];      // --> This will be the max possible difference

	/* 
		Now we will iterate from left to right and start adding 2*k 
		so as to minimize the differences between max and min 
	*/

	for(int i=0; i<n-1; i++){

		/* Since we incremented curr_val so it can be the candidate to become max */

		int mx = max(nums[n-1], nums[i]+(2*k));     

		/* Since we incremented all prev elements before i, so nums[0]+(2*k) and nums[i+1] are the candidates for min */

		int mn = min(nums[i+1], nums[0]+(2*k));

		ans = min(ans, mx-mn);
	}

	return ans;
}
```

<br>

### 7.  Wiggle Subsequence (O(n) Approach) - Tricky
A wiggle sequence is a sequence where the differences between successive numbers strictly alternate between positive and negative. The first difference (if one exists) may be either positive or negative. Given an integer array nums, return the length of the longest wiggle subsequence of nums.

Input: nums = [1,17,5,10,13,15,10,5,16,8]

Output: 7

```cpp
/*
	pattern = 0 --> neutral
	pattern = 1 --> pos slope
	pattern = -1 --> neg slope
*/

int wiggleMaxLength(vector<int>& nums) {
	int n = nums.size();
	if(n==1) return 1;

	int prev = nums[0];
	int pattern = 0;
	int ans = 1;

	for(int i=1; i<n; i++){

		/* If curr pattern is increasing and prev pattern was not increasing, we will update the answer and pattern */ 

		if(nums[i]>prev and pattern!=1){
			ans++;
			pattern = 1;
		}

		/* If curr pattern is decreasing and prev pattern was not decreasing, we will update the answer and pattern */ 

		else if(nums[i]<prev and pattern!=-1){
			ans++;
			pattern = -1;
		}

		prev = nums[i];    // --> prev will get updated everytime
	}

	return ans;
}
```

