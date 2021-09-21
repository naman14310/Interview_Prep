# Sliding Window

<br>

## @ Type 1 : Fixed Window Size


### 1. Count Occurences of Anagrams
Given a word pat and a text txt. Return the count of the occurences of anagrams of the word in the text.

Hint: Create a count array for storing frequency

```cpp
int count_occurance(string pat, string txt) {
    int ans = 0;
    vector<int> v1 (26, 0);
    vector<int> v2 (26, 0);

    int k = pat.size();
    int n = txt.size();
    int i=0;

    for( ; i<k; i++){
        v1[pat[i]-'a'] += 1;
        v2[txt[i]-'a'] += 1;
    }
    if(v1==v2) ans+=1;

    for( ; i<n; i++){
        v2[txt[i]-'a'] +=1;
        v2[txt[i-k]-'a'] -=1;

        if(v1==v2) ans+=1;
    }
    return ans;
}
```

<br>

### 2. First negative integer in every window of size k

Hint: The idea is to have a variable firstNegativeIndex to keep track of the first negative element in the k sized window.

```cpp
vector<long long> printFirstNegativeInteger(long long int A[], long long int N, long long int k) {  
    vector<long long> v;                                   
    int first_negative_index = -1;
    int next_index = 0;
    
    for(int i=0; i<k; i++){
        if(A[i]<0){
            first_negative_index=i;
            break;
        }
    }
    
    if(first_negative_index != -1){
        v.push_back(A[first_negative_index]);
        next_index = first_negative_index+1;
    }
    else{
        v.push_back(0);
        next_index = k; 
    }
        
    for(int i=1; i<N-k+1; i++){
        
        if(first_negative_index>=i) 
            v.push_back(A[first_negative_index]);
        
        else{
            for(int j=next_index; j<i+k; j++){
                if(A[j]<0){
                    first_negative_index = j;
                    break;
                }
            }
            if(first_negative_index>=i){
                v.push_back(A[first_negative_index]);
                next_index = first_negative_index+1;
            } 
            else{
                v.push_back(0);
                next_index = i+k;
            }
        }
    }
    return v;
 }
```

<br>

### 3. Maximum Points You Can Obtain from Cards (Think in reverse)
There are several cards arranged in a row, and each card has an associated number of points. In one step, you can take one card from the beginning or from the end of the row. You have to take exactly k cards. Given the integer array cardPoints and the integer k, return the maximum score you can obtain.

Input: cardPoints = [1,2,3,4,5,6,1], k = 3

Output: 12

Hint: Similar to minimum sum windows of size (n-k)

```cpp
int maxScore(vector<int>& cardPoints, int k) {
    int totalSum = 0;
    for(int point : cardPoints)
        totalSum += point;

    int window_size = cardPoints.size() - k;

    int i=0;
    int minSum = 0, windowSum = 0;

    for( ; i<window_size; i++)
        windowSum += cardPoints[i];

    minSum = windowSum;

    for( ; i<cardPoints.size(); i++){
        windowSum += cardPoints[i];
        windowSum -= cardPoints[i-window_size];

        minSum = min(minSum, windowSum);
    }

    return totalSum - minSum;
}
```

<br>

### 4. Minimum Number of K Consecutive Bit Flips (Tricky)
In an array nums containing only 0s and 1s, a k-bit flip consists of choosing a (contiguous) subarray of length k and simultaneously changing every 0 in the subarray to 1, and every 1 in the subarray to 0. Return the minimum number of k-bit flips required so that there is no 0 in the array.  If it is not possible, return -1.

Input: nums = [0,0,0,1,0,1,1,0], k = 3

Output: 3

Hint: Do Not flip bits everytime. Instead use one boolean variable reverse to track if bits are reversed. Remember to flip the next bit (if reverse is true) before sliding the window.

