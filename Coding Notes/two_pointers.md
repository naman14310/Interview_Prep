# Two Pointers 

## Easy

#### 1. Move Zeroes
Given an integer array nums, move all 0's to the end of it while maintaining the relative order of the non-zero elements.
Hint: Use two pointers from left to right. Left pointer to keep track for rightmost non zero index. Right pointer to iterate array.

```cpp
void moveZeroes(vector<int>& nums) {
    int i=0, j = 1;
    while(j<nums.size()){
        if(nums[i]!=0){
            i++;
            if(i==j) j++;
        }
        else if(nums[i]==0 && nums[j]!=0){
            swap(nums[i], nums[j]);
            i++;
            j++;
        }
        else
            j++;
    } 
}
```

#### 2. Remove Element
Given an array nums and a value val, remove all instances of that value in-place and return the new length.

```cpp
int removeElement(vector<int>& nums, int val) {
    int i=0;
    int len = nums.size();
    while(i<len){
        if(nums[i]==val){
            nums[i]=nums[len-1];
            len--;
        }
        else
            i++;
    }
    return len;
}
```

#### 3. Remove Duplicates from Sorted Array
Given a sorted array nums, remove the duplicates in-place such that each element appears only once and returns the new length.

```cpp
int removeDuplicates(vector<int>& nums) {
    if(nums.size()<=1) return nums.size();
    int i=0,j=1;
    int count=1;
    while(j<nums.size()){
        if(nums[i]==nums[j])
            j++;
        
        else{
            i++;
            swap(nums[i], nums[j]);
            count++;
            j++;
        }
    }
    return count;
}
```

#### 4. Remove Duplicates from Sorted Array II
Given a sorted array nums, remove the duplicates in-place such that duplicates appeared at most twice and return the new length.

```cpp
int removeDuplicates(vector<int>& nums) {
    int len = nums.size();
    if(len<=2) return len;

    int var = nums[0];
    int i=1, j=1, count=1;
    while(j<len){
        if(nums[j]==var){
            count++;
            if(count<=2){
                swap(nums[i], nums[j]);
                i++; j++;
            }
            else j++;
        }
        else{
            var = nums[j];
            count = 1;
            swap(nums[i], nums[j]);
            i++; j++;
        }
    }
    return i;
}
```

#### 5. Merge Sorted Array
Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.

```cpp
void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
   int a = m-1;
   int b = n-1;
   int i = m+n-1;
   while(a>=0 && b>=0){
        if(nums1[a]<=nums2[b]){
            nums1[i] = nums2[b];
            b--;
        }
        else{
            nums1[i] = nums1[a];
            a--;
        }
        i--;
    }
    while(b>=0){
        nums1[i] = nums2[b];
        b--;
        i--;
    }
}
```

#### 6. Intersection of Two Arrays II
Given two integer arrays nums1 and nums2, return an array of their intersection. Each element in the result must appear as many times as it shows in both arrays and you may return the result in any order.

```cpp
vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
    sort(nums1.begin(), nums1.end());
    sort(nums2.begin(), nums2.end());
    int n1 = nums1.size(), n2 = nums2.size();
    int i1 = 0, i2 = 0;
    vector<int> res;
    while(i1 < n1 && i2 < n2){
        if(nums1[i1] == nums2[i2]) {
            res.push_back(nums1[i1]);
            i1++;
            i2++;
        }
        else if(nums1[i1] > nums2[i2])
            i2++;
        
        else
            i1++;
    }
    return res;
}
```

#### 7. Backspace String Compare
Given two strings s and t, return true if they are equal when both are typed into empty text editors. '#' means a backspace character.

```cpp
bool backspaceCompare(string s, string t) {
    int k=0,p=0;
    for(int i=0;i<s.size();i++)
    {
      if(s[i]=='#')
      {
          k--;
          k=max(0,k);
      }
      else
      {
          s[k]=s[i];
          k++;
      }
    }
    for(int i=0;i<t.size();i++)
    {
       if(t[i]=='#')
       {
            p--;
            p=max(0,p);
       }
       else
       {
            t[p]=t[i];
            p++;
       }
    }
    if(k!=p) return false;
    else
    {
        for(int i=0;i<k;i++)
        {
            if(s[i]!=t[i]) return false;
        }
        return true;
    }
}
```

