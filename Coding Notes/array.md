# Arrays

## Easy

#### 1. Majority Element
The majority element is the element that appears more than ⌊n / 2⌋ times. You may assume that the majority element always exists in the array.

HINT : Moore's Voting algo (cancel out majority item so far with other item).

```cpp
int majorityElement(vector<int>& nums) {
    int mitem = nums[0], mcount = 1;

    for(int i=1; i<nums.size(); i++){
        if(nums[i]==mitem){
            mcount++;
        }
        else{
            mcount--;
        }
        if(mcount == 0){
            mitem = nums[i];
            mcount = 1;
        }
    }

    return mitem;
}
```

#### 2. Best Time to Buy and Sell Stock (Only one transaction allowed)

Approach : Find max difference between any two elements of array

```cpp
int maxProfit(vector<int>& prices) {
    int n = prices.size();
    int minPrice = INT_MAX;
    int mP = 0;

    for(int i=0; i<n; i++){
        if(minPrice>prices[i])
            minPrice = prices[i];
        
        if(prices[i]-minPrice  > mP)
            mP = prices[i]-minPrice ;
    }
    return  mP;  
}
```

#### 3. Best Time to Buy and Sell Stock II (As many transactions as you want)

Hint: Always buy when price[i]<price[i+1] and buy == 0. Always sell when price[i]>price[i+1] and buy == 1.

```cpp
int maxProfit(vector<int>& prices) {
    int n = prices.size();
    if(n==1) return 0;
    if(n==2) return prices[1]>prices[0]?prices[1]-prices[0]:0;

    int profit = 0, buy = 0, init = 0;
    for(int i=0; i<n-1; i++){
        if(prices[i]<prices[i+1] && buy==0){
            buy = 1;
            init = prices[i];
        }
        else if(prices[i]>prices[i+1] && buy==1){
            profit+= prices[i] - init;
            buy = 0;
        }
    }

    if(buy==1) profit += prices[n-1]-init;
    return profit;
}
```

#### 4. Find All Numbers Disappeared in an Array (Swap sort variation)

Hint: Swap sort (Use swap sort whenever they asked for duplicacy or missingness of no. ranges from [1,n] )

Similar questions (on leetcode):
1. Find All Duplicates in an Array
2. Set Mismatch


```cpp
vector<int> findDisappearedNumbers(vector<int>& nums) {
    vector<int> result;
    for(int i=0; i<nums.size(); i++){
        while(i!=nums[i]-1 && nums[i]!=nums[nums[i]-1]){

            int a = nums[i]; 
            int b = nums[nums[i]-1];
            nums[i] = b;
            nums[a-1] = a;

        }
    }

    for(int i=0; i<nums.size(); i++)
        if(nums[i]!=i+1) result.push_back(i+1);
    
    return result;
}

```

#### 5. Maximum Subarray (Kadane algo)
Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

```cpp
int maxSubArray(vector<int>& nums) {
    int maxSum = nums[0], tempSum = nums[0];
    for(int i=1; i<nums.size(); i++){
        tempSum = max(tempSum+nums[i], nums[i]);
        maxSum = max(tempSum, maxSum);
    }
    return maxSum;
}
```

#### 6. Duplicate Zeros
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

#### 7. Minimum Moves to Equal Array Elements II
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

#### 8. Partition Array Into Three Parts With Equal Sum (Prefix Sum)
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

#### 7. Subarray Sum Equals K (Tricky)
Given an array of integers nums and an integer k, return the total number of continuous subarrays whose sum equals to k.

Approach: Create hashmap and initialize with mp[0] = 1. Compute prefix sum. Iterate over the prefix sum and calculate gain (gain = cs[i]-k) for every iteration. If gain exist in hashmap, add mp[gain] to answer. Add prefix sum of each iteration to hashmap.

