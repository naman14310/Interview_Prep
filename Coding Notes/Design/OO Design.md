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

### 2. My Calendar I
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

### 3. Encode and Decode TinyURL

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

### 4. Design Browser History
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

### 5. Design Twitter

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
