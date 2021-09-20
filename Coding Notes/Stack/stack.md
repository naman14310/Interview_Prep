# Stack

<br>

## @ Stack Simulation

### 1. Insert at the end of Stack without using any Data structure

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

<br>


### 2. Reverse a stack without using any Data structure

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

<br>


### 3. Validate Stack Sequences
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

<br>


### 4. Evaluate Reverse Polish Notation
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

<br>

### 5. Simplify Path
(Question)[https://leetcode.com/problems/simplify-path/]

Input: "/a//b////c/d//././/.."

Output: "/a/b/c"

```cpp
string simplifyPath(string path) {
    string res = "", word = "";
    stack<string> stk;


    for(char ch : path){

        if(ch=='/'){

            if(word!=""){
                if(word==".") word = "";

                else if(word==".."){
                    if(!stk.empty()) stk.pop();
                    if(!stk.empty()) stk.pop();
                    word = "";
                }

                else{
                    reverse(word.begin(), word.end());
                    stk.push(word);
                    word = "";
                }
            }

            if(stk.empty() or stk.top()!="/")
                stk.push("/");
        }

        else word.push_back(ch);
    }


    if(word!="" and word!="."){
        if(word==".."){
            if(!stk.empty()) stk.pop();
            if(!stk.empty()) stk.pop();
        }
        else{
            reverse(word.begin(), word.end());
            stk.push(word);
        }
    }


    while(!stk.empty()){
        res += stk.top();
        stk.pop();
    }

    reverse(res.begin(), res.end());

    if(res.size()==0) res.push_back('/');

    if(res.size()>1 and res.back()=='/') res.pop_back();
    return res;

}
```

<br>


## @ Parenthesis Based Questions

### 1. Valid Parentheses

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

<br>

### 2. Minimum Remove to Make Valid Parentheses

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

<br>

### 3. Longest Valid Parenthesis (Very Tricky)
Given a string consisting of opening and closing parenthesis, find the length of the longest valid parenthesis substring.

[Explaination](https://www.geeksforgeeks.org/length-of-the-longest-valid-substring/)

Algo:

1. Create an empty stack and push -1 to it. The first element of the stack is used to provide a base for the next valid string. 
2. Initialize result as 0.
3. If the character is '(' i.e. str[i] == '('), push index'i' to the stack. 
4. Else (if the character is ')')

   a) Pop an item from the stack (Most of the time an opening bracket)
   
   b) If the stack is not empty, then find the length of current valid substring by taking the difference between the current index and top of the stack. If current length is more than the result, then update the result.
   
   c) If the stack is empty, push the current index as a base for the next valid substring.

5. Return result.

```cpp
int longestValidParentheses(string s) {
    if(s.length()<2) 
        return 0;

    stack<int> stk;
    stk.push(-1);

    int ans = 0;
    for(int i=0; i<s.length(); i++){
    
        if(s[i]=='(') 
            stk.push(i);
            
        else{
            stk.pop();
            
            /* If stack becomes empty then this ')' was not valid and hence now this index will become base for next valid string */
            
            if(stk.empty()) 
                stk.push(i);
            else
                ans = max(ans, i-stk.top());
        }
    }
    return ans;
}
```

<br>


### 4. Score of Parentheses (Tricky)
Given a balanced parentheses string s, compute the score of the string based on the following rule:
1. () has score 1
2. AB has score A + B, where A and B are balanced parentheses strings.
3. (A) has score 2 * A, where A is a balanced parentheses string.

```cpp
/* -1 denotes pushing open bracket in stack */

int scoreOfParentheses(string s) {
    stack<int> stk;

    for(int i=0; i<s.length(); i++){

        if(s[i]=='(') stk.push(-1);

        else{
            if(stk.top()==-1){
                stk.pop();
                stk.push(1);
            }
            else{
                int sum = 0;
                while(stk.top()!=-1){
                    sum += stk.top();
                    stk.pop();
                }

                stk.pop();
                stk.push(2*sum);
            }
        }
    }

    int ans = 0;
    while(!stk.empty()){
        ans += stk.top();
        stk.pop();
    }

    return ans;
}
```

<br>


## @ Hard to guess as Stack

### 1. Remove All Adjacent Duplicates in String II
You are given a string s and an integer k, a k duplicate removal consists of choosing k adjacent and equal letters from s and removing them, causing the left and the right side of the deleted substring to concatenate together. Return the final string after all such duplicate removals have been made. 

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

<br>

### 2. Remove Duplicate Letters (Smallest Subsequence of Distinct Characters)
Given a string s, remove duplicate letters so that every letter appears once and only once. You must make sure your result is the smallest in lexicographical order among all possible results.

Input: s = "bcabc"

Output: "abc"

```cpp
string removeDuplicateLetters(string s) {

    unordered_map<char, int> mp;     // --> will store running freq of each char
    stack<char> stk;
    vector<bool> vis (26, false);

    string ans = "";

    for(char ch : s)
        mp[ch]++;

    /* one by one iterate for all chars */

    for(char ch : s){

        /* If some char is all vis (i.e already present is stk), decrement the freq and continue */

        if(vis[ch-'a']){
            mp[ch]--;       // --> don't forget to decrement freq before continue
            continue;
        } 

        /* pop till stk.top() is lexically greater then curr_char and its freq >=1 and also mark it unvisited */

        while(!stk.empty() and stk.top()>ch and mp[stk.top()]>=1){
            vis[stk.top()-'a'] = false;
            stk.pop();
        }

        /* push curr char to stk, decrement freq and mark it vis */

        stk.push(ch);
        mp[ch]--;
        vis[ch-'a'] = true;
    }

    while(!stk.empty()){
        ans.push_back(stk.top());
        stk.pop();
    }

    reverse(ans.begin(), ans.end());
    return ans;
}
```

