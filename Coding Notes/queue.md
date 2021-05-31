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

