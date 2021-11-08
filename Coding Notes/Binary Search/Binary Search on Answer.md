# Tricky Problems on Binary Search on Answer

<br>

### 1. Capacity To Ship Packages Within D Days 
A conveyor belt has packages that must be shipped from one port to another within D days. Each day, we load the ship with packages on the conveyor belt (in the order given by weights). We may not load more weight than the maximum weight capacity of the ship. Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within D days.

```cpp
bool isValid(vector<int> & weights, int D, int capacity){
    int count = 1;
    int i = 0;
    int cap = 0;
    while(i<weights.size()){
        cap += weights[i];
        if(cap>capacity){
            count++;
            cap=0;
        }
        else
            i++;
    }
    if(count>D) return false;
    else return true;
}

int binarySearch(vector<int> & weights, int low, int high, int D){
    int ans = high;
    while(low<=high){
        int mid = low + (high-low)/2;
        if(isValid(weights, D, mid)){
            ans = mid;
            high = mid-1;
        }
        else low = mid+1;
    }
    return ans;
}

int shipWithinDays(vector<int>& weights, int D) {
    int low = 0;
    int high = 0;
    for(int i : weights){
        low = max(low, i);
        high+=i;
    }
    return binarySearch(weights, low, high, D);
}
```

<br>

### 2. Koko Eating Bananas 
There are n piles of bananas, the ith pile has piles[i] bananas. The guards have gone for h hours. Koko decides her eating speed of k. Each hour, she chooses some pile of bananas and eats k bananas from that pile. If the pile has less than k bananas, she eats all of them and will not eat any more bananas during this hour. Koko wants to finish eating all the bananas before the guards return. Return the minimum integer k such that she can eat all the bananas within h hours.

**Example**
Input: piles = [30,11,23,4,20], h = 5
Output: 30

```cpp
bool isvalid(vector<int> & piles, long long int mid, int h){
    long long int cnt = 0;
    for(int i=0; i<piles.size(); i++){
        if(piles[i]<=mid) cnt++;
        else cnt += (int) ceil ((double)piles[i]/(double)mid);
    }
    if(cnt>h) return false;
    return true;
}

int binarySearch(vector<int> & piles, long long int start, long long int end, int h){
    int ans = end;
    while(start<=end){
        long long int mid = start + (end-start)/2;
        if(isvalid(piles, mid, h)){
            ans = mid;
            end = mid-1;
        }
        else start = mid+1;
    }
    return ans;
}

int minEatingSpeed(vector<int>& piles, int h) {
    long long int low = 1, high = 0;
    for(int i : piles) high+=i;
    return binarySearch(piles, low, high, h);
}
```

<br>

### 3. Minimum Number of Days to Make m Bouquets 
Given an integer array bloomDay, an integer m and an integer k. We need to make m bouquets. To make a bouquet, you need to use k adjacent flowers from the garden. The garden consists of n flowers, the ith flower will bloom in the bloomDay[i] and then can be used in exactly one bouquet. Return the minimum number of days you need to wait to be able to make m bouquets from the garden. If it is impossible to make m bouquets return -1.

Input: bloomDay = [7,7,7,7,12,7,7], m = 2, k = 3

Output: 12

Explanation: We need 2 bouquets each should have 3 flowers.
Here's the garden after the 7 and 12 days:

After day 7: [x, x, x, x, _, x, x]

We can make one bouquet of the first three flowers that bloomed. We cannot make another bouquet from the last three flowers that bloomed because they are not adjacent.

After day 12: [x, x, x, x, x, x, x]

It is obvious that we can make two bouquets in different ways.

```cpp
bool isValid(vector<int> & bloomDay, int mid, int m, int k){
    int i=0, sz=0, cnt=0;
    while(i<bloomDay.size()){
        if(i==0 || bloomDay[i-1]>mid) sz=0;
        if(bloomDay[i]<=mid) sz++;
        if(sz==k){
            sz=0;
            cnt++;
        }
        i++;
    }
    if(cnt<m) return false;
    return true;        
}

int binarySearch(vector<int> & bloomDay, int start, int end, int m, int k){
    int ans = end;
    while(start<=end){
        int mid = start + (end-start)/2;
        if(isValid(bloomDay, mid, m, k)){
            ans = mid;
            end = mid-1;
        }
        else
            start = mid+1;           
    }
    return ans;
}

int minDays(vector<int>& bloomDay, int m, int k) {
    int n = bloomDay.size();
    if(m*k > n) return -1;

    int low = INT_MAX;
    int high = INT_MIN;

    for(int b : bloomDay){
        low = min(b, low);
        high = max(b, high);
    }
    return binarySearch(bloomDay, low, high, m, k);
}
```