<br>

### 3. The Celebrity Problem
A celebrity is a person who is known to all but does not know anyone at a party. If you go to a party of N people, find if there is a celebrity in the party or not.

Hint: Everytime pop two items from stack untill only one left. Then,
1. If A knows B, then A can't be a celebrity. Discard A, and B may be celebrity.
2. If A doesn't know B, then B can't be a celebrity. Discard B, and A may be celebrity.

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

<br>

### 4. Sum of Subarray Minimums (Too Tricky)
Given an array of integers arr, find the sum of min(b), where b ranges over every (contiguous) subarray of arr. Since the answer may be large, return the answer modulo 10^9 + 7.

Input: arr = [3,1,2,4]

Output: 17

**Approach 1 : Two Nested Loops - O(n2)**

```cpp
/* Total subarrays in an array of len n = n*(n+1)/2 */

int sumSubarrayMins(vector<int>& arr) {
    int n = arr.size();
    int sum = 0;
    int mod = 1000000007;

    for(int i=0; i<n; i++){
        int mn = arr[i];

        for(int j=i; j<n; j++){
            mn = min(mn, arr[j]);
            sum = ((sum%mod) + (mn%mod)) % mod;
        }
    }

    return sum;
}
```

**Approach 2 : Monotonic stack and concept of Subarrays - O(n)**
```cpp
/* 
    Intuition : For every element at index i, find the nearest left element and nearest right element 
    This will become the range of subarray in which this element will contribute as min to whole sum

    Count of total subarrs in which any element i will occur as min = (NSL+1)*(NSR+1)
*/


int sumSubarrayMins(vector<int>& arr) {
    int n = arr.size();
    int sum = 0;
    int M = 1000000007;

    stack<int> stk;
    vector<int> left (n, 0);
    vector<int> right (n, 0);

    /* Filling left */

    stk.push(0);

    for(int i=1; i<n; i++){

        while(!stk.empty() and arr[stk.top()]>arr[i])
            stk.pop();

        /* We only need max numbers between arr[i] and NSL */

        if(stk.empty()) left[i] = i;
        else left[i] = i-stk.top()-1;

        stk.push(i);
    }

    stk = stack<int>();          // --> This will reintialize stk with empty container and act as clear() method

    /* Filling Right */

    stk.push(n-1);

    for(int i=n-2; i>=0; i--){

        /* ALERT : While filing right, use equals to sign (which was not in left) for handling duplicates */

        while(!stk.empty() and arr[stk.top()]>=arr[i])          
            stk.pop();

        /* We only need max numbers between arr[i] and NSR */

        if(stk.empty()) right[i] = n-i-1;
        else right[i] = stk.top()-i-1;

        stk.push(i);
    }


    for(int i=0; i<n; i++){

        long total_occurance = ((left[i]+1)%M * (right[i]+1)%M)%M;
        long contribution = ((total_occurance%M) * (arr[i]%M))%M;

        sum = ((sum%M) + (contribution%M)) % M; 
    }

    return sum;
}
```

<br>


## @ Stack Design 

### 1. Implement Stack using queues

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

<br>

### 2. Min Stack (Tricky)
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

<br>

### 3. Design a Stack With Increment Operation (Tricky)
Design a stack which supports the following operations. Implement the CustomStack class:

1. CustomStack(int maxSize) Initializes the object with maxSize which is the maximum number of elements in the stack or do nothing if the stack reached the maxSize.
2. void push(int x) Adds x to the top of the stack if the stack hasn't reached the maxSize.
2. int pop() Pops and returns the top of stack or -1 if the stack is empty.
3. void inc(int k, int val) Increments the bottom k elements of the stack by val. If there are less than k elements in the stack, just increment all the elements in the stack.

[Explaination](https://leetcode.com/problems/design-a-stack-with-increment-operation/discuss/539716/JavaC%2B%2BPython-Lazy-increment-O(1))

**Lazy Increment:** Use an additional array to record the increment value. inc[i] means for all elements stack[0] ~ stack[i], we should plus inc[i] when popped from the stack. Then inc[i-1]+=inc[i], so that we can accumulate the increment inc[i] for the bottom elements and the following pops.

```cpp
class CustomStack {
public:
    
    vector<int> inc;
    stack<int> stk;
    int MAXSIZE;
    
    CustomStack(int maxSize) {
        inc = vector<int> (maxSize+1, 0);
        MAXSIZE = maxSize;
    }
    
    void push(int x) {
        if(stk.size()==MAXSIZE) return;
        stk.push(x);
    }
    
    int pop() {
        if(stk.empty()) return -1;
        
        int x = stk.top();
        int idx = stk.size();
        
        stk.pop();
        
        if(inc[idx]>0){
            x+=inc[idx];
            inc[idx-1] += inc[idx];
            inc[idx] = 0;
        }
        
        return x;
    }
    
    void increment(int k, int val) {

        if(stk.size()>k)
            inc[k] += val;
        else
            inc[stk.size()] += val;
    }
};
```

<br>

### 4. Implement k stacks in a single array
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
