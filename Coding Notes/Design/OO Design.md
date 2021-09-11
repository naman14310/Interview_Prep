# Problems on Object Oriented Design (OOD)


<br>

### 1. Design HashMap

Used Chaining for collision removal

```cpp
class MyHashMap {
public:
        
    vector<list<pair<int,int>>> mp;         // --> vector of linked list of pair {key, values}
    int size = 6151;                        // prime number for uniform distribution
    
    
    MyHashMap() {    
        mp.resize(size);
    }
    
        
    void put(int key, int value) {
        int hash = key % size;
        
        for(auto &itr : mp[hash]){
            if(itr.first==key){
                itr.second = value;
                return;
            }
        }
        
        mp[hash].push_back({key, value});
    }
    
    
    /* Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    
    int get(int key) {
        int hash = key % size;
        
        for(auto &itr : mp[hash]){
            if(itr.first==key)
                return itr.second;
        }
        
        return -1;
    }
    
    
    /* Removes the mapping of the specified value key if this map contains a mapping for the key */
    
    void remove(int key) {
        int hash = key % size;
        
        for(auto &itr : mp[hash]){
            if(itr.first==key){
                mp[hash].remove(itr);
                return;   
            }
        }
    }
};
```

<br>

### 2. Maximum Frequency Stack (Tricky)
Design a stack-like data structure to push elements to the stack and pop the most frequent element from the stack. Implement the FreqStack class:
1. FreqStack() constructs an empty frequency stack.
2. void push(int val) pushes an integer val onto the top of the stack.
3. int pop() removes and returns the most frequent element in the stack.

If there is a tie for the most frequent element, the element closest to the stack's top is removed and returned.