```cpp
int minKBitFlips(vector<int>& A, int K) {
    int i=0, j=K-1;
    int flip_count = 0;
    bool rev = false;

    while(j<A.size()){

        if((rev and A[i]==1) or (!rev and A[i]==0)){
            rev = !rev;
            flip_count++;
        }

        if(rev and j+1<A.size())
            A[j+1] = !A[j+1];

        i++; j++;
    }

    while(i<A.size()){
        if((rev and A[i]==1) or (!rev and A[i]==0)) return -1;
        i++;
    }

    return flip_count;
}
```

<br>



## @ Type 2 : Variable Window Size
These type of problems can be solved with two poiters i and j. `i` points to the leftmost element of window (including i) and `j` points to rightmost element of window (including j)

<br>

### 1. Longest Substring Without Repeating Characters

```cpp
int lengthOfLongestSubstring(string s) {
    if(s.length()==0) 
        return 0;

    vector<int> last_index (256, -1);  // It will store index of last occurance
    int i=0, j=0;
    int ans = 1;

    while(j<s.length()){
        char ch = s[j];

        if(last_index[ch]>=i)
            i = last_index[ch]+1;

        last_index[ch] = j;
        ans = max(ans, j-i+1);
        j++;
    }

    return ans;
}
```

<br>

### 2. Longest Substring with atmost k distinct characters

```cpp
int longest_substring(string s, int k){
    unordered_map<char, int> freq;
    int ans=0;
    int i=0, j=0;

    while(j<s.size()){
        char ch = s[j];
        freq[ch]+=1;

        while(freq.size()>k){
            char temp = s[i];
            i++;

            if(freq[temp]>1) freq[temp]--;
            else freq.erase(temp); 
        }
        
        ans = max(ans, j-i+1);
        j++;
    }
    return ans;
}
```

**Similar Questions**

1. Fruit in a Basket (leetcode)
2. Pick toys (aditya verma playlist)

<br>

### 3. Longest Substring with exactly k distinct characters

```cpp
int longestKSubstr(string s, int k) {
    int i=0, j=0;
    int ans=-1;
    vector<int> freq (26, 0);

    while(j<s.length()){
        int idx = s[j]-'a';
        freq[idx]++;

        if(freq[idx]==1) k--;

        while(k<0){
            int idx2 = s[i]-'a';
            freq[idx2]--;

            if(freq[idx2]==0) k++;
            i++;
        }

        if(k==0)
            ans = max(ans, j-i+1);

        j++;
    }

    return ans;
}
```

<br>

### 4. Count Subarrays with sum lies within a Range
Given an array of non negative integers A, and a range (B, C). Find the number of continuous subsequences in the array which have sum S in the range [B, C] or B <= S <= C.

Input: A = [10, 5, 1, 0, 2], B = 6, C = 8

Output: 3

Hint: Count_between(B,C) = Count_atmost(C) - Count_atmost(B-1)

```cpp

int cntSubarray (vector<int> &A, int K){
    int i=0, j=0;
    int sum=0, cnt=0;

    while(j<A.size()){
        sum+=A[j];

        while(sum>K){
            sum -= A[i];
            i++;
        }

        if(i<=j) cnt += j-i+1;

        j++;
    }

    return cnt;
}


int Solution::numRange(vector<int> &A, int B, int C) {
    int cnt1 = cntSubarray(A, C);
    int cnt2 = cntSubarray(A, B-1);
    return cnt1-cnt2;
}
```

<br>

### 5. Count Subarrays with exactly K Different Integers

Input: nums = [1,2,1,3,4], k = 3

Output: 3

Explanation: Subarrays formed with exactly 3 different integers: [1,2,1,3], [2,1,3], [1,3,4].

Hint: exactly(K) = atMost(K) - atMost(K-1)

```cpp
int countAtmost (vector<int> & nums, int k){
    int i=0, j=0;
    unordered_map<int, int> mp;

    int distinct = 0;
    int count = 0;

    while(j<nums.size()){
        int val = nums[j];
        mp[val]++;

        if(mp[val]==1) distinct++;

        while(distinct>k){
            int val2 = nums[i];
            mp[val2]--;

            if(mp[val2]==0) distinct--;
            i++;
        }

        count += j-i+1;
        j++;
    }

    return count;
}


int subarraysWithKDistinct(vector<int>& nums, int k) {
    int ans = countAtmost(nums, k) - countAtmost(nums, k-1);
    return ans;
}
```

