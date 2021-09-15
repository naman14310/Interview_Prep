# Two Pointers 

## Easy

#### 1. Reverse Only Letters
Given a string s, reverse the string according to the following rules:
1. All the characters that are not English letters remain in the same position.
2. All the English letters (lowercase or uppercase) should be reversed.

```cpp
string reverseOnlyLetters(string s) {
    int i=0, j=s.length();

    while(i<j){

        if(isalpha(s[i]) and isalpha(s[j])){
            swap(s[i], s[j]);
            i++; j--;
        }

        if(!isalpha(s[i])) i++;
        if(!isalpha(s[j])) j--;
    }

    return s;
}
```

<br>

#### 2. Move Zeroes
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

<br>

#### 3. Remove Element
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

<br>

#### 4. Remove Duplicates from Sorted Array
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

<br>

#### 5. Remove Duplicates from Sorted Array II
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

<br>

#### 6. Merge Sorted Array
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

<br>

#### 7. Intersection of Two Arrays II
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

<br>

#### 8. Backspace String Compare
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

<br>

#### 9. Squares of a Sorted Array 
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

<br>

#### 10. Is Subsequence
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

<br>

#### 11. Valid Palindrome II
Given a string s, return true if the s can be palindrome after deleting at most one character from it.

```cpp
bool validPalindrome(string s) {
    int n = s.length();
    if(n<3) return true;

    int i=0, j=n-1;

    while(i<j){

        /* When we found the 1st unmatched pair, two cases arises */

        if(s[i]!=s[j]){

            /* Case 1 : skip ith character and check for rest */

            int i1 = i+1, j1 = j;

            while(i1<j1 and s[i1]==s[j1]){
                i1++; j1--;
            }

            /* Case 2 : skip jth character and check for rest */

            int i2 = i, j2 = j-1;

            while(i2<j2 and s[i2]==s[j2]){
                i2++; j2--;
            }

            return i1>=j1 or i2>=j2;       // --> if in any of the cases, ptr crosses each other, string will be palindrome
        }

        i++; j--;

    }

    return true;
}
```

<br>

#### 12. Container With Most Water

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

<br>

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

<br>

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

<br>

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

<br>

#### 4. Count triplets with sum smaller than X

```cpp
long long countTriplets(long long arr[], int n, long long sum){
      sort(arr, arr+n);
      long long count = 0;
      
      for(int i=0; i<n-2; i++){
          int j=i+1, k=n-1;
          while(j<k){
              if(arr[i]+arr[j]+arr[k]<sum){
                  count+=k-j;
                  j++;
              } 
              else k--;
          }
      }
      return count;
}
```

<br>

#### 5. Boats to Save People
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

<br>

#### 6. Max Number of K-Sum Pairs (Two sum in other words)
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

<br>

#### 7. Valid Triangle Number
Given an integer array nums, return the number of triplets chosen from the array that can make triangles if we take them as side lengths of a triangle.

Theorem: In a triangle, the length of any side is less than the sum of the other two sides.

Hint : Largest side of triangle should be less then other two sides.

```cpp
int twoSum (vector<int> & nums, int i, int j, int k){
    int pairs = 0;

    while(i<j){
        if(nums[i]+nums[j] > nums[k]){
            pairs += j-i;
            j--;
        }
        else i++;
    }

    return pairs;
}


int triangleNumber(vector<int>& nums) {
    sort(nums.begin(), nums.end());
    int triangle = 0;

    for(int k=nums.size()-1; k>1; k--)
        triangle += twoSum (nums, 0, k-1, k);

    return triangle;
}
```

<br>

#### 8. 4Sum
Given an array nums of n integers, return an array of all the unique quadruplets [nums[a], nums[b], nums[c], nums[d]] such that:
1. 0 <= a, b, c, d < n
2. a, b, c, and d are distinct.
3. nums[a] + nums[b] + nums[c] + nums[d] == target

You may return the answer in any order.