[Explaination](https://www.youtube.com/watch?v=0fRmVjxopiE)

Approach:
1. Create an unordered map of integers (freq) to store the frequency of each element.
2. Create an unordered map of the stack (stk) to store the elements of the same frequency in the same stack. This is for tackling the "frequency tie" condition.
3. Create a variable to keep a note of the current maximum frequency (maxfreq).

```cpp
class FreqStack {
public:
    
    unordered_map<int, stack<int>> stk;        // --> map of {freq, stack of vals}
    unordered_map<int,int> freq;               // --> map of {vals, freq}
    int maxfreq;
    
    FreqStack() {
        maxfreq = 0;
    }
    
    
    void push(int val) {
        freq[val]++;
        
        if(freq[val]>maxfreq) 
            maxfreq = freq[val];
        
        stk[freq[val]].push(val);
    }
    
    
    int pop() {
        int val = stk[maxfreq].top();
        stk[maxfreq].pop();
        
        if(stk[maxfreq].empty())
            maxfreq--;
        
        freq[val]--;
        return val;
    }
};
```

<br>

### 3. LRU Cache
Design a data structure that follows the constraints of a Least Recently Used (LRU) cache. Implement the LRUCache class:
1. LRUCache(int capacity) Initialize the LRU cache with positive size capacity.
2. int get(int key) Return the value of the key if the key exists, otherwise return -1.
3. void put(int key, int value) Update the value of the key if the key exists. Otherwise, add the key-value pair to the cache. If the number of keys exceeds the capacity from this operation, evict the least recently used key.

The functions get and put must each run in O(1) average time complexity.

```cpp
class LRUCache {
public:
    
    unordered_map<int, list<pair<int,int>>::iterator> mp;
    list<pair<int,int>> cache;
    int freespace;
    
    
    LRUCache(int capacity) {
        freespace = capacity;
    }
    
    
    int get(int key) {
        if(mp.find(key)==mp.end()) 
            return -1;
        
        /* erase node from curr pos and append it to front */
        
        auto ptr = mp[key];
        int val = (*ptr).second;
        
        cache.erase(ptr);
        cache.push_front({key,val});
        
        mp[key] = cache.begin();
        return val;
    }
    
    
    void put(int key, int value) {
        
        if(mp.find(key)!=mp.end()){
            
            /* erase node from curr pos */
            
            auto ptr = mp[key];
            cache.erase(ptr);
            freespace++;
        }
        else if(freespace==0){
                
            /* erase last node of linked_list */

            int erased_key = cache.back().first;
            mp.erase(erased_key);
            cache.pop_back();
            freespace++;
        }
        
        /* insert node node at front of linked list */
        
        cache.push_front({key, value});
        mp[key] = cache.begin();
        freespace--;
    }
};
```

<br>

### 4. LFU Cache (Tricky)
Design and implement a data structure for a Least Frequently Used (LFU) cache. Implement the LFUCache class:
1. int get(int key) Gets the value of the key if the key exists in the cache. Otherwise, returns -1.
2. void put(int key, int value) Update the value of the key if present, or inserts the key if not already present. When the cache reaches its capacity, it should invalidate and remove the least frequently used key before inserting a new item. For this problem, when there is a tie (i.e., two or more keys with the same frequency), the least recently used key would be invalidated.

The freq of key in the cache is incremented either a get or put operation is called on it. The functions get and put must each run in O(1) average time complexity.

```cpp
class LFUCache {
public:
    
    /*
          Data Structure Visualization:
    
    
          Cache (map of {freq, list of pair {key, val}})
          --------------------------------------------
            
           freq             Linked lists containing elements having freq equals to key 
                            arranged in manner such that most recent will be at front.
                            
            1       -       {key,value} --> {key, value}
            
            2       -       {key,value} --> {key,value} --> {key,value} 
            
            3       -       {key,value} --> {key,value}   <---------------------------------------
                                                                                                 |
            4       -       {key,value}                                                          |
                                                                                                 |
                                                                                                 |
                                                                                                 |    Iterators gives us the
          mp (map of {key, pair {freq, Iterator to listnode})                                    |    location of key in cache
          --------------------------------------------------                                     |    so that we can easily 
                                                                                                 |    erase or shift nodes in  
           key              Pair containing freq of key and a pointer to its node in Cache       |    in cache in O(1) time.
                                                                                                 |
            1       -       {freq, Iterator} ____________________________________________________| 
            
            2       -       {freq, Iterator}
            
            3       -       {freq, Iterator}
            
    */
    
    
    unordered_map<int, pair<int, list<pair<int,int>>::iterator> > mp;       
    unordered_map<int, list<pair<int,int>> >  cache;                     
    
    int minfreq;            // --> will keep track of min freq so far
    int freespace;          // --> will keep track of free spaces so far
    
    
    LFUCache(int capacity) {
        freespace = capacity;
        minfreq = 1;
    }
    
    
    int get(int key) {
        if(mp.find(key)==mp.end()) return -1;
        
        /* 
            1. erase curr node from cache[curr_freq] 
            2. increase its freq and append it to the front of cache [new_freq]
            3. update freq and iterator in map[key]
            4. increment minfreq var, if erased key was the only lowest freq key
        */
        
        int freq = mp[key].first;
        auto itr = mp[key].second;
        int val = (*itr).second;
        
        cache[freq].erase(itr);
        
        freq++;
        cache[freq].push_front({key, val});
        
        mp[key] = {freq, cache[freq].begin()};
        
        if(cache[minfreq].size()==0)
            minfreq++;
        
        return val;
    }
    
    
    void put(int key, int value) {
        
        /* 
            If key is already present then:
            
            1. erase curr node from cache[curr_freq] 
            2. increase its freq and append it to the front of cache[new_freq] with new val
            3. update freq and iterator in map[key]
            4. increment minfreq var, if erased key was the only lowest freq key
        */
        
        if(mp.find(key)!=mp.end()){
            
            int freq = mp[key].first;
            auto itr = mp[key].second;
            
            cache[freq].erase(itr);
            
            freq++;
            cache[freq].push_front({key, value});
        
            mp[key] = {freq, cache[freq].begin()};
            
            if(cache[minfreq].size()==0)
                minfreq++;
        }
        
        else{
            
            /* 
                Else if cache is full then:
                1. fetch key of last node of cache[minfreq] and erase last node
                2. erase that key from mp 
                3. append new node at front of cache[1], Since it is new node so eventaully its freq will be 1
                4. insert new entry in mp too
            */
            
            if (freespace==0){
                if(cache[minfreq].size()==0) return;
                
                int erased_key = cache[minfreq].back().first;
                
                cache[minfreq].pop_back();
                mp.erase(erased_key);
                
                cache[1].push_front({key, value});
                mp[key] = {1, cache[1].begin()};
            }
            
            /* Else simply append new node in cache[1] and insert new entry in mp. Decrement the free space */
            
            else{
                cache[1].push_front({key, value});
                mp[key] = {1, cache[1].begin()};
                freespace--;
            }
            
            minfreq = 1;
        }
        
    }
};
```

<br>

### 5. My Calendar I
You are implementing a program to use as your calendar. We can add a new event if adding the event will not cause a double booking. The event can be represented as a pair of integers start and end. Implement the MyCalendar class:

1. MyCalendar() Initializes the calendar object.
2. boolean book(int start, int end) Returns true if the event can be added to the calendar successfully without causing a double booking.

Hint: Use set to store all bookings in sorted order and use binary search.

```cpp
class MyCalendar {
public:
    set<pair<int,int>> s;
    
    MyCalendar() {
    }
    
    bool book(int start, int end) {
        
        if(s.empty()){
            s.insert({start, end});
            return true;
        }
        
        auto next = s.lower_bound({start, end});
        
        if(next==s.end()){
            if((--next)->second<=start){
                s.insert({start, end});
                return true;
            }
            else return false;
        }
        else if(next==s.begin()){
            if(next->first>=end){
                s.insert({start, end});
                return true;
            }
            else return false;
        }
        
        else if(end<=next->first and start>=(--next)->second){
            s.insert({start,end});
            return true;
        }
        
        else return false;
            
    }
};
```

<br>

### 6. Encode and Decode TinyURL

```cpp
class Solution {
public:

    unordered_map<string, string> long2short;
    unordered_map<string, string> short2long;
    
    string pattern = "012345689abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    int counter = 0;
    
    
    int getToken(){
        return counter;
    }
    
    
    string encode(string long_url) {
        int token = getToken();
        string short_url = "http://tinyurl.com/";
        
        while(token>0){
            short_url = to_string(pattern[token%62]) + short_url;
            token = token/62;
        }
        
        short_url = to_string(pattern[token%62]) + short_url;
        
        long2short[long_url] = short_url;
        short2long[short_url] = long_url;
        
        counter++;
        return short_url;
    }

 
    string decode(string short_url) {
        return short2long[short_url];
    }
};
```

<br>

### 7. Design Browser History
You have a browser where you start on the homepage and you can visit another url, get back in the history number of steps or move forward in the history number of steps. Implement the BrowserHistory class:
1. BrowserHistory(string homepage) Initializes the object with the homepage of the browser.
2. void visit(string url) Visits url from the current page. It clears up all the forward history.
3. string back(int steps) Move steps back in history. If you can only return x steps in the history and steps > x, you will return only x steps. Return the current url after moving back in history at most steps.
4. string forward(int steps) Move steps forward in history. If you can only forward x steps in the history and steps > x, you will forward only x steps. Return the current url after forwarding in history at most steps.

Hint: use two stacks

![img](https://assets.leetcode.com/users/interviewrecipes/image_1591684752.png)

```cpp
class BrowserHistory {
public:
    
    stack<string> bckstk;
    stack<string> fwdstk;
    
    BrowserHistory(string homepage) {
        bckstk.push(homepage);
    }
    
    void visit(string url) {
        bckstk.push(url);
        fwdstk = stack<string>();
    }
    
    string back(int steps) {
        
        while(bckstk.size()>1 and steps>0){
            fwdstk.push(bckstk.top());
            bckstk.pop();
            steps--;
        }
        
        return bckstk.top();
    }
    
    string forward(int steps) {
        
        while(fwdstk.size()>0 and steps>0){
            bckstk.push(fwdstk.top());
            fwdstk.pop();
            steps--;
        }
        
        return bckstk.top();
    }
};
```

<br>

### 8. Design Twitter

Design a simplified version of Twitter where users can post tweets, follow/unfollow another user, and is able to see the 10 most recent tweets in the user's news feed. Implement the Twitter class:

1. Twitter() Initializes your twitter object.
2. void postTweet(int userId, int tweetId) Composes a new tweet with ID tweetId by the user userId. Each call to this function will be made with a unique tweetId.
3. List<Integer> getNewsFeed(int userId) Retrieves the 10 most recent tweet IDs in the user's news feed. Each item in the news feed must be posted by users who the 4. user followed or by the user themself. Tweets must be ordered from most recent to least recent.
5. void follow(int followerId, int followeeId) The user with ID followerId started following the user with ID followeeId.
6. void unfollow(int followerId, int followeeId) The user with ID followerId started unfollowing the user with ID followeeId.

```cpp
class Twitter {
public:
    
    unordered_map<int, vector<pair<int,int>> > tweets;          // --> map of { userId, vector of ({time, tweetId}) }
    unordered_map<int, set<int>> followList;                    // --> map of {userId, set of followers}
    int time;                                                   // --> timestamp
    
    
    Twitter() {
        time = 0;
    }
    

    void postTweet(int userId, int tweetId) {
        tweets[userId].push_back({time, tweetId});
        time++;
    }
    

    vector<int> getNewsFeed(int userId) {
        vector<int> t;
        priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>> > minheap;
        
        /* Since user is follower of himself hence we will push his tweets in minheap */
        
        for(auto tweet : tweets[userId]){
            minheap.push(tweet);
            
            if(minheap.size()>10)
                minheap.pop();
        }
        
        /* pushing followee's tweets in minheap */
        
        for(int flw : followList[userId]){
            
            for(auto tweet : tweets[flw]){
                minheap.push(tweet);

                if(minheap.size()>10)
                    minheap.pop();
            }
        }
        
        /* pop recent 10 tweets */
        
        while(!minheap.empty()){
            t.push_back(minheap.top().second);
            minheap.pop();
        }
        
        reverse(t.begin(), t.end());
        return t;
    }
    
    
    void follow(int followerId, int followeeId) {
        followList[followerId].insert(followeeId);              // --> logn time to insert
    }
    
    
    void unfollow(int followerId, int followeeId) {
        followList[followerId].erase(followeeId);               // --> logn time to erase
    }
    
};
```
