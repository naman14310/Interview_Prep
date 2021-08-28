# Heap (Priority Queue)

**Maxheap using priority queue**

`priority_queue<int> maxheap;`

**Minheap using priority queue**

`priority_queue<int, vector<int>, greater<int>> minheap;`


#### 1. Sort a nearly sorted (or K sorted) array
Given an array of n elements, where each element is at most k away from its target position. Sort the array.

```cpp
vector<int> sort_k_sorted_array(vector<int> & v, int k){
    vector<int> res;
    priority_queue<int, vector<int>, greater<int>> minheap;
    int i=0;
    
    for( ; i<k-1; i++)
        minheap.push(v[i]);

    for( ; i<v.size(); i++){
        minheap.push(v[i]);
        res.push_back(minheap.top());
        minheap.pop();
    }
    
    while(!minheap.empty()){
        res.push_back(minheap.top());
        minheap.pop();
    }
    
    return res;
}
```

#### 2. Last Stone Weight
You are given an array of integers stones where stones[i] is the weight of the ith stone. On each turn, we choose the heaviest two stones and smash them together. 
1. If x == y, both stones are destroyed, and
2. If x != y, the stone of weight x is destroyed, and the stone of weight y has new weight y - x.

At the end of the game, there is at most one stone left. Return the weight of the left stone. If there are no stones left, return 0.

```cpp
int lastStoneWeight(vector<int>& stones) {
    priority_queue<int> maxHeap;

    for(int s : stones)
        maxHeap.push(s);

    while(maxHeap.size()>1){
        int s1 = maxHeap.top(); maxHeap.pop();
        int s2 = maxHeap.top(); maxHeap.pop();

        if(s1!=s2)
            maxHeap.push(abs(s1-s2));
    }

    if(maxHeap.empty()) return 0;
    return maxHeap.top();
}
```