<br>

### 4. Find the Smallest Divisor Given a Threshold
Given an array of integers nums and an integer threshold, we will choose a positive integer divisor, divide all the array by it, and sum the division's result. Find the smallest divisor such that the result mentioned above is less than or equal to threshold. Each result of the division is rounded to the nearest integer greater than or equal to that element. (For example: 7/3 = 3 and 10/2 = 5).

```cpp
bool isValid(vector<int> nums, int mid, int threshold){
    int sum=0;
    for(auto i : nums){
        double d = ceil((double)i/(double)mid);
        sum+= (int)d;
    }
    if(sum>threshold) return false;
    return true;
}

int binarySearch(vector<int>nums, int start, int end, int threshold){
    int ans = end;
    while(start<=end){
        int mid = start + (end-start)/2;
        if(isValid(nums, mid, threshold)){
            ans = mid;
            end = mid-1;
        }
        else
            start = mid+1;
    }
    return ans;
}

int smallestDivisor(vector<int>& nums, int threshold) {
    int low = 1;
    int high = INT_MIN;
    for(int i : nums)
        high = max(high, i);
    
    return binarySearch(nums, low, high, threshold);
}
```

<br>

### 5. Sum of Mutated Array Closest to Target
Given an integer array arr and a target value target, return the integer value such that when we change all the integers larger than value in the given array to be equal to value, the sum of the array gets as close as possible (in absolute difference) to target. In case of a tie, return the minimum such integer.

```cpp
int getSum(vector<int> & arr, int mid, int target){
    int sum=0;
    for(auto i : arr){
        if(i>mid) sum+=mid;
        else sum+=i;
    }
    return sum;
}


int binarySearch(vector<int> & arr, int start, int end, int target, int & mindiff){
    int ans = end;
    while(start<=end){
        int mid = start + (end-start)/2;

        int sum = getSum(arr, mid, target);
        int diff = abs(sum-target);

        if(diff<mindiff){
            ans = mid;
            mindiff = diff;
        }

        else if(diff==mindiff && mid<ans){
            ans=mid;
        }

        if(sum<target) start = mid+1;
        else end = mid-1;
    }
    return ans;     
}


int findBestValue(vector<int>& arr, int target) {
    int low = 0;
    int high = INT_MIN;
    for(int i : arr){
        high = max(high, i);
    }
    int mindiff = INT_MAX;
    return binarySearch(arr, low, high, target, mindiff);
}
```

<br>

### 6. Allocate minimum number of pages
You are given N number of books. Every ith book has Ai number of pages.You have to allocate books to M number of students. The task is to find that particular permutation in which the maximum number of pages allocated to a student is minimum of those in all the other permutations and print this minimum value. Each book will be allocated to exactly one student. Each student has to be allocated at least one book.

Note: Return -1 if a valid assignment is not possible, and allotment should be in contiguous order

```cpp
bool isValid(int arr[], int mid, int m, int n){
    int i=0, sum=0, cnt=1;
    while(i<n){
        sum+=arr[i];

        if(sum>mid){
            cnt++;
            sum=arr[i];
        }
        i++;
    }
    if(cnt<=m) return true;
    else return false;
}

int binarySearch(int arr[], int start, int end, int m, int n){
    int ans = -1;
    while(start<=end){
        int mid = start + (end-start)/2;

        if(isValid(arr, mid, m, n)){
            ans = mid;
            end = mid-1;
        }
        else start = mid+1;
    }
    return ans;
}

int findPages(int arr[], int n, int m) 
{   if(n<m) return -1;

    int low = INT_MIN, high = 0;
    for(int i=0; i<n; i++){
        low = max(low, arr[i]);
        high+=arr[i];
    }

    int res = binarySearch(arr, low, high, m, n);
    
    int maxpage = 0, i=0, sum=0;
    while(i<n){
        sum+=arr[i];
        if(sum>res){
            maxpage = max(maxpage, sum-arr[i]);
            sum=arr[i];
        }
        i++;
    }
    return max(sum,maxpage);
}
```

