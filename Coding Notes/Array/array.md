# Arrays

## Easy

#### 1. Duplicate Zeros
Given a fixed length array arr of integers, duplicate each occurrence of zero, shifting the remaining elements to the right. Note that elements beyond the length of the original array are not written.

Hint: Start shifting from back

```cpp
void duplicateZeros(vector<int>& arr) {
    int zc = 0;
    for(int i=0; i<arr.size(); i++)
        if(arr[i]==0) 
            zc++;

    int i=arr.size()-1;

    while(i>=0){
      if(arr[i]==0){
          zc--;
          int pos = i+zc;
          if(pos<arr.size()) arr[pos] = arr[i];
          if(pos+1<arr.size()) arr[pos+1] = 0;
      }
      else{
          int pos = i+zc;
          if(pos<arr.size()) arr[pos] = arr[i];
      }
      i--;
    }
}
```

#### 2. Minimum Moves to Equal Array Elements II
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

#### 3. Partition Array Into Three Parts With Equal Sum (Prefix Sum)
Given an array of integers arr, return true if we can partition the array into three non-empty parts with equal sums.

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

#### 4. Least Greater Element
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

## Medium

#### 1. Product of Array Except Self
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

#### 2. Beautiful Arrangement II
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

#### 3. Pairs of Songs With Total Durations Divisible by 60
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

#### 4. Advantage Shuffle
Given two arrays A and B of equal size, the advantage of A with respect to B is the number of indices i for which A[i] > B[i]. Return any permutation of A that maximizes its advantage with respect to B.

Input: A = [2,7,11,15], B = [1,10,4,11]
Output: [2,11,7,15]

Approach: Sort both the arrays (B with its index). Iterate over A. if A[i] is just greater then B[i] then store it at index of B[i]. Otherwise, store it at an empty place from the end.

```cpp
static bool mysort(pair<int,int> a, pair<int,int> b){
    if(a.first>b.first) return true;
    return false;
}

vector<int> advantageCount(vector<int>& A, vector<int>& B) {
    vector<pair<int,int>> v;
    for(int i=0; i<B.size(); i++)
        v.push_back({B[i], i});

    vector<int> res(A.size(), 0);

    sort(A.begin(), A.end());
    sort(v.begin(), v.end(), mysort);

    int i=0, j=A.size()-1;

    for(int k=0; k<A.size(); k++){
        int pos = v[k].second;
        if(A[j]>v[k].first){
            res[pos] = A[j];
            j--;
        }
        else{
            res[pos] = A[i];
            i++;
        }
    }
    return res;
}
```

#### 5. Sort Colors (Dutch National Flag algo - 3 pointer solution)
Given an array nums with n objects colored red, white, or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

Hint: Settle 0 at the left end, 2 at the right end => 1 will automatically settle at middle.

```cpp
void sortColors(vector<int>& nums) {
    int low = 0, mid = 0, high = nums.size()-1;
    while(mid<=high){
        if(nums[mid]==0){
        //swap low and mid
            int temp = nums[low];
            nums[low] = nums[mid];
            nums[mid] = temp;
            low++;
            mid++;
        }
        else if(nums[mid]==2){
        // swap high and mid
            int temp = nums[high];
            nums[high] = nums[mid];
            nums[mid] = temp;
            high--;
        }
        else mid++;
    }
}
```

#### 6. Global and Local Inversions
You are given an integer array nums of length n which represents a permutation of all the integers in the range [0, n - 1].
The number of global inversions is the number of the different pairs (i, j) where:
0 <= i < j < n and nums[i] > nums[j]
The number of local inversions is the number of indices i where:
0 <= i < n - 1 and nums[i] > nums[i + 1]
Return true if the number of global inversions is equal to the number of local inversions.

```cpp
bool isIdealPermutation(vector<int>& A) {
    bool flag = false;
    for(int i=0; i<A.size(); i++){
        if(abs(A[i]-i)>1){
            flag = true;
            break;
        }
    }
    if(flag) return false;
    return true;
}
```

#### 7. Rotate Array (Awesome 3 line reverse logic)

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

#### 8. Next Permutation

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


#### 9. 4Sum II
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

#### 10. Increasing Triplet Subsequence
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

#### 11. Smallest Range II (Tricky)
Given an array A of integers, for each integer A[i] we need to choose either x = -K or x = K, and add x to A[i] (only once). After this process, we have some array B. Return the smallest possible difference between the maximum value of B and the minimum value of B.

[Explaination](https://leetcode.com/problems/smallest-range-ii/discuss/173495/Actual-explanation-for-people-who-don't-understand-(I-hope))

```cpp
int smallestRangeII(vector<int>& A, int k) {
    if(A.size()==1) return 0;
    sort(A.begin(), A.end());
    int ans = INT_MAX;
    
    for(int i=0; i<A.size()-1; i++){
        int a = min(A[0]+k, A[i+1]-k);
        int b = max(A[i]+k, A[A.size()-1]-k);
        ans = min(ans, abs(a-b));
    }
    ans = min(ans, A[A.size()-1]-A[0]);
    return ans;
}
```

#### 12. Sum of Absolute Differences in a Sorted Array (Prefix Sum)
You are given an integer array nums sorted in non-decreasing order. Build and return an integer array result with the same length as nums such that result[i] is equal to the summation of absolute differences between nums[i] and all the other elements in the array.

Hint: Calculate prefix sum. Find left sum and right sum for every index i and calculate results.

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

#### 13. Minimum Numbers of Function Calls to Make Target Array (Reverse Logic)
Your task is to form an integer array nums from an initial array of zeros arr that is the same size as nums. Either we can increment any element by 1 or we can multiply any element by 2.

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

#### 14. Find Permutation
Given a positive integer n and a string s consisting only of letters D or I, you have to find any permutation of first n positive integer that satisfy the given input string. D means the next number is smaller, while I means the next number is greater.

Input 1: n = 3, s = ID

Return: [1, 3, 2]

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


## Hard

#### 1. Trapping Rain Water
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

#### 2. Longest Consecutive Sequence (Tricky)
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

#### 3. Candy
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


