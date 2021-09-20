# Problems on Monotonous Increasing/Decreasing Stack 

**Patterns: NGL | NGR | NSL | NSR**

<br>

### 1. Daily Temperatures
Given a list of daily temperatures temperatures, return a list such that, for each day, how many days you would have to wait until a warmer temperature. If there is no future day for which this is possible, put 0 instead. 

Input: temperatures = [73, 74, 75, 71, 69, 72, 76, 73]

Output: [1, 1, 4, 2, 1, 1, 0, 0]

```cpp
vector<int> dailyTemperatures(vector<int>& T) {
    stack<pair<int,int>> s;
    int n = T.size();
    vector<int> res (n, 0);
    s.push({T[n-1], n-1});

    for(int i=n-2; i>=0; i--){            
        while(!s.empty() && s.top().first<=T[i]) s.pop();

        if(s.empty()) res[i] = 0;
        else res[i] = s.top().second - i;

        s.push({T[i], i});
    }
    return res;
}
```

<br>

### 2. Find the Most Competitive Subsequence (Very Tricky)
A subsequence 'a' is more competitive than 'b' (of the same length) if in the first position where a and b differ, subsequence 'a' has number less than the corresponding number in b. For example, [1,3,4] is more competitive than [1,3,5].

Hint: Create a monotonic increasing stack. Pop out elements from the stack till stack.top() is less then arr[i] and we have enough elements left in array to maintain size k. Else push if k>0.

```cpp
vector<int> mostCompetitive(vector<int>& nums, int k) {
    stack<int> stk;
    int n = nums.size();

    for(int i=0; i<n; i++){

        while(!stk.empty() && stk.top()>nums[i] && n-i>k){
            stk.pop();
            k++;
        }

        if(k>0){
            stk.push(nums[i]);
            k--;
        }
    }

    vector<int> res;
    while(!stk.empty()){
        res.push_back(stk.top());
        stk.pop();
    }

    reverse(res.begin(), res.end());
    return res;  
}
```

<br>

### 3. Remove K Digits (Very Tricky)
Given string num representing a non-negative integer num, and an integer k, return the smallest possible integer after removing k digits from num.

