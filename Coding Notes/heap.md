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