#### 8. Squares of a Sorted Array 
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

#### 9. Is Subsequence
Given two strings s and t, check if s is a subsequence of t.

```cpp
/* EFFIECIENT FOR SINGLE QUERY => O(N) Approach*/

bool isSubsequence(string s, string t) {
    if(s.length()==0 && t.length()==0) return true;
    if(s.length()>t.length()) return false;
    int i=0, j=0;
    while(i<s.length() && j<t.length()){
        if(s[i]==t[j]){
            i++;j++;
        }
        else j++;
    }
    if(i<s.length()) return false;
    return true;
}

/* EFFIECIENT FOR MULTIPLE QUERIES -> Q*(LOG N) Approach */
    
bool isSubsequence(string s, string t) {
    unordered_map<char, vector<int>> mp;
    int idx = 0;
    for(char ch : t){
        mp[ch].push_back(idx);
        idx++;
    }

    int low=0;
    for(int i=0; i<s.length(); i++){
        char ch = s[i];
        if(mp.find(ch)==mp.end()) return false;
        auto v = mp[ch];
        int pos = lower_bound(v.begin(), v.end(), low) - v.begin();
        if(pos<v.size()) low = v[pos]+1;
        else return false;
    }
    return true;
}

```

#### 10. Container With Most Water

```cpp
int maxArea(vector<int>& height) {
    int res = 0;
    int len = height.size();
    int i=0, j=len-1;
    while(i<j){
        res = max(res, min(height[i],height[j])*(j-i) );
        if(height[i]<height[j]) i++;
        else j--;
    }
    return res;
}
```

## Medium

#### 1. 3Sum (Remove Duplicates)
Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i != j, i != k, and j != k, and nums[i] + nums[j] + nums[k] == 0.
=> Notice that the solution set must not contain duplicate triplets.

```cpp
vector<vector<int>> threeSum(vector<int>& nums) {
    vector<vector<int>> ans;
    if(nums.size()<3) return ans;

    sort(nums.begin(), nums.end());

    for(int k=0; k<nums.size()-2; k++){
        if(k>0 && nums[k]==nums[k-1]) continue; // FOR REMOVING DUPLICATES

        int sum = -(nums[k]);
        int i=k+1, j=nums.size()-1;

        while(i<j){
            if(nums[i]+nums[j]==sum){
                vector<int> temp;
                temp.push_back(nums[k]);
                temp.push_back(nums[i]);
                temp.push_back(nums[j]);
                ans.push_back(temp);
                i++;
                j--;
                while(i<j && nums[i]==nums[i-1]) i++; // FOR REMOVING DUPLICATES
                while(i<j && nums[j]==nums[j+1]) j--; // FOR REMOVING DUPLICATES
            }
            else if(nums[i]+nums[j]<sum) i++;
            else j--;
        }
    }
    return ans;
}
```

#### 2. 3Sum With Multiplicity (Tricky)
Given an integer array arr, and an integer target, return the number of tuples i, j, k such that i < j < k and arr[i] + arr[j] + arr[k] == target. As the answer can be very large, return it modulo 109 + 7.

Approach:
The idea is same with 3Sum, but need to care about combination of duplicates. Firstly sort A, then say we fix i as first number, and we want to find all possibilities of A[lo] + A[hi] == target - A[i].
Case1 => A[lo] != A[hi]. Count the number of duplicates of A[lo] and A[hi], as cntlo and cnthi, then for i, there are cnthi*cntlo possibilities
Case2 => A[lo] == A[hi]. Now all number between lo and hi are same. Say the number of duplicates is n, any two of n can meet A[lo] + A[hi] == target - A[i], that is n*(n-1)/2.

