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

#### 3. Valid Parentheses

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