<br>

### 7. Split Array Largest Sum
Given an array nums which consists of non-negative integers and an integer m, you can split the array into m non-empty continuous subarrays. Write an algorithm to minimize the largest sum among these m subarrays.

Input: nums = [7,2,5,10,8], m = 2

Output: 18

```cpp
bool isValid (vector<int> & nums, int mid, int m){
    int i=0, sum=0;
    int cnt = 1;

    while(i<nums.size()){
        if(sum+nums[i]>mid){
            sum=0;
            cnt++;
        }
        else{
            sum += nums[i];
            i++;
        }
    }

    return cnt<=m;
}


int binarySearch (vector<int> & nums, int start, int end, int m){
    int ans = end;

    while(start<=end){
        int mid = start + (end-start)/2;

        if(isValid(nums, mid, m)){
            ans = mid;
            end = mid-1;
        }
        else
            start = mid+1;
    }

    return ans;
}


int splitArray(vector<int>& nums, int m) {
    int start = *max_element(nums.begin(), nums.end());
    int end = accumulate(nums.begin(), nums.end(), 0);

    return binarySearch (nums, start, end, m);
}
```

<br>

### 8. Painter's Partition Problem (IB)
Given 2 integers A and B and an array of integars C of size N. Element C[i] represents length of ith board. There are A painters available and each of them takes B units of time to paint 1 unit of board. Calculate and return minimum time required to paint all boards under the constraints that any painter will only paint contiguous sections of board. Return the ans % 10000003

Input: A = 10, B = 1, C = [1, 8, 11, 3]

Output: 11

PS: Use unsigned long long datatype for variables created by us.

```cpp
bool isValid (int A, int B, vector<int>& C, unsigned long long mid){
    int i=0, cnt=0;
    unsigned long long time = 0;

    while(i<C.size()){
        
        if(time+B*C[i]<=mid){
            time += B*C[i];
            i++;
        }
        else{
            time=0;
            cnt++;
        }
    }
    cnt++;

    return cnt<=A;
}


unsigned long long binarySearch (int A, int B, vector<int> &C, unsigned long long low, unsigned long long high){
    unsigned long long ans = high;

    while(low<=high){
        unsigned long long mid = low + (high-low)/2;

        if(isValid(A, B, C, mid)){
            ans = mid;
            high = mid-1;
        }

        else low = mid+1;
    }

    return ans;
}


int Solution::paint(int A, int B, vector<int> &C) {
    int mod = 10000003;
    B = B%mod;

    unsigned long long mx = *max_element(C.begin(), C.end());
    unsigned long long sum = accumulate(C.begin(), C.end(), 0);

    unsigned long long low = (B*mx);
    unsigned long long high = (B*sum);

    if(A==1) 
        return high%mod;

    unsigned long long maxLimit =  binarySearch(A, B, C, low, high);

    int i=0;
    unsigned long long time=0, maxTime=0;

    while(i<C.size()){
        
        if(time+B*C[i]<=maxLimit){
            time += B*C[i];
            i++;
        }
        else{
            maxTime = max(time, maxTime);
            time=0;
        }
    }
    maxTime = max(time, maxTime);
    
    return maxTime%mod;
}
```

<br>

### 9. WoodCutting Made Easy (IB)

