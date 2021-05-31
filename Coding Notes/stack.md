# Stack

## Easy

#### 1. Implement Stack using queues

```cpp
queue<int> q1, q2;

MyStack() {
}

/* Push element x onto stack. */

void push(int x) {
    if(!q2.empty()) q2.push(x);
    else q1.push(x);
}

/* Removes the element on top of the stack and returns that element. */

int pop() {
    if(!q1.empty()){
        while(q1.size()>1){
            cout<<q1.front()<<endl;
            q2.push(q1.front());
            q1.pop();
        }
        int x = q1.front();
        q1.pop();
        return x;
    }
    else{
        while(q2.size()>1){
            q1.push(q2.front());
            q2.pop();
        }
        int x = q2.front();
        q2.pop();
        return x;
    }
}

/* Get the top element. */

int top() {
    if(!q1.empty()){
        while(q1.size()>1){
            cout<<q1.front()<<endl;
            q2.push(q1.front());
            q1.pop();
        }
        int x = q1.front();
        q2.push(x);
        q1.pop();
        return x;
    }
    else{
        while(q2.size()>1){
            q1.push(q2.front());
            q2.pop();
        }
        int x = q2.front();
        q1.push(x);
        q2.pop();
        return x;
    }
}

/* Returns whether the stack is empty. */

bool empty() {
    if(q1.empty() && q2.empty()) return true;
    else return false;
}
```

#### 2. Min Stack (Tricky)
Hint: Use two stacks. One for storing elements in order. Second for storing min element so far.

```cpp
/* Time Complexity: O(1) | Space Complexity: O(n) */

stack<int> s1;
stack<int> s2;

MinStack() {
}

void push(int val) {
    s1.push(val);
    if(s2.empty()){
        s2.push(val);
    }
    else{
        if(val<s2.top()) s2.push(val);
        else s2.push(s2.top());
    }
}

void pop() {
    s1.pop(); s2.pop();
}

int top() {
    return s1.top();
}

int getMin() {
    return s2.top();
}
```

More optimized solution with O(1) Space Complexity

