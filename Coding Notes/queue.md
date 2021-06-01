# Queue

#### 1. Design Circular Queue

```cpp
class MyCircularQueue {
public:
    
    int front, rear;
    int SIZE;
    vector<int> q;
    
    MyCircularQueue(int k) {
        SIZE = k;
        front = -1;
        rear = -1;
        q = vector<int> (SIZE, 0);
    }
    
    bool enQueue(int value) {
        if(isFull()) return false;
        
        rear=(rear+1)%SIZE;
        q[rear] = value;

        if(front==-1) front=0;
        return true;
    }
    
    bool deQueue() {
        if(isEmpty()) return false;
        if(front==rear){
            front=-1; 
            rear=-1;
        }
        else
            front=(front+1)%SIZE;
        return true;
    }
    
    int Front() {
        if(isEmpty()) return -1;
        return q[front];
    }
    
    int Rear() {
        if(isEmpty()) return -1;
        return q[rear];
    }
    
    bool isEmpty() {
        return rear==-1;
    }
    
    bool isFull() {
        return front == (rear+1)%SIZE;
    }
};
```

#### 2. Implement Queue using Stacks

```cpp
stack<int> s1;
stack<int> s2;

MyQueue() {
}

/** Push element x to the back of queue. */
void push(int x) {
    s1.push(x);
}

/** Removes the element from in front of queue and returns that element. */
int pop() {
    if(!s2.empty()){
        int x = s2.top();
        s2.pop();
        return x;
    }
    else{
        if(s1.empty()) return -1;

        while(!s1.empty()){
            s2.push(s1.top());
            s1.pop();
        }

        int x = s2.top();
        s2.pop();
        return x;
    }
}

/** Get the front element. */
int peek() {
    if(!s2.empty()){
        int x = s2.top();
        return x;
    }
    else{
        if(s1.empty()) return -1;

        while(!s1.empty()){
            s2.push(s1.top());
            s1.pop();
        }

        int x = s2.top();
        return x;
    }
}

/** Returns whether the queue is empty. */
bool empty() {
    return s1.empty() && s2.empty();
}
```

#### 4. Reverse Queue using Recursion

```cpp
void reverse(queue<int> & q){
    if(q.empty()) 
        return;
    
    int x = q.front();
    q.pop();
    
    reverse(q);
    q.push(x);
}
```

#### 5. Reverse First K elements of Queue 

Approach:

1. Create an empty stack.

2. One by one dequeue first K items from given queue and push the dequeued items to stack.

3. Enqueue the contents of stack at the back of the queue

4. Dequeue (size-k) elements from the front and enque them one by one to the same queue. 

```cpp
queue<int> modifyQueue(queue<int> q, int k){
    stack<int> stk;
    
    for(int i=0; i<k; i++){
        stk.push(q.front());
        q.pop();
    }
    
    while(!stk.empty()){
        q.push(stk.top());
        stk.pop();
    }
    
    for(int i=0; i<q.size()-k; i++){
        int x = q.front();
        q.pop();
        q.push(x);
    }
    
    return q;
}
```

#### 6. Interleave the first half of the queue with second half

Approach:

1.Push the first half elements of queue to stack.

2.Enqueue back the stack elements.

3.Dequeue the first half elements of the queue and enqueue them back.

4.Again push the first half elements into the stack.

5.Interleave the elements of queue and stack.

```cpp
queue<int> interleave_two_halves(queue<int> & q){
    stack<int> s;
    queue<int> ans;
    int mid = q.size()/2;
    
    for(int i=0; i<mid; i++){
        s.push(q.front());
        q.pop();
    }
    
    while(!s.empty()){
        q.push(s.top());
        s.pop();
    }
    
    for(int i=0; i<mid; i++){
        int x = q.front();
        q.pop();
        q.push(x);
    }
    
    for(int i=0; i<mid; i++){
        s.push(q.front());
        q.pop();
    }
    
    while(!q.empty() && !s.empty()){
        ans.push(s.top()); s.pop();
        ans.push(q.front()); q.pop();
    }
    
    return ans;
}
```
