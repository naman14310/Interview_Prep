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

### 2. Insert Interval
You are given an array of non-overlapping intervals intervals where intervals[i] = [starti, endi] and intervals is sorted in ascending order by starti. You are also given an interval newInterval = [start, end] that represents the start and end of another interval.

Insert newInterval into intervals such that intervals is still sorted in ascending order by starti and intervals still does not have any overlapping intervals (merge overlapping intervals if necessary). Return intervals after the insertion.

Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]

Output: [[1,2],[3,10],[12,16]]

```cpp
vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
    int n = intervals.size();

    /* Boundary Case 1 : If n==0 */

    if(n==0) return {{newInterval}};

    /* Boundary Case 2 : If newInterval is greater then all intervals */

    if(newInterval[0]>intervals.back()[1]){
        intervals.push_back(newInterval);
        return intervals;
    }

    vector<vector<int>> res;
    int i=0;

    while(i<n){

        /* If new_start > curr_end, simply push curr_interval to res and increment i */  

        if(newInterval[0]>intervals[i][1]){
            res.push_back(intervals[i]);
            i++;
        }

        /* 
            Else If new_start <= curr_end and new_end < curr_start, No Overlapping Case
            Hence push newInterval to res and will push others as well after it
        */

        else if (newInterval[1]<intervals[i][0]){
            res.push_back(newInterval);
            break;
        }


        /* 
            Else newInterval is overlapping with currInterval, hence 
            push min(new_start, curr_start) and max(new_end, curr_end) to res
            and apply merge interval logic for others
        */

        else{
            res.push_back({min(newInterval[0], intervals[i][0]), max(newInterval[1], intervals[i][1])});
            i++;
            break;
        }
    }


    /* Simple merge interval logic for remaining intervals */

    while(i<n){

        if(intervals[i][0]<=res.back()[1])
            res.back()[1] = max(res.back()[1], intervals[i][1]);

        else
            res.push_back(intervals[i]);

        i++;
    }

    return res;
}
```

<br>

### 3. Interval List Intersections
You are given two lists of closed intervals, where firstList[i] = [starti, endi] and secondList[j] = [startj, endj]. Each list of intervals is pairwise disjoint and in sorted order. Return the intersection of these two interval lists.

![img](https://assets.leetcode.com/uploads/2019/01/30/interval1.png)

Input: firstList = [[0,2],[5,10],[13,23],[24,25]], secondList = [[1,5],[8,12],[15,24],[25,26]]

Output: [[1,2],[5,5],[8,10],[15,23],[24,24],[25,25]]

```cpp
/* 
    Following are two cases for overlapping:
    1. If end1 lies between start2 -- end2
    2. If end2 lies between start1 -- end1
*/

bool overlap(vector<int> & interval1, vector<int> & interval2){
    int start1 = interval1[0], start2 = interval2[0];
    int end1 = interval1[1], end2 = interval2[1];

    if(end1>=start2 and end1<=end2) return true;
    if(end2>=start1 and end2<=end1) return true;

    return false;
}


vector<vector<int>> intervalIntersection(vector<vector<int>>& firstList, vector<vector<int>>& secondList) {
    int i=0, j=0;
    vector<vector<int>> res;

    while(i<firstList.size() and j<secondList.size()){

        /* If ith and jth intervals overlap then pushback their intersection in res */

        if(overlap(firstList[i], secondList[j])){

            int start_of_intersection = max(firstList[i][0], secondList[j][0]);
            int end_of_intersection = min(firstList[i][1], secondList[j][1]);

            res.push_back({start_of_intersection, end_of_intersection});
        }

        /* If end_i > end_j then increment j,  Else increment i */

        if(firstList[i][1] > secondList[j][1])
            j++;

        else i++;
    }

    return res;
}
```

<br>

### 4. Maximum Length of Pair Chain (IMP)
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

### 5. Non-overlapping Intervals
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

### 6. Maximum Number of Events That Can Be Attended
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

### 7. Minimum Number of Arrows to Burst Balloons
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

### 8. Course Schedule III
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

