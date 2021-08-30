# Problems on Object Oriented Design (OOD)

### 1. Design Twitter

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