<br>

### 6. Longest Repeating Character Replacement (Tricky)
You are given a string s and an integer k. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most k times.

Input: s = "ABAB", k = 2

Output: 4

Hint: Given this, we can apply the at most k changes constraint and maintain a sliding window such that

`(length of substring - number of times of the maximum occurring character in the substring) <= k`

```cpp
int characterReplacement(string s, int k) {
    vector<int> freq (26, 0);
    int ans = 0;
    int i=0, j=0;
    int total_charCount = 0, max_freq_count = 0;

    while(j<s.length()){
        char ch = s[j];
        freq[ch-'A']++;
        total_charCount++;

        if(max_freq_count<freq[ch-'A'])
            max_freq_count = freq[ch-'A'];

        if(total_charCount - max_freq_count > k){
            char temp = s[i];
            freq[temp-'A']--;
            total_charCount--;
            i++;
        }

        ans = max(ans, j-i+1);
        j++;
    }
    return ans;
}
```

<br>

### 7. Smallest Distinct Window
Given a string 's'. The task is to find the smallest window length that contains all the characters of the given string at least one time. For eg. A = “aabcbcdbca”, then the result would be 4 as of the smallest window will be “dbca”.

```cpp
string smallestWindow(string s){
    unordered_set<char> st;
    for(auto ch : s) st.insert(ch);
    int k = st.size();            // Number of unique elements

    string ans = s;
    unordered_map<int,int> freq;
    int i=0, j=0;

    while(j<s.size()){
        char ch = s[j];
        freq[ch] += 1;

        while(freq.size()==k){
            if(j-i+1<ans.size())
                ans = s.substr(i, j-i+1);

            char temp = s[i];

            if(freq[temp]==1) freq.erase(temp);
            else freq[temp]--;

            i++;
        }

        j++;
    }
    return ans;
}
```

<br>
    
### 8. Minimum Window Substring (IMP)
Given two strings s and t of lengths m and n respectively, return the minimum window substring of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".

Hint: Use two vectors for storing frequency of characters. Use one count variable. Increment count whenever `freq_t[ch]>0 and freq_s[ch]==freq_t[ch]` and decrement it whenever `freq_t[temp]>0 and freq_s[temp]<freq_t[temp]`

