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

#### 6. Car Pooling
There is a car with capacity empty seats. The vehicle only drives east. You are given the integer capacity and an array trips where trip[i] = [numPassengersi, fromi, toi] indicates that the ith trip has numPassengersi passengers and the locations to pick them up and drop them off are fromi and toi respectively. The locations are given as the number of kilometers due east from the car's initial location.

Return true if it is possible to pick up and drop off all passengers for all the given trips, or false otherwise.

Input: trips = [[3,2,7],[3,7,9],[8,3,9]], capacity = 11

Output: true

Hint : Use heap to keep track of seat occupancy at each intant. Before giving seats to new passengers, pop all passengers from the heap whose endTime less then startTime of new passengers.

```cpp
static bool comp (vector<int> & v1, vector<int> & v2){
    return v1[1]<=v2[1];
}

bool carPooling(vector<vector<int>>& trips, int capacity) {
    int n = trips.size();
    sort(trips.begin(), trips.end(), comp);

    priority_queue<pair<int, int>, vector<pair<int,int>>, greater<pair<int,int>> > occupied;      // --> pair {end_location, num_passengers}

    for(int i=0; i<n; i++){
        int num_passengers = trips[i][0], start_loc = trips[i][1], end_loc = trips[i][2];

        while(!occupied.empty() and occupied.top().first <= start_loc){
            capacity += occupied.top().second;
            occupied.pop();
        }

        if(capacity < num_passengers) return false;

        occupied.push({end_loc, num_passengers});
        capacity -= num_passengers;
    }

    return true;
}
```