```cpp
vector<vector<int>> fourSum(vector<int>& nums, int target) {
    int n = nums.size();
    vector<vector<int>> res;

    if(n<4) return res;

    sort(nums.begin(), nums.end());

    for(int i=0; i<n; i++){

        if(i>0 and nums[i]==nums[i-1]) continue;

        for(int j=i+1; j<n; j++){

            if(j>i+1 and nums[j]==nums[j-1]) continue;

            long long sum = nums[i] + nums[j];
            int l = j+1, r = n-1;

            while(l<r){

                if(sum+nums[l]+nums[r]==target){
                    res.push_back({nums[i], nums[j], nums[l], nums[r]});
                    l++; r--;

                    while(l<r and nums[l]==nums[l-1]) l++;
                    while(l<r and nums[r]==nums[r+1]) r--;
                }

                else if(sum+nums[l]+nums[r]>target)
                    r--;

                else l++;

            }
        }
    }

    return res;
}
```

<br>

#### 9. Sort Array By Parity (Inplace)
Given an integer array nums, move all the even integers at the beginning of the array followed by all the odd integers. Return any array that satisfies this condition.

Input: nums = [3,1,2,4]

Output: [2,4,3,1]

```cpp
vector<int> sortArrayByParity(vector<int>& nums) {
    int n = nums.size();
    int i=0, j=n-1;

    while(i<j){

        if(nums[i]%2==0) i++;

        else{
            swap(nums[i], nums[j]);
            j--;
        }
    }

    return nums;
}
```

<br>

#### 10. Sort Array By Parity II (Inplace)
Given an array of integers nums, half of the integers in nums are odd, and the other half are even. Sort the array so that whenever nums[i] is odd, i is odd, and whenever nums[i] is even, i is even. Return any answer array that satisfies this condition.

Input: nums = [4,2,5,7]

Output: [4,5,2,7]

```cpp
vector<int> sortArrayByParityII(vector<int>& nums) {

    int i=0;

    while(i<nums.size()){

        if((i%2==0 and nums[i]%2==0) or (i%2==1 and nums[i]%2==1))
            i++;

        else{

            int j = i+1;

            while(j<nums.size()){

                if(i%2==0 and j%2==1 and nums[j]%2==0){
                    swap(nums[i], nums[j]);
                    break;
                }

                else if(i%2==1 and j%2==0 and nums[j]%2==1){
                    swap(nums[i], nums[j]);
                    break;
                }

                else j++;
            }

            i++;
        }

    }

    return nums;
}
```

<br>

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

<br>

#### 2. Next Smallest Palindrome
Given a numeric string A representing a large number you need to find the next smallest palindrome greater than this number.

Input: A = "23545"

Output: "23632"

Hint: There can be three different types of inputs that need to be handled separately.
1. The input number is palindrome and has all 9s. For example “9 9 9”. Output should be “1 0 0 1”
2. The input number is not palindrome. For example “1 2 3 4”. Output should be “1 3 3 1”
3. The input number is palindrome and doesn’t have all 9s. For example “1 2 2 1”. Output should be “1 3 3 1”.

```cpp
string Solution::solve(string A) {

    /* ------------------ Case1 : If all digits are 9 ------------------- */

    bool all9 = true;

    for(char ch: A){
        if(ch!='9'){
            all9 = false;
            break;
        }
    }

    if(all9){
        A[0] = '1';
        for(int i=1; i<A.length(); i++) A[i] = '0';
        A.push_back('1');
        return A;
    }


    /* --------------------- Case2 : If not all 9 ----------------------- */


    int i=0, j=A.size()-1;
    bool smaller = true;
    
    /* Iterate i from left to right and j from right to left. And if A[i]!=A[j], update A[j] = A[i] */

    while(i<j){
        if(A[i]==A[j]){
            i++; j--;
            continue;
        } 

        if(A[j]>=A[i]) smaller = true;
        else smaller = false;

        A[j] = A[i];
    }
    
    /* If the new formed number is smaller then prev */

    if(smaller){
    
        /* 
            Continue iteration, if A[i] == 9 then make it 0 and continue
            Else Update both A[i] and A[j] to A[i]+1 and break the loop
        */
    
        while(i>=0 and j<A.size()){
            if(A[i]=='9'){
                A[i]='0'; A[j]='0'; 
                i++; j--;
                continue;
            }

            int num = (A[i]-'0') + 1;
            A[i] = num + '0';
            A[j] = num + '0'; 
            break;
        }
    }

    return A;
}
```
