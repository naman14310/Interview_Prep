# Arrays

## Easy

#### 977. Squares of a Sorted Array 
Given an integer array nums sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.

```cpp
vector<int> sortedSquares(vector<int>& nums) {
    int len = nums.size();
    vector<int> res(len, 0);
    int l=0, r=len-1, pos = len-1;
    while(l<=r){
        int sqrL = nums[l]*nums[l];
        int sqrR = nums[r]*nums[r];
        if(sqrR > sqrL){
            res[pos] = sqrR;
            r--; 
        }
        else{
            res[pos] = sqrL;
            l++;
        }
        pos--;
    }
    return res;
}
```

#### 169. Majority Element
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

#### 122. Best Time to Buy and Sell Stock II (As many transactions as you want)

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

#### 448. Find All Numbers Disappeared in an Array

Hint : Swap sort (Use swap sort whenever they asked for duplicacy or missingness of no. ranges from [1,n] )

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

697. Degree of an Array
Given a non-empty array of non-negative integers nums, the degree of this array is defined as the maximum frequency of any one of its elements. Your task is to find the smallest possible length of a (contiguous) subarray of nums, that has the same degree as nums.

Solution with single pass:

```cpp
int findShortestSubArray(vector<int>& nums) {
    unordered_map<int,pair<pair<int,int>,int>> mp;
    for(int i=0; i<nums.size(); i++){
        int num = nums[i];

        if(mp.find(num)==mp.end()){
            mp[num] = {{i,i}, 1};
        }
        else{
           pair< pair<int,int>, int > p = mp[num];
            int start = p.first.first;
            int count = p.second;
            mp[num] = {{start, i}, count+1};
        }
    }

    int ans = INT_MAX;
    int maxfrequency = 0;
    for(auto p : mp){

        int len = p.second.first.second - p.second.first.first + 1;
        int freq = p.second.second;
        if(maxfrequency<freq){
            maxfrequency = freq;
            ans = len;
        }
        else if(maxfrequency==freq && ans>len){
            maxfrequency = freq;
            ans = len;
        }
    }
    return ans;
}
```
#### 121. Best Time to Buy and Sell Stock (Only one transaction allowed)

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

#### 53. Maximum Subarray
Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

Hint: Kadane's algo

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