[Video explaination](https://www.youtube.com/watch?v=iwv1llyN6mo)

```cpp
string minWindow(string s, string t) {
    if(t.length()>s.length()) 
        return "";

    vector<int> freq_s (128, 0);
    vector<int> freq_t (128, 0);

    for(char ch : t)
        freq_t[ch]++;

    /* No. of distinct characters in t */
    int k=0;            
    for(int i=0; i<128; i++)
        if(freq_t[i]>0) k++;

    /* It stores count of characters which are fulfilled completely so far */
    int count=0; 

    string ans = s;
    bool found = false;
    int i=0, j=0;

    while(j<s.length()){
        char ch = s[j];
        freq_s[ch]++;

        if(freq_t[ch]>0 and freq_s[ch]==freq_t[ch])
            count++;

        while(count==k){
            found = true;
            if(j-i+1 < ans.length())
                ans = s.substr(i, j-i+1);

            char temp = s[i];
            freq_s[temp]--;

            if(freq_t[temp]>0 and freq_s[temp]<freq_t[temp]) count--;

            i++;
        }

        j++;
    }

    if(!found) return "";
    return ans;
}
```

<br>

### 9. Minimum Operations to Reduce X to Zero (Think in Reverse)
You are given an integer array nums and an integer x. In one operation, you can either remove the leftmost or the rightmost element from the array nums and subtract its value from x. Note that this modifies the array for future operations. Return the minimum number of operations to reduce x to exactly 0 if it's possible, otherwise, return -1.

Input: nums = [1,1,4,2,3], x = 5

Output: 2

Hint: Similar to finding longest window having sum = totalSum-x

```cpp
int minOperations(vector<int>& nums, int x) {
    int totalSum = 0;
    for(int num : nums)
        totalSum += num;

    if(totalSum<x) return -1;

    int required_windowSum = totalSum-x;
    int windowSize = 0;
    int sum = 0; 
    int i=0, j=0;
    bool found = false;

    while(j<nums.size()){
        sum += nums[j];

        while(sum>required_windowSum){
            sum -= nums[i];
            i++;
        }

        if(sum==required_windowSum){
            found = true;
            windowSize = max(windowSize, j-i+1);
        }

        j++;
    }

    if(!found) return -1;
    return nums.size()-windowSize;
}
```

<br>

### 10. Max Consecutive Ones III
Given a binary array nums and an integer k, return the maximum number of consecutive 1's in the array if you can flip at most k 0's.

```cpp
    int longestOnes(vector<int>& nums, int k) {
        int ans = 0;
        int i=0, j=0;
        int zero_count = 0;
        
        while(j<nums.size()){
            if(nums[j]==0) zero_count++;
            
            while(zero_count>k){
                if(nums[i]==0) zero_count--;
                i++;
            }
            
            ans = max(ans, j-i+1);
            j++;
        }
        return ans;
    }
```

<br>

### 11. Get Equal Substrings Within Budget
You are given two strings s and t of the same length. You want to change s to t. Changing the i-th character of s to i-th character of t costs |s[i] - t[i]| that is, the absolute difference between the ASCII values of the characters. You are also given an integer maxCost. Return the maximum length of a substring of s that can be changed to be the same as the corresponding substring of t with a cost less than or equal to maxCost. If there is no substring from s that can be changed to its corresponding substring from t, return 0.

Input: s = "abcd", t = "bcdf", maxCost = 3

Output: 3

```cpp
int equalSubstring(string s, string t, int maxCost) {
    int ans = 0;
    int i=0, j=0;
    int cost = 0;

    while(j<s.length()){
        cost += abs(s[j]-t[j]);

        while(cost>maxCost){
            cost -= abs(s[i]-t[i]);
            i++;
        }

        ans = max(ans, j-i+1);
        j++;
    }
    return ans;
}
```

<br>

### 12. Smallest Range Covering Elements from K Lists (Too Tricky)
You have k lists of sorted integers. Find the smallest range that includes at least one number from each of the k lists.

Input: nums = [[4,10,15,24,26],[0,9,12,20],[5,18,22,30]]

Output: [20,24]

Approach: We can break this problem into two parts:
1. Create a single sorted vector containing numbers from all list and respective listIDs
2. Now this problem is reduced to -> find minimum window containing all listIDs (which is similar to minimum window substring)

```cpp
vector<int> smallestRange(vector<vector<int>>& nums) {

    int k = nums.size();            // --> k is total number of lists

    /* ---------------------------------- Part 1 ------------------------------------ */

    // --> Create a single sorted vector containing numbers from all list  

    vector<pair<int,int>> v;        // --> vector of pair {num, listID}

    for(int i=0; i<nums.size(); i++){

        for(int num : nums[i])
            v.push_back({num, i});
    }

    sort(v.begin(), v.end());

    /* ---------------------------------- Part 2 ------------------------------------- */

    // --> find minimum window containing all listIDs 

    int i=0, j=0;
    vector<int> freq (k, 0);

    int cnt = 0;
    int minRange = INT_MAX;
    vector<int> ans;

    while(j<v.size()){
        int id = v[j].second;

        freq[id]++;
        if(freq[id]==1) cnt++;

        while(cnt==k){

            if(v[j].first - v[i].first < minRange){
                minRange = v[j].first - v[i].first;
                ans = {v[i].first, v[j].first};
            }

            int idi = v[i].second;

            freq[idi]--;
            if(freq[idi]==0) cnt--;

            i++;
        }

        j++;
    }

    return ans;
}
```