[Video Solution](https://www.youtube.com/watch?v=ZvaRHYYI0-4)

```cpp
stack<long long int> s;
long long int minItem = INT_MAX;

MinStack() {
}

void push(int val) {
    if(s.empty()){
        s.push(val);
        minItem = val;
    }
    else{
        if(val<minItem){
            s.push(2*val - minItem);
            minItem = val;
        }
        else s.push(val);
    }
}

void pop() {
    if(s.top()<minItem)
        minItem = 2*minItem - s.top();
    s.pop();
}

int top() {
    return s.top();
}

int getMin() {
    return minItem;
}
```

#### 3. Validate Stack Sequences
Given two sequences pushed and popped with distinct values, return true if and only if this could have been the result of a sequence of push and pop operations on an initially empty stack.

```cpp
bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
    stack<int> s;
    int i=0, j=0;

    while(i<pushed.size()){
        if(!s.empty() && s.top()==popped[j]){
            s.pop();
            j++;
        }
        else{
            s.push(pushed[i]);
            i++;
        }
    }

    while(j<popped.size() && s.top()==popped[j]){
        s.pop();
        j++;
    } 

    if(s.empty()) return true;
    return false;
}
```

#### 4. Evaluate Reverse Polish Notation
Input: tokens = ["4","13","5","/","+"]

Output: 6

Explanation: (4 + (13 / 5)) = 6

```cpp
int evalRPN(vector<string>& tokens) {

    stack<int> stk;
    for(string s : tokens){

        if(s=="+" || s=="-" || s=="*" || s=="/"){
            int num2 = stk.top(); stk.pop();
            int num1 = stk.top(); stk.pop();

            int res = 0;
            if(s=="+") res = num1+num2;
            else if(s=="-") res = num1-num2;
            else if (s=="*") res = num1*num2;
            else res = num1/num2;
            stk.push(res);
        }
        else
            stk.push(stoi(s));
    }
    return stk.top();
}
```

#### 5. Insert at the end of Stack without using any Data structure

Hint : Use recursion

```cpp
void insert_at_end(stack<int> & stk, int val){
    if(stk.empty()){
        stk.push(val);
        return;
    }

    int item = stk.top();
    stk.pop(); 
    insert_at_end(stk, val);
    stk.push(item);
}
```

#### 6. Reverse a stack without using any Data structure

```cpp
void reverse_stack(stack<int> & stk){
    if(stk.empty()) 
        return;

    int item = stk.top();
    stk.pop();

    reverse_stack(stk);
    insert_at_end(stk, item);   // Use above function for insert_at_end()
}
```
## Medium

### @ Problems on Monotonous Increasing Stack (NGL|NGR|NSL|NSR)

#### 1. Daily Temperatures
Given a list of daily temperatures temperatures, return a list such that, for each day in the input, tells you how many days you would have to wait until a warmer temperature. If there is no future day for which this is possible, put 0 instead. For example, given the list of temperatures temperatures = [73, 74, 75, 71, 69, 72, 76, 73], your output should be [1, 1, 4, 2, 1, 1, 0, 0].

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

#### 2. Find the Most Competitive Subsequence (Very Tricky)
Given an integer array nums and a positive integer k, return the most competitive subsequence of nums of size k. An array's subsequence is a resulting sequence obtained by erasing some (possibly zero) elements from the array. We define that a subsequence a is more competitive than a subsequence b (of the same length) if in the first position where a and b differ, subsequence a has a number less than the corresponding number in b. For example, [1,3,4] is more competitive than [1,3,5] because the first position they differ is at the final number, and 4 is less than 5.

Hint: Create a monotonic increasing stack. Pop out elements from the stack till stack.top() is less then arr[i] and  we have enough elements left in array to maintain size k. Else push if k>0.

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

#### 3. Remove K Digits (Very Tricky)
Given string num representing a non-negative integer num, and an integer k, return the smallest possible integer after removing k digits from num.

[Video Solution](https://www.youtube.com/watch?v=r_OyrbYWP1M)

Hint: Maintain a monotonic increasing stack. Find running peaks from left to right and delete k peaks. (Running peaks means also check for new peak that is created after deletion of some element). Remove few elements from back if still k>0. ** check for boundary cases **

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

#### 4. Stock span problem 

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

#### 5. Largest Rectangle in Histogram 
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

#### 6. Largest Rectangle in Binary Matrix

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


### @ Parenthesis Based Questions

#### 1. Valid Parentheses

```cpp
bool isValid(string s) {
    stack<char> stk;
    for(char ch : s){
        if(ch=='[' || ch=='(' || ch=='{'){
            stk.push(ch);
        }
        else if(ch==']'){
            if(stk.empty())return false;
            if(stk.top()=='[') stk.pop();
            else return false;
        }
        else if(ch==')'){
            if(stk.empty())return false;
            if(stk.top()=='(') stk.pop();
            else return false;
        }
        else if(ch=='}'){
            if(stk.empty())return false;
            if(stk.top()=='{') stk.pop();
            else return false;
        }
    }

    if(stk.empty()) return true;
    else return false;
}
```

#### 2. Minimum Remove to Make Valid Parentheses

Approach: First iterate from left to right and remove extra ')'  brackets using bracketCount. Similaily, then move from right to left and remove extra '(' brackets using bracketCount.

```cpp
string minRemoveToMakeValid(string s) {
    int n = s.length();
    int bracketCount = 0;
    string s1 = "", s2 = "";

    for(int i=0; i<n; i++){
        if(s[i]=='('){
            s1.push_back(s[i]);
            bracketCount++;
        }
        else if(s[i]==')'){
            if(bracketCount>0){
                s1.push_back(s[i]);
                bracketCount--;
            }
        }
        else s1.push_back(s[i]);
    }

    bracketCount = 0;
    int n2 = s1.length();

    for(int i = n2-1; i>=0; i--){
        if(s1[i]==')'){
            s2.push_back(s1[i]);
            bracketCount++;
        }
        else if(s1[i]=='('){
            if(bracketCount>0){
                s2.push_back(s1[i]);
                bracketCount--;
            }
        }
        else s2.push_back(s1[i]);
    }
    reverse(s2.begin(), s2.end());
    return s2;
}
```

### @ Hard to guess as Stack

#### 1. Remove All Adjacent Duplicates in String II
You are given a string s and an integer k, a k duplicate removal consists of choosing k adjacent and equal letters from s and removing them, causing the left and the right side of the deleted substring to concatenate together. We repeatedly make k duplicate removals on s until we no longer can. Return the final string after all such duplicate removals have been made. It is guaranteed that the answer is unique.

Hint: Use stack to store freq of char while iterating over string. Pop out that character that exceeds freq k.

```cpp
string removeDuplicates(string s, int k) {
    stack<pair<char,int>> stk;
    for(int i=0; i<s.length(); i++){
        if(stk.empty() || stk.top().first != s[i]) 
            stk.push({s[i], 1});

        else if(stk.top().second < k-1){
            int freq = stk.top().second;
            stk.pop();
            stk.push({s[i], freq+1});
        }
        
        else stk.pop();
    }

    string ans = "";

    while(!stk.empty()){
        char ch = stk.top().first;
        int freq = stk.top().second;
        stk.pop();
        for(int i=0; i<freq; i++) 
            ans.push_back(ch);
    }

    reverse(ans.begin(), ans.end());
    return ans;
}
```

#### 2. The Celebrity Problem
A celebrity is a person who is known to all but does not know anyone at a party. If you go to a party of N people, find if there is a celebrity in the party or not.

Approach: 

If A knows B, then A can't be a celebrity. Discard A, and B may be celebrity.

If A doesn't know B, then B can't be a celebrity. Discard B, and A may be celebrity.

```cpp
int celebrity(vector<vector<int> >& matrix, int n) {
    stack<int> stk;

    for(int i=0; i<n; i++)
        stk.push(i);    
    
    while(stk.size()>1){
        int a = stk.top(); stk.pop();
        int b = stk.top(); stk.pop();
        if(matrix[a][b]==0)
            stk.push(a);
        else
            stk.push(b);
    }

    int probable_celebrity = stk.top();

    for(int i=0; i<n; i++)
        if(i!=probable_celebrity && matrix[i][probable_celebrity]==0) return -1;
    
    for(int j=0; j<n; j++)
        if(matrix[probable_celebrity][j]==1) return -1;
    
    return probable_celebrity;
}
```


## Hard

#### 1. Implement k stacks in a single array
The idea is to use two extra arrays for efficient implementation of k stacks in an array. Following are the two extra arrays are used:

1) top[]: This is of size k and stores indexes of top elements in all stacks.

2) next[]: This is of size n and stores indexes of next item for the items in array arr[]. Here arr[] is actual array that stores k stacks.

