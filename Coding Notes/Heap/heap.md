# Heap (Priority Queue)

**Maxheap using priority queue**

`priority_queue<int> maxheap;`

**Minheap using priority queue**

`priority_queue<int, vector<int>, greater<int>> minheap;`

<br>

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

#### 3. Top K Frequent Elements
Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.

```cpp
vector<int> topKFrequent(vector<int>& nums, int k) {

    unordered_map<int,int> mp;
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>> > minheap;
    vector<int> res;

    for(int n : nums)
        mp[n]++;

    for(auto p : mp){
        minheap.push({p.second, p.first});

        if(minheap.size()>k)
            minheap.pop();
    }

    while(!minheap.empty()){
        auto p = minheap.top(); minheap.pop();
        res.push_back(p.second);
    }

    return res;
}
```

#### 4. Sort Characters By Frequency
Given a string s, sort it in decreasing order based on the frequency of characters, and return the sorted string.

Input: s = "tree"

Output: "eert"

```cpp
string frequencySort(string s) {
    unordered_map<char, int> mp;
    priority_queue<pair<int,char>> maxheap;
    string res = "";

    for(char ch : s)
        mp[ch]++;

    for(auto p : mp)
        maxheap.push({p.second, p.first});

    while(!maxheap.empty()){
        auto p = maxheap.top(); maxheap.pop();
        for(int i=0; i<p.first; i++)
            res.push_back(p.second);
    }

    return res;
}
```

#### 5. K Closest Points to Origin
Given an array of points where points[i] = [xi, yi] and an integer k, return the k closest points to the origin (0, 0). You may return the answer in any order. 

```
vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {

    priority_queue<pair<int, pair<int,int>>> maxheap;
    vector<vector<int>> res;

    for(auto p : points){
        int x = p[0], y = p[1];
        int dist = x*x + y*y;

        maxheap.push({dist, {x, y}});

        if(maxheap.size()>k)
            maxheap.pop();
    }

    while(!maxheap.empty()){
        auto p = maxheap.top(); maxheap.pop();
        res.push_back({p.second.first, p.second.second});
    }

    return res;
}
```
