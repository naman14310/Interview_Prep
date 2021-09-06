# Problems on Intervals

<br>

### 1. Merge Intervals
Given an array of intervals where intervals[i] = [starti, endi], merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

Input: intervals = [[1,3],[2,6],[8,10],[15,18]]

Output: [[1,6],[8,10],[15,18]]

Hint: Sort all intervals by starttime.

```cpp
vector<vector<int>> merge(vector<vector<int>>& intervals) {
    int n = intervals.size();
    sort(intervals.begin(), intervals.end());       // --> sort by start

    vector<vector<int>> res;
    res.push_back(intervals[0]);

    for(int i=1; i<n; i++){
        
        /* If intervals are overlapping, update prev_end to max(prev_end, curr_end) */
        
        if(intervals[i][0]<=res.back()[1])
            res.back()[1] = max(res.back()[1], intervals[i][1]);
        
        /* Else pushback new interval to res vector */
        
        else
            res.push_back(intervals[i]);
    }

    return res;
}
```

<br>

### 1. Maximum Length of Pair Chain (IMP)
You are given an array of n pairs pairs where pairs[i] = [lefti, righti] and lefti < righti. A pair p2 = [c, d] follows a pair p1 = [a, b] if b < c. A chain of pairs can be formed in this fashion. Return the length longest chain which can be formed.

Input: pairs = [[1,2],[2,3],[3,4]]

Output: 2

Hint: Sort by pairs.second in ascending order

```cpp
static bool comp (vector<int> & v1, vector<int> & v2){
    return v1[1]<v2[1];
}


int findLongestChain(vector<vector<int>>& pairs) {
    int n = pairs.size();
    sort(pairs.begin(), pairs.end(), comp);     // --> Sort on the basis of pair[1] in ascending order

    int prev = pairs[0][1];
    int cnt = 1;

    for(int i=1; i<n; i++){

        if(pairs[i][0]>prev){
            cnt++;
            prev = pairs[i][1];
        }

    }

    return cnt;
}
```

<br>

### 2. Non-overlapping Intervals
Given an array of intervals intervals where intervals[i] = [starti, endi], return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

Input: intervals = [[1,2],[2,3],[3,4],[1,3]]

Output: 1

Hint: Minimum remove for overlapping interval = n - longest_chain_pair_len (above question)

```cpp
static bool comp (vector<int> & v1, vector<int> & v2){
    return v1[1]<v2[1];
}


int eraseOverlapIntervals(vector<vector<int>>& intervals) {
    int n = intervals.size();

    sort(intervals.begin(), intervals.end(), comp);

    int prev = intervals[0][1];
    int longest_chain_len = 1;

    for(int i=1; i<n; i++){

        if(intervals[i][0]>=prev){
            prev = intervals[i][1];
            longest_chain_len++;
        }    
    }

    return n - longest_chain_len;
}
```

<br>

### 3. Maximum Number of Events That Can Be Attended
Given an array of events where events[i] = [startDayi, endDayi].  You can attend an event i at any day d where startTimei <= d <= endTimei. You can only attend one event at any time d. Return the maximum number of events you can attend.

Input: events = [[1,4],[4,4],[2,2],[3,4],[1,1]]

Output: 4

Hint: Use minheap for getting events of lowest deadline. Iterate for all days and process all events for that day.

```cpp
static bool comp (vector<int> & v1, vector<int> & v2){
    return v1[0]<v2[0];
}


int maxEvents(vector<vector<int>>& events) {
    int n = events.size();
    priority_queue<int, vector<int>, greater<int>> minheap;     // --> contains endday

    sort(events.begin(), events.end(), comp);                   // --> sort events by startTime

    int i=0;
    int attended = 0;

    for(int d=1; d<=100000; d++){

        /* remove all past events from minheap */

        while(!minheap.empty() and minheap.top()<d)
            minheap.pop();

        /* push all events to minheap that start on curr day */

        while(i<n and events[i][0]==d){
            minheap.push(events[i][1]);
            i++;
        }

        /* schedule one event for curr day */

        if(!minheap.empty()){
            minheap.pop();
            attended++;
        }

        if(i==n and minheap.empty()) break;
    }

    return attended;
}
```

<br>

### 4. Minimum Number of Arrows to Burst Balloons
There are some spherical balloons spread in two-dimensional space. For each balloon, provided input is the start and end coordinates of the horizontal diameter. 
An arrow can be shot up from different points along the x-axis. A balloon with xstart and xend bursts by an arrow shot at x if xstart ≤ x ≤ xend. Given an array points where points[i] = [xstart, xend], return the minimum number of arrows that must be shot to burst all balloons.

Input: points = [[10,16],[2,8],[1,6],[7,12]]

Output: 2

Hint: 
1. Sort all points by end in ascending order. 
2. Then start iterate from left and mark range as endPoint[i]
3. Burst all balloons ahead it, which lies under this range and increment arrow by one.
4. Repeat this process by increment i to first unbursted balloon.

```cpp
static bool comp (vector<int> & v1, vector<int> & v2){
    return v1[1]<v2[1];
}

int findMinArrowShots(vector<vector<int>>& points) {
    int n = points.size();
    sort(points.begin(), points.end(), comp);        // --> sort based on end in ascending order

    int i=0;
    int arrow = 0;

    while(i<n){
        int range = points[i][1];

        int j=i+1;

        while(j<n and points[j][0]<=range)
            j++;

        arrow++;
        i = j;
    }

    return arrow;
}
```

<br>

### 5. Course Schedule III
You are given an array courses where courses[i] = [durationi, lastDayi] indicate that the ith course should be taken continuously for durationi days and must be finished before or on lastDayi. You will start on the 1st day and you cannot take two or more courses simultaneously. Return the maximum number of courses that you can take.

Input: courses = [[100,200],[200,1300],[1000,1250],[2000,3200]]

Output: 3

Hint: 
1. Sort all courses by their lastDay. Use one maxheap which will contain duration of all courses taken so far.
2. Initiate timestamp to 0.
3. Iterate for all courses, If curr_course will start after timestamp, push it in maxheap and update timestamp.
4. Else If its duration is less then any course taken earlier (will be present on top of maxheap), then replace that with curr_course.

```cpp
static bool comp (pair<int,int> p1, pair<int,int> p2){
    return p1.second < p2.second;
}


int scheduleCourse(vector<vector<int>>& courses) {

    vector<pair<int,int>> v;
    for(auto c : courses)
        v.push_back({c[0], c[1]});

    sort(v.begin(), v.end(), comp);         // --> sort all courses based on their lastDay

    priority_queue<int> maxheap;            // --> will store duration of courses taken so far
    int timeStamp = 0;

    /* Iterate for all courses */

    for(int i=0; i<v.size(); i++){
        int duration = v[i].first, lastDay = v[i].second;

        /* If we can take curr course without any clash then push it in maxheap and update timeStamp */

        if(timeStamp + duration <= lastDay){
            maxheap.push(duration);
            timeStamp += duration;
        }

        /* 
            Else if we've taken any course earlier whose duration is greater then curr_course
            then replacing curr_course with that earlier course of higher duration (will be on the top of maxheap)
            will be more beneficial for us.
        */

        else if (!maxheap.empty() and maxheap.top() > duration){
            timeStamp = timeStamp - maxheap.top() + duration;
            maxheap.pop();
            maxheap.push(duration);
        }
    }

    return maxheap.size();
}
```

