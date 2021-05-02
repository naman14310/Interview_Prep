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

#### 4. Merge Sorted Array
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

#### 5. Intersection of Two Arrays II
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

#### 6. Backspace String Compare
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

#### 7. Squares of a Sorted Array 
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

#### 8. Is Subsequence
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