[Video Solution](https://www.youtube.com/watch?v=r_OyrbYWP1M)

Hint: Maintain a monotonic increasing stack. Find running peaks from left to right and delete k peaks. (Running peaks means also check for new peak that is created after deletion of some element). Remove few elements from back if still k>0. 

** alert for boundary cases **

```cpp
string removeKdigits(string num, int k) {
    int n = num.size();
    stack<char> stk;
    stk.push(num[0]);

    int i=1;
    while(i<n){

        while(!stk.empty() && num[i]<stk.top() && k>0){
            stk.pop();
            k--;
        }

        if(num[i]=='0' && stk.empty()){
            i++; 
            continue;
        } 

        stk.push(num[i]);        
        i++;
    }

    while(!stk.empty() && k>0){
        stk.pop();
        k--;
    }

    if(stk.empty()) return "0";

    string res = "";
    while(!stk.empty()){
        res.push_back(stk.top());
        stk.pop();
    }

    reverse(res.begin(), res.end());
    return res;
}
```

<br>

### 4. Stock span problem 
we have a series of n daily price quotes for a stock and we need to calculate the span of stock’s price for all n days. The span of the stock’s price on a given day i is defined as the maximum number of consecutive days just before the given day, for which the price of the stock on the current day is less than or equal to its price on the given day.

Hint : Push indexes instead of actual elements

```cpp
vector <int> calculateSpan(int price[], int n){
    vector<int> res (n, 1);
    stack<int> stk;
    stk.push(0);

    for(int i=1; i<n; i++){
        while(!stk.empty() && price[stk.top()]<=price[i])
            stk.pop();

        if(!stk.empty())
            res[i] = i-stk.top();
        else
            res[i] = i+1;

        stk.push(i);
    }
    return res;
}
```

<br>

### 5. Largest Rectangle in Histogram 
Given an array of integers heights representing the histogram's bar height where the width of each bar is 1, return the area of the largest rectangle in the histogram.

![hist](https://assets.leetcode.com/uploads/2021/01/04/histogram.jpg)

Hint: Find NSL and NSR for calculating width for each index. Then area for each index will be width * heights[i].

```cpp
vector<int> nsl, nsr;

void NSL(vector<int> & heights, int n){
    stack<int> stk;

    for(int i=0; i<n; i++){
        while(!stk.empty() && heights[stk.top()]>=heights[i]) stk.pop();

        if(stk.empty()) nsl.push_back(0);
        else nsl.push_back(stk.top()+1);

        stk.push(i);
    }
}

void NSR(vector<int> & heights, int n){
    stack<int> stk;
    vector<int> temp (n, 0);
    nsr = temp;

    for(int i=n-1; i>=0; i--){
        while(!stk.empty() && heights[stk.top()]>=heights[i]) stk.pop();

        if(stk.empty()) nsr[i] = n-1;
        else nsr[i] = stk.top()-1;

        stk.push(i);
    }
}

int largestRectangleArea(vector<int>& heights) {
    int n = heights.size();
    int maxArea = 0;
    NSL(heights, n); 
    NSR(heights, n);

    for(int i=0; i<n; i++){
        int width = (i - nsl[i]) + (nsr[i] - i) + 1;
        int area = heights[i] * width;
        maxArea = max(area, maxArea);
    }
    return maxArea;
}
```

<br>

### 6. Largest Rectangle in Binary Matrix

Hint : Use concept of Largest area in Histogram

```cpp
void NSL(vector<int> & heights, vector<int> & nsl, int n){
    stack<int> stk;

    for(int i=0; i<n; i++){
        while(!stk.empty() && heights[stk.top()]>=heights[i]) 
            stk.pop();

        if(stk.empty()) nsl.push_back(0);
        else nsl.push_back(stk.top()+1);

        stk.push(i);
    }
}

void NSR(vector<int> & heights, vector<int> & nsr, int n){
    stack<int> stk;
    nsr = vector<int> (n, 0);

    for(int i=n-1; i>=0; i--){
        while(!stk.empty() && heights[stk.top()]>=heights[i]) 
            stk.pop();

        if(stk.empty()) nsr[i] = n-1;
        else nsr[i] = stk.top()-1;

        stk.push(i);
    }
}

int largestRectangleArea(vector<int> & heights) {
    vector<int> nsl, nsr;
    int n = heights.size();

    NSL(heights, nsl, n); 
    NSR(heights, nsr, n);

    int maxArea = 0;

    for(int i=0; i<n; i++){
        int width = (i - nsl[i]) + (nsr[i] - i) + 1;
        int area = heights[i] * width;
        maxArea = max(area, maxArea);
    }

    return maxArea;
}

int maximalRectangle(vector<vector<char>>& matrix) {
    int row = matrix.size();
    if(row==0) return 0;

    int col = matrix[0].size();
    vector<int> arr(col, 0);

    for(int j=0; j<col; j++)
        arr[j] = matrix[0][j]-'0';

    int ans = largestRectangleArea(arr);

    for(int i=1; i<row; i++){

        for(int j=0; j<col; j++)
            arr[j] = matrix[i][j] == '0' ? 0 : arr[j] + (matrix[i][j]-'0');

        int area = largestRectangleArea(arr);
        ans = max(ans, area);
    }

    return ans;
}
```

<br>

### 7. Number of Visible People in a Queue
There are n people standing in a queue. You are given an array heights of distinct integers where heights[i] represents the height of the ith person. A person can see another person to their right in the queue if everybody in between is shorter than both of them. More formally, the ith person can see the jth person if i < j and min(heights[i], heights[j]) > max(heights[i+1], heights[i+2], ..., heights[j-1]).

Return an array answer of length n where answer[i] is the number of people the ith person can see to their right in the queue.

![img](https://assets.leetcode.com/uploads/2021/05/29/queue-plane.jpg)

Input: heights = [10,6,8,5,11,9]

Output: [3,1,2,1,1,0]

```cpp
vector<int> canSeePersonsCount(vector<int>& heights) {
    int n = heights.size();
    vector<int> res (n, 0);

    stack<int> stk;
    stk.push(heights.back());

    for(int i=n-2; i>=0; i--){
        int cnt = 0;

        while(!stk.empty() and stk.top()<heights[i]){
            stk.pop();
            cnt++;
        }

        if(!stk.empty()) cnt++; 

        res[i] = cnt;
        stk.push(heights[i]);
    }

    return res;
}
```

<br>

### 8. Next Greater Element II (Circular Array)
Given a circular integer array nums , return the next greater number for every element in nums. If it doesn't exist, return -1 for this number.

Input: nums = [1,2,1]

Output: [2,-1,2]

**Approach 1: Find NGR and LeftMost greater (if NGR doesn't exist) - O(nlogn) | O(n)**

Approach: 
1. First find NGR for every element. 
2. Then for all those element whose NGR doesn't exist, find leftMost_greater element (by sorting in descending order and find leftmost greater index) 

```cpp
/* NGR: Nearest Greater Right */

vector<int> NGR (vector<int> &nums){
    int n = nums.size();
    vector<int> ngr (n, INT_MIN);

    stack<int> stk;
    stk.push(nums[n-1]);

    for(int i=n-2; i>=0; i--){

        while(!stk.empty() and stk.top()<=nums[i])
            stk.pop();

        if(!stk.empty())
            ngr[i] = stk.top();

        stk.push(nums[i]);
    }

    return ngr;
}


/* It will return leftmost greater element for those indexes who do not have greater element in right */

void leftmostGrt (vector<int> &nums, vector<int> & res){
    vector<pair<int,int>> v;
    for(int i=0; i<nums.size(); i++)
        v.push_back({nums[i], i});

    sort(v.begin(), v.end(), greater<pair<int,int>>());

    int mx = v[0].first;
    int mx_idx = v[0].second;

    for(int i=1; i<nums.size(); i++){
        int idx = v[i].second;

        if(res[idx]==INT_MIN and mx>v[i].first)
            res[idx] = mx;

        if(v[i].second < mx_idx){
            mx = v[i].first;
            mx_idx = v[i].second;
        }
    }
}


vector<int> nextGreaterElements(vector<int>& nums) {
    vector<int> res = NGR (nums);
    leftmostGrt(nums, res);

    for(int i=0; i<res.size(); i++)
        if(res[i]==INT_MIN)
            res[i] = -1;

    return res;
}
```

<br>

**Approach 2: Loop Twice - O(n) | O(n)**

Loop Twice, to get the Next Greater Number of a circular array.

```cpp
vector<int> nextGreaterElements(vector<int>& nums) {
    int n = nums.size();
    vector<int> res (n, -1);
    stack<int> stk;

    for(int i=2*n-1; i>=0; i--){

        int idx = i%n;          // --> This idx will be used as an index

        while(!stk.empty() and stk.top()<=nums[idx])
            stk.pop();

        if(!stk.empty())
            res[idx] = stk.top();

        stk.push(nums[idx]);
    }

    return res;
}
```
