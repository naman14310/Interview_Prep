# Famous Patterns in Array problems


## @ Swap Sort

#### 1. Find All Numbers Disappeared in an Array (Swap sort variation)

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

#### 2. First Missing Positive (Swap Sort)
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


## @ Buy and Sell Stocks 

#### 1. Best Time to Buy and Sell Stock (Only one transaction allowed)

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

#### 2. Best Time to Buy and Sell Stock II (As many transactions as you want)

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

#### 3. Best Time to Buy and Sell Stock III (Atmost two transactions allowed)

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


## @ Majority elements (Moore's voting)

#### 1. Majority Element I
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

#### 2. Majority Element II
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


## @ Kadane's Algo

#### 1. Maximum Subarray (Kadane algo)
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

#### 2. Maximum Product Subarray (Kadane with multiplication)
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


## @ Problems with Subarrays

#### 1. Subarray Sum Equals K (Tricky)
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

#### 2. Subarray Product Less Than K (Tricky)
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

#### 3. Number of Subarrays with Bounded Maximum
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

#### 4. Arithmetic Slices
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

#### 5. Shortest Unsorted Continuous Subarray
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


## @ Wave Sort

#### 1. Wiggle Sort 1
Given an unsorted array arr. Reorder it in-place such that :  arr[0] <= arr[1] >= arr[2] <= arr[3] . . . .

Hint: Jump over odd posn and compare left and right elements. 

```cpp
#include<bits/stdc++.h>
using namespace std;

void wiggleSort(vector<int> & v){
    int n = v.size();
    for(int i=1; i<n; i+=2){
        
        if(i==n-1 and v[i]<v[i-1]) swap(v[i], v[i-1]);
        else{
            if(v[i]<v[i-1]) swap(v[i], v[i-1]);
            if(v[i]<v[i+1]) swap(v[i], v[i+1]);
        }
    }
}
```

#### 2. Wiggle Sort II
Given an integer array nums, reorder it such that nums[0] < nums[1] > nums[2] < nums[3]....

```cpp

/* Naive Solution */

void wiggleSort(vector<int>& nums) {
    vector<int> temp = nums;
    
    sort(temp.begin(), temp.end(), greater<int>());
    int i=0;
    
    int idx=1;
    while(idx<nums.size()){
        nums[idx] = temp[i];
        idx+=2; i++;
    }
    
    idx = 0;
    while(idx<nums.size()){
        nums[idx] = temp[i];
        idx+=2; i++;
    }
}
```

Optimized Approach:
1. Find median using quick select in O(n) time
2. We can use three-way partitioning to arrange the numbers so that those larger than the median come first, then those equal to the median come next, and then those smaller than the median come last.
3. Now, we can arrange the elements in the three categories in a deterministic way.

(1) Elements that are larger than the median: we can put them in the first few odd slots;

(2) Elements that are smaller than the median: we can put them in the last few even slots;

(3) Elements that equal the median: we can put them in the remaining slots.