All entries in top[] are initialized as -1 to indicate that all stacks are empty. All entries next[i] are initialized as i+1 because all slots are free initially and pointing to next slot. Top of free stack, ‘free’ is initialized as 0.

[Explaination](https://www.geeksforgeeks.org/efficiently-implement-k-stacks-single-array/)

```cpp
class Kstack{

    vector<int> stk;
    vector<int> next;
    vector<int> top;
    int next_free;

    public:

        Kstack(int size, int K){
            stk = vector<int> (size, 0);

            for(int i=0; i<size; i++) next.push_back(i+1);
            next[size-1] = -1;

            top = vector<int> (K, -1);
            next_free = 0;            
        }

        void push(int val, int stk_num){
            if(full(stk_num)) return;

            int idx = next_free;
            next_free = next[idx];
            next[idx] = top[stk_num];
            top[stk_num] = idx;

            stk[idx] = val;
        }

        /* POP -> Just write push operations in reverse order */ 

        int pop(int stk_num){
            if(empty(stk_num)) return -1;

            int idx = top[stk_num];
            top[stk_num] = next[idx];
            next[idx] = next_free;
            next_free = idx;

            return stk[idx];
        }

        bool full(int stk_num){
            return next_free == -1;
        }

        bool empty(int stk_num){
            return top[stk_num] == -1;
        }
};
```