[Question](https://www.interviewbit.com/problems/woodcutting-made-easy/)

```cpp
bool isValid(vector<int> &A, long long B, long long bladeHeight){
    long long sum=0;

    for(auto h : A)
        if(h>bladeHeight) sum += h-bladeHeight;
    
    return sum>=B;
}


int binarySearch (vector<int> &A, long long B, long long low, long long high){
    long long ans = -1;
    
    while(low<=high){
        long long mid = low + (high-low)/2;

        if(isValid(A, B, mid)){
            ans = mid;
            low = mid+1;
        }

        else high = mid-1;
    }

    return ans;
}


int Solution::solve(vector<int> &A, int B) {
    long long low = 0;
    long long high = *max_element(A.begin(), A.end());

    return binarySearch(A, B, low, high);
}
```

<br>

### 10. Minimum Speed to Arrive on Time

[Question](https://leetcode.com/problems/minimum-speed-to-arrive-on-time/)

Input: dist = [1,3,2], hour = 2.7

Output: 3

```cpp
bool reachOnTime (vector<int> &dist, double hour, double speed){
    double time = 0;
    int n = dist.size();

    for(int i=0; i<n; i++){
        double d = dist[i];

        if(i<n-1)
            time += ceil(d/speed);
        else
            time += d/speed;
    }

    return time<=hour;
}


int binarySearch (vector<int> &dist, double hour, int start, int end){
    int ans = end;

    while(start<=end){
        int mid = start + (end-start)/2;

        if(reachOnTime(dist, hour, mid)){
            ans = mid;
            end = mid-1;
        }

        else start = mid+1;
    }

    return ans;
}


int minSpeedOnTime(vector<int>& dist, double hour) {
    int n = dist.size();
    if(n-1 >= hour) return -1;

    int start = 1, end = INT_MAX;
    return binarySearch (dist, hour, start, end);
}
```

<br>

### 11. Heaters
Given the positions of houses and heaters on a horizontal line, return the minimum radius standard of heaters so that those heaters could cover all houses.

Input: houses = [1,5], heaters = [2]

Output: 3

```cpp
bool isValid(vector<int> &minDist, int radius){        
    for(int d : minDist)
        if(d>radius) return false;

    return true;
}


int binarySearch(vector<int> &minDist, int start, int end){
    int ans = end;

    while(start<=end){
        int mid = start + (end-start)/2;

        if(isValid(minDist, mid)){
            ans = mid;
            end=mid-1;
        }

        else start = mid+1;
    }        

    return ans;
}


int findRadius(vector<int>& houses, vector<int>& heaters) {
    sort(heaters.begin(), heaters.end());
    vector<int> minDist (houses.size(), INT_MAX);

    /* Compute closest distance of every house from nearest heater */

    for(int i=0; i<houses.size(); i++){
        int h = houses[i];
        auto pos = lower_bound(heaters.begin(), heaters.end(), h) - heaters.begin();

        if(pos==0){
            int closest = pos;
            minDist[i] = abs(heaters[closest] - h);
        }

        else if(pos==heaters.size()){
            int closest = pos-1;
            minDist[i] = abs(heaters[closest] - h);
        }

        else{
            int closest1 = pos-1, closest2 = pos;
            minDist[i]= min(abs(heaters[closest1] - h), abs(heaters[closest2] - h));
        }
    }


    return binarySearch(minDist, 0, INT_MAX);
}
```

<br>

### 12. [Sell Diminishing-Valued Colored Balls](https://leetcode.com/problems/sell-diminishing-valued-colored-balls/)

```cpp
long long getOrder (vector<int> &inventory, int mid, int orders){
    int n = inventory.size();       
    long long ord = 0;

    for(int i=n-1; i>=0; i--){
        if(inventory[i]>mid)
            ord += inventory[i] - mid;
    }

    return ord;
}


long long maxProfit (vector<int> &inventory, int start, int end, int orders){
    int threshold = 0;
    long long ord = 0;

    while(start<=end){
        int mid = start + (end-start)/2;

        long long o = getOrder(inventory, mid, orders);

        if(o<=orders){
            threshold = mid;
            ord = o;
            end = mid-1;
        }

        else start = mid+1;
    }


    long long profit = 0;

    for(int i=inventory.size()-1; i>=0; i--){
        if(inventory[i]>threshold){
            long long n = inventory[i] - threshold;
            long long temp = ((n) * (threshold + 1 + inventory[i])) / 2;

            profit = (profit + temp) % 1000000007;
        }
    }


    while(ord<orders){
        profit = (profit + threshold) % 1000000007;
        ord++;
    }

    return profit;
}


int maxProfit(vector<int>& inventory, int orders) {
    sort(inventory.begin(), inventory.end());
    int start = 0, end = inventory.back();

    return maxProfit (inventory, start, end, orders);
}
```