[Video Solution](https://www.youtube.com/watch?v=MHocw0bP1rA)

```cpp
int subarraySum(vector<int>& nums, int k) {
    int ans = 0;
    unordered_map<int, int> mp;
    mp[0] = 1;
    int cs = 0;
    for(int n : nums){
        cs += n;
        int gain = cs - k;
        if(mp.find(gain)!=mp.end()) ans+=mp[gain];
        mp[cs] += 1;
    }
    return ans;
}
```

#### 8. Subarray Product Less Than K (Tricky)
Count and print the number of (contiguous) subarrays where the product of all the elements in the subarray is less than k.

Hint: Everytime when we add new no. to existing subarray, and if product of new subarray is less then k => then it will add R-L+1 subarray to our answer.

[Video Solution](https://www.youtube.com/watch?v=4775IgUKfww)

```cpp
int numSubarrayProductLessThanK(vector<int>& nums, int k) {
    int product = nums[0];
    int ans = 0;
    int l=0, r=0;
    while(r<nums.size()){
        if(product>=k){
            product /= nums[l];
            if(l==r){
                r++;
                if(r>=nums.size()) break;
                product*=nums[r];
            }
            l++;
        }
        else{
            ans += r-l+1;
            r++;
            if(r>=nums.size()) break;
            product*=nums[r];
        }   
    }
    return ans;
}
```

#### 9. Maximum Product Subarray (Kadane with multiplication)
Given an integer array nums, find a contiguous non-empty subarray within the array that has the largest product, and return the product.

Hint: Maintain positive and negative products and swap(ptemp, ntemp) whenever negative element appears. ps, zeros are break points (reinitialize ptemp, ntemp to 1)

```cpp
int maxProduct(vector<int>& nums) {
    int ptemp = 1, ntemp = 1, product = INT_MIN;
    bool zero = false;

    for(int i=0; i<nums.size(); i++){
        if(nums[i]==0){
            zero = true;
            break;
        }
    }

    for(int i=0; i<nums.size(); i++){
        if(nums[i]==0){
            ptemp = 1;
            ntemp = 1;
            continue;
        }
        if(nums[i]<0){
            int temp = ptemp; 
            ptemp = ntemp;
            ntemp = temp;
        }
        ptemp = max(ptemp*nums[i], nums[i]);
        ntemp = min(ntemp*nums[i], nums[i]);
        product = max(ptemp, product);
    }
    if(zero==true) return max(product,0);
    else return product;
}
```

#### 10. Shortest Unsorted Continuous Subarray
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

#### 11. Rotate Array (Awesome 3 line reverse logic)

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

#### 12. Next Permutation

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


#### 13. 4Sum II
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

#### 14. Arithmetic Slices
An integer array is called arithmetic if it consists of at least three elements and if the difference between any two consecutive elements is the same. Given an integer array nums, return the number of arithmetic subarrays of nums.

Hint: Find count of equal common differences. If count is n then it gives n*(n-1)/2 subarrays.

```cpp
int numberOfArithmeticSlices(vector<int>& A) {
    int len = A.size();
    int ans = 0;
    int i=1;
    while(i<len){
        int item = A[i]-A[i-1];
        int count = 0;

        while(i<len && A[i]-A[i-1]==item){
            count++;
            i++;
        }
        if(count>=2) ans += (count*(count-1))/2;
    }
    return ans;
}
```

#### 15. Increasing Triplet Subsequence
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

#### 16. Smallest Range II (Tricky)
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

#### 17. Sum of Absolute Differences in a Sorted Array (Prefix Sum)
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

#### 18. Majority Element II (Variation of Moore Voting)
Given an integer array of size n, find all elements that appear more than ⌊ n/3 ⌋ times.

Hint: Use item1 and item2 to store largest and second largest element & count1 and count2 to store their respective counts.

```cpp
bool checkValidity(vector<int> & nums, int item){
    int c = 0;
    for(int i : nums)
        if(i==item) c++;

    if(c > nums.size()/3) return true;
    return false;
}

vector<int> majorityElement(vector<int>& nums) {
    int count1 = 0, count2 = 0;
    int item1 = 0, item2 = 0;
    int i=0;

    while(i<nums.size()){
        if(count1 == 0){
            item1 = nums[i];
            count1++;
        }
        else if(item1==nums[i]) count1++;

        else if(count2 == 0){
            item2 = nums[i];
            count2++;
        }
        else if(item2 == nums[i]) count2++;

        else{
            count1--;
            count2--;
        }

        if(count2>count1){
            swap(item1, item2);
            swap(count1, count2);
        }
        i++;
    }

    vector<int> res;
    if(checkValidity(nums, item1))
        res.push_back(item1);
    if(item2!=item1)
        if(checkValidity(nums, item2))
            res.push_back(item2);

    return res;
}

```

## Hard

#### 1. First Missing Positive (Swap Sort)
Given an unsorted integer array nums, find the smallest missing positive integer.

```cpp
int firstMissingPositive(vector<int>& nums) {
    int len = nums.size();
    if(len<=0) return 1;

    for(int i=0; i<len; i++){
        while(nums[i]>0 && nums[i]<len && i!=nums[i]-1 && nums[i]!=nums[nums[i]-1]){
            int a = nums[i];
            int b = nums[nums[i]-1];
            nums[i] = b;
            nums[a-1] = a;
        }
    }

    for(int i=0; i<len; i++){
        if(i!=nums[i]-1) return i+1;
    }
    return len+1;
}
```

#### 2. Trapping Rain Water
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

#### 3. Longest Consecutive Sequence (Tricky)
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

#### 4. Best Time to Buy and Sell Stock III (Atmost two transactions allowed)

[Video Solution](https://www.youtube.com/watch?v=37s1_xBiqH0)

```cpp
/* 

Whole idea is to divide the whole array in two parts 
and perform two transaction in two different parts

*/

int maxProfit(vector<int>& prices) {

    int n = prices.size();

    vector<int> left (n, 0);     // This will contain max profit for left part (1st transaction)
    vector<int> right (n, 0);    // This will contain max profit for right part (2nd transaction)

    int leftMin = prices[0];
    int rightMax = prices[n-1];

    for(int i=1; i<n; i++){
        if(prices[i]<=leftMin){
            leftMin = prices[i];
            left[i] = left[i-1];
        }
        else{
            left[i] = max(left[i-1], prices[i]-leftMin);
        }
    }

    for(int i=n-2; i>=0; i--){

        if(prices[i]>=rightMax){
            rightMax = prices[i];
            right[i] = right[i+1];
        }
        else{
            right[i] = max(right[i+1], rightMax-prices[i]);
        }
    }

    int ans = 0;

    for(int i=0; i<n; i++)
        ans = max(ans, left[i]+right[i]);

    return ans;
}
```
