# Famous Patterns in Array problems

<br>

## @ Swap Sort

### 1. Find All Numbers Disappeared in an Array (Swap sort variation)

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

<br>

### 2. First Missing Positive (Swap Sort)
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

<br>



## @ Buy and Sell Stocks 

### 1. Best Time to Buy and Sell Stock (Only one transaction allowed)
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

<br>

### 2. Best Time to Buy and Sell Stock II (As many transactions as you want)
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

<br>

### 3. Best Time to Buy and Sell Stock III (Atmost two transactions allowed)

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

<br>



## @ Majority elements (Moore's voting)

### 1. Majority Element I
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

<br>

### 2. Majority Element II (IMP)
Given an integer array of size n, find all elements that appear more than ⌊ n/3 ⌋ times.

Hint: If at any instance, we have three distinct elements, if we remove them then, our answer does not change.

Approach:
1. Create mx1 and mx2 to store two max frequent items so far
2. Create cnt1 and cnt2 for storing their freq
3. Iterate over the array, If we get mx1 or mx2 then increment their cnt else decrement both cnts (we are removing 3 distinct elements)
4. If cnt2 becomes larger then cnt1, swap both vars.
5. After completing all iterations, mx1 and mx2 will only be our possible answers, so once again iterate the array to count their freqs.
6. Return answer whose freq>n/3 else return -1 

```cpp
int Solution::repeatedNumber(const vector<int> &A) {
    int n = A.size();

    int mx1 = INT_MIN, mx2 = INT_MIN;
    int cnt1 = 0, cnt2 = 0;

    for(int num : A){

        if(num==mx1 or cnt1==0){
            mx1 = num;
            cnt1++;
        }
        else if(num==mx2 or cnt2==0){
            mx2 = num;
            cnt2++;
        }
        else{
            cnt1--;
            cnt2--;
        }

        if(cnt2>cnt1){
            swap(mx1, mx2);
            swap(cnt1, cnt2);
        }
    }

    int freq1 = 0, freq2 = 0;

    for(int num : A){
        if(num==mx1 and cnt1>0) freq1++;
        if(num==mx2 and cnt2>0) freq2++;    
    }

    if(freq1 > n/3) 
        return mx1;

    else if(freq2 > n/3)
        return mx2;
    
    else return -1;
}

```

<br>



## @ Kadane's Algo

### 1. Maximum Subarray (Kadane algo)
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

<br>

### 2. Maximum Product Subarray (Kadane with multiplication)
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

<br>

### 3. Maximum Sum Circular Subarray

Hint: Find maxSum subarray and minSum subarray using Kadane's algo. If maxSum<0 return maxSum, else return max(maxSum, totalSum-minSum)

[Full Explaination](https://leetcode.com/problems/maximum-sum-circular-subarray/discuss/178422/One-Pass)

![img](https://assets.leetcode.com/users/motorix/image_1538888300.png)

```cpp
/* Kadane's algo for max sum subarray */

int maxSum_subarray(vector<int> & nums){
    int temp_max = nums[0], maxSum = nums[0];

    for(int i=1; i<nums.size(); i++){
        temp_max = max(temp_max+nums[i], nums[i]);
        maxSum = max(maxSum, temp_max);
    }

    return maxSum;
}

/* Kadane's algo for min sum subarray (just replace max by min)*/

int minSum_subarray(vector<int> & nums){
    int temp_min = nums[0], minSum = nums[0];

    for(int i=1; i<nums.size(); i++){
        temp_min = min(temp_min+nums[i], nums[i]);
        minSum = min(minSum, temp_min);
    }

    return minSum;
}

int maxSubarraySumCircular(vector<int>& nums) {
    int totalSum = 0;

    int maxSum = maxSum_subarray(nums);
    int minSum = minSum_subarray(nums);
    
    /* 
        Boundary Case : If maxSum turns out to be negative that means whole array is negative, 
        In this case, we will return the smallest negative value (i.e. close to zero)
        Hence, we will simply return maxSum
    */
    
    if(maxSum<0)
        return maxSum;

    for(int num : nums)
        totalSum += num;

    return max(maxSum, totalSum-minSum);        // --> return max between (normal kadane sum, circular kadane sum)
}
```

<br>

### 4. K-Concatenation Maximum Sum (Tricky)
Given an integer array arr and an integer k, modify the array by repeating it k times. For example, if arr = [1, 2] and k = 3 then the modified array will be [1, 2, 1, 2, 1, 2]. Return the maximum sub-array sum in the modified array. Note that the length of the sub-array can be 0 and its sum in that case is 0. As the answer can be very large, return the answer modulo 1e9 + 7.

Input: arr = [1,-2,1], k = 5

Output: 2

```cpp
int M = 1000000007;

/* It will simply return max Subarray sum using Kadane's algorithm */

int maxsumSubarray (vector<int> & arr){
    int tempSum = arr[0], maxSum = arr[0];

    for(int i=1; i<arr.size(); i++){
        tempSum = max ((tempSum+arr[i])%M, arr[i]%M);
        maxSum = max (tempSum, maxSum);
    }

    return maxSum;
}

/* It will return max Suffix (of 1st array) + Prefix (of 2nd array) sum using Kadane on two concatenated vectors */ 

int maxPrefixSuffixSum (vector<int> & arr){
    vector<int> double_arr = arr;
    double_arr.insert(double_arr.end(), arr.begin(), arr.end());

    int maxPSsum = maxsumSubarray (double_arr);
    return maxPSsum;
}


int kConcatenationMaxSum(vector<int>& arr, int k) {
    long long totalSum = 0;
    int res = 0;

    for(int num : arr)
        totalSum = (totalSum%M + num%M)%M;

    int KadaneSum = maxsumSubarray(arr);
    int maxPS = maxPrefixSuffixSum(arr);

    /* If k=1 return simple Kadane Sum */

    if(k==1) res = KadaneSum%M;

    /* 
        Else we have 2 cases : Either totalSum is positive is negative 

        1. If totalSum < 0 --> return max(KadaneSum, maxPrefixSuffix)
        2. if totalSum >=0 --> return maxPrefixSuffix + (k-2) * totalSum

    */

    else if(totalSum<0)
        res = max(KadaneSum%M, maxPS%M);

    else
        res = (maxPS + (totalSum%M * (k-2)%M )%M ) % M;

    return max(res%M,0);      // --> return max(res,0) because we can also return empty subarray
}
```

<br>



## @ Wave Sort

### 1. Wiggle Sort 1
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

<br>

### 2. Wiggle Sort II
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