```cpp
int threeSumMulti(vector<int>& arr, int target) {
    int n = arr.size(), mod = 1e9+7;
    sort(arr.begin(), arr.end());
    int ans = 0;
    for(int i=0; i<n-2; i++){
        int j = i+1, k = n-1;

        while(j<k){
            if(arr[i] + arr[j] + arr[k] == target){
                int countj = 1, countk = 1; 
                while(j<n-1 && arr[j]==arr[j+1]){
                    j++; countj++;
                }
                while(k>i+1 && arr[k]==arr[k-1]){
                    k--; countk++;
                }
                if(arr[j]!=arr[k])                                 // -----> CASE 1
                    ans = (ans + countj*countk) % mod;
                else                                               // -----> CASE 2
                    ans = (ans + (countj*(countj-1))/2) % mod;

                j++; k--;
            }
            else if(arr[i] + arr[j] + arr[k] > target) k--;
            else j++;
        }
    }   
    return ans;
}
```



#### 3. 3Sum Closest
Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. 

```cpp
int threeSumClosest(vector<int>& nums, int target) {
    sort(nums.begin(), nums.end());
    int closest = INT_MAX;
    int ans = 0;
    for(int i=0; i<nums.size()-2; i++){
        int a = nums[i];
        int l = i+1; 
        int r = nums.size()-1;
        while(l<r){
            int sum = a+nums[l]+nums[r];
            if(closest > abs(target-sum)){
                closest = abs(target-sum);
                ans = sum;
            }
            if(sum>target) r--;
            else l++;
        }
    }
    return ans;
}
```

#### 4. Boats to Save People
You are given an array people where people[i] is the weight of the ith person, and an infinite number of boats where each boat can carry a maximum weight of limit. Each boat carries at most two people at the same time, provided the sum of the weight of those people is at most limit. Return the minimum number of boats to carry every given person.

```cpp
int numRescueBoats(vector<int>& people, int limit) {
    sort(people.begin(), people.end());
    int i=0, j=people.size()-1;
    int boats = 0;
    while(i<=j){
        if(people[i]+people[j]<=limit){
            i++; j--;
        }
        else j--;
  
        boats++;
    }
    return boats;
}
```

#### 5. Max Number of K-Sum Pairs (Two sum in other words)
You are given an integer array nums and an integer k. In one operation, you can pick two numbers from the array whose sum equals k and remove them from the array.
Return the maximum number of operations you can perform on the array.

```cpp
int maxOperations(vector<int>& nums, int k) {
    sort(nums.begin(), nums.end());
    int i=0, j=nums.size()-1;
    int ans = 0;
    while(i<j){
        if(nums[i] + nums[j] == k){
            ans++;
            i++; j--;
        }

        else if(nums[i] + nums[j] > k)
            j--;

        else
            i++;
    }
    return ans;
}
```

## Hard

#### 1. Trapping Rain Water (Best approach using two pointers) - O(n) | O(1)

[Video Solution](https://www.youtube.com/watch?v=C8UjlJZsHBw)

Hint: Use two pointers left and right, and two variables maxLeft and maxRight. Now iterate over the array and calculate trapped water on each index using following logic:
1. If maxLeft < maxRight => maxLeft will be used for calculating water at that index => if height[i] > maxLeft then update maxLeft else calculate trapped water using maxLeft - height[i] and do left++;
2. if maxRight > maxLeft => maxRight will be used => use same above logic

```cpp
int trap(vector<int>& height) {
    int n = height.size();
    if(n<3) return 0;

    int left = 1, right = n-2;
    int maxLeft = height[0], maxRight = height[n-1];

    int water = 0;

    while(left<=right){

        if(maxLeft<=maxRight){
            if(height[left]>maxLeft){
                maxLeft = height[left];
                left++;
            }
            else{
                water += maxLeft - height[left];
                left++;
            }
        }
        else{
            if(height[right]>maxRight){
                maxRight = height[right];
                right--;
            }
            else{
                water += maxRight - height[right];
                right--;
            }
        }
    }

    return water;
}
```
