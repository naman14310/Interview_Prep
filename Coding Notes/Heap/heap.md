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
You are given the integer capacity and an array trips where trip[i] = [numPassengersi, fromi, toi] indicates that the ith trip has numPassengersi passengers and the locations to pick them up and drop them off are fromi and toi respectively. The locations are given as the number of kilometers due east from the car's initial location.

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

#### 7. Seat Reservation Manager
Design a system that manages the reservation state of n seats that are numbered from 1 to n. Implement the SeatManager class:
1. SeatManager(int n) Initializes a SeatManager object that will manage n seats numbered from 1 to n. All seats are initially available.
2. int reserve() Fetches the smallest-numbered unreserved seat, reserves it, and returns its number.
3. void unreserve(int seatNumber) Unreserves the seat with the given seatNumber.

```cpp
priority_queue<int, vector<int>, greater<int>> minheap;

SeatManager(int n) {
    for(int i=1; i<=n; i++)
        minheap.push(i);
}

int reserve() {
    int seat = minheap.top();
    minheap.pop();
    
    return seat;
}

void unreserve(int seatNumber) {
    minheap.push(seatNumber);
}
```

#### 8. Longest Happy String
A string is called happy if it does not have any of the strings 'aaa', 'bbb' or 'ccc' as a substring. Given three integers a, b and c, return any string s, which satisfies following conditions:

1. s is happy and longest possible.
2. s contains at most a occurrences of the letter 'a', at most b occurrences of the letter 'b' and at most c occurrences of the letter 'c'.
3. s will only contain 'a', 'b' and 'c' letters.
4. If there is no such string s return the empty string "".

Input: a = 1, b = 1, c = 7

Output: "ccaccbcc"

Hint: Use most frequent char at every instant which satisfies the condition.

```cpp
string longestDiverseString(int a, int b, int c) {

    priority_queue<pair<int,char>> maxheap;

    if(a>0) maxheap.push({a, 'a'});
    if(b>0) maxheap.push({b, 'b'});
    if(c>0) maxheap.push({c, 'c'});

    char prev = '#';
    int cnt = 0;
    string s = "";

    while(!maxheap.empty()){
        auto p = maxheap.top(); maxheap.pop();

        if(p.second==prev) {
            cnt++;
            if(cnt==3){
                if(maxheap.empty()) return s;

                auto p2 = maxheap.top(); maxheap.pop();
                s.push_back(p2.second);
                prev = p2.second;
                cnt = 1;

                if(p2.first>1) maxheap.push({p2.first-1, p2.second});   
                maxheap.push(p);
            }
            else{
                s.push_back(p.second);
                if(p.first>1) maxheap.push({p.first-1, p.second});   
            }
        }
        
        else{
            s.push_back(p.second);
            prev = p.second;
            cnt = 1;

            if(p.first>1) maxheap.push({p.first-1, p.second});   
        }
    }

    return s;
}
```

#### 9. Top K Frequent Words
Given an array of strings words and an integer k, return the k most frequent strings. Return the answer sorted by the frequency from highest to lowest. Sort the words with the same frequency by their lexicographical order.

Input: words = ["the","day","is","sunny","the","the","the","sunny","is","is"], k = 4

Output: ["the","is","sunny","day"]

```cpp
/* 
    user defined comparator for set and priority queues will be defined like this 
    REMEMBER : Use opposite sign
*/

struct comp{
  bool operator()(pair<int, string> p1, pair<int, string> p2){
      if(p1.first>p2.first) return true;         
      if(p1.first==p2.first and p1.second < p2.second) return true;
      return false;
  }  
};


vector<string> topKFrequent(vector<string>& words, int k) {
    unordered_map<string, int> mp;
    priority_queue<pair<int, string>, vector<pair<int, string>>, comp> minheap;
    vector<string> res;

    for(string s : words)
        mp[s]++;

    for(auto p : mp){
        minheap.push({p.second, p.first});

        if(minheap.size()>k)
            minheap.pop();
    }

    while(!minheap.empty()){
        res.push_back(minheap.top().second);
        minheap.pop();
    }

    reverse(res.begin(), res.end());
    return res;
}
```

#### 10. Task Scheduler (Tricky)
Given a characters array tasks, representing the tasks a CPU needs to do, where each letter represents a different task. Tasks could be done in any order. Each task is done in one unit of time. For each unit of time, the CPU could complete either one task or just be idle. However, there is a non-negative integer n that represents the cooldown period between two same tasks. Return the least number of units of times that the CPU will take to finish all the given tasks.

Hint: Use Two heaps, one to fetch task with max freq and other for keeping track of forbidden tasks till cooldown period

```cpp
int leastInterval(vector<char>& tasks, int cooldown_period) {

    unordered_map<char, int> mp;    // --> for storing frequency of each task

    /* maxheap will contain task sorted by their freq -->  pair of {freq, char} */

    priority_queue<pair<int, char>> maxheap;  

    /* minheap will store forbidden tasks till cooldown period --> pair of {prev_time, char} */

    priority_queue<pair<int, char>, vector<pair<int, char>>, greater<pair<int, char>> > minheap;       


    for(char ch : tasks)
        mp[ch]++;

    for(auto p : mp)
        maxheap.push({p.second, p.first});

    int time = 0;

    while(!maxheap.empty() or !minheap.empty()){

        /* pop all tasks from minheap whose cooldown period gets over and push them to maxheap */

        while(!minheap.empty() and minheap.top().first+cooldown_period<time){
            auto ch = minheap.top().second; minheap.pop();
            maxheap.push({mp[ch], ch});
        }


        /* pop one task with highest freq from maxheap and allocate to cpu */

        if(!maxheap.empty()){
            auto task = maxheap.top(); maxheap.pop();
            mp[task.second]--;

            /* If freq of curr task > 0 push it into minheap for cooldown period */

            if(mp[task.second]>0)
                minheap.push({time, task.second});
        }

        time++;
    }

    return time;
}
```
